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

## Tips

- Input contexts to test: query filters, `$where` clauses, `$regex`, `$gt/$lt`.
- Use JSON-oriented payloads (`{"$ne": null}` or `{"$gt": ""}`).
- Escape or validate JSON structures before execution.
- In blind challenges, look for time-based payloads using heavy operations.
