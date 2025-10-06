---
title: "IDOR (Insecure Direct Object Reference)"
description: "Exploit broken access control vulnerabilities by manipulating object identifiers in web applications."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["idor", "access control", "authorization", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn to identify and exploit broken object reference vulnerabilities to access unauthorized data."
aliases: ["/web/idor/"]
slug: "idor"
toc: true
---

## Introduction

**IDOR** vulnerabilities occur when direct object references (like IDs) are exposed without proper authorization checks.  
These weaknesses often provide access to other users' data in CTF web challenges.

## Example

```text
https://example.com/profile?id=123
```

Change the parameter:

```text
https://example.com/profile?id=456
```

## Tips

- Enumerate sequential IDs and UUIDs.  
- Look for exposed numeric parameters (`id`, `uid`, `order_id`).  
- Combine with **authorization tokens** or cookies to confirm privilege checks.  
- Monitor API responses and status codes for unauthorized access hints.
