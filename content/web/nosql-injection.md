---
title: NoSQL Injection
---

NoSQL injection is a vulnerability that allows an attacker to manipulate NoSQL queries in a web application. This can be used to read, modify, or delete data from a database, or even to execute commands on the underlying operating system.

For example, consider a web application that uses a NoSQL database to store user information. The application might construct NoSQL queries by concatenating user input, like so:

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

If the application does not properly sanitize the input, an attacker could provide a username like `{"$ne": null}` and the resulting query would be:

```json
{ "username": {"$ne": null} }
```

This query would return all users where the `username` field is not `null`, potentially exposing sensitive information.
