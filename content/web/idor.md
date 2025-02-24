---
title: IDOR
---

IDOR (Insecure Direct Object Reference) is a vulnerability that occurs when an application provides direct access to objects based on user-supplied input. As a result, an attacker can manipulate the input and access unauthorized data.

For example, consider the following URL:

```
https://example.com/profile?id=123
```

An attacker can change the `id` parameter to access other users' profiles:

```
https://example.com/profile?id=456
```

### Exploitation

To exploit IDOR, follow these steps:

1. Identify the vulnerable parameter.
2. Change the parameter value to access unauthorized data.
3. Analyze the response to confirm the vulnerability.
