---
title: SQL Injection
---

SQL injection is a vulnerability that allows an attacker to execute arbitrary SQL code on a server. This can be used to read, modify, or delete data from a database, or even to execute commands on the underlying operating system.

For example, consider a web application that uses a SQL database to store user information. The application might construct SQL queries by concatenating user input, like so:

```python
import sqlite3

def get_user(username):
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE username = '{username}'"
    cursor.execute(query)
    return cursor.fetchall()

username = input('Enter a username: ')
print(get_user(username))
```

If the application does not properly sanitize the input, an attacker could provide a username like `' OR 1=1 --` and the resulting query would be:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```
