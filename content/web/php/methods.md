---
title: "Methods"
description: "Understand common PHP functions and limitations that can lead to vulnerabilities or bypasses."
date: 2025-10-06
draft: false
weight: 10
categories: ["Web"]
tags: ["php", "password_hash", "bcrypt", "security"]
authors: ["CTF.Support Team"]
summary: "Some PHP core functions have weaknesses or limitations exploitable in CTF contexts, such as length limits in password hashing."
aliases: ["/web/php/methods/"]
slug: "methods"
toc: true
---

## password_hash()

When using `PASSWORD_BCRYPT` (default since PHP 5.5.0), passwords are limited to **72 bytes**.
Anything longer is silently truncated, this can weaken brute-force complexity or comparison logic.

Example behavior:

```php
var_dump(password_hash(str_repeat("A",80), PASSWORD_BCRYPT));
```

This will return the hash for 72 A's.
