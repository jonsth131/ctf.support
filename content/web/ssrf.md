---
title: "Server-Side Request Forgery (SSRF)"
description: "Exploit vulnerable servers to make unauthorized requests to internal or external systems on behalf of the target."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["ssrf", "server request forgery", "web exploitation", "ctf"]
authors: ["CTF.Support Team"]
summary: "Use SSRF to access internal services, scan ports, or extract local resources through unsanitized fetch requests."
aliases: ["/web/ssrf/"]
slug: "ssrf"
toc: true
---

## Introduction

SSRF allows an attacker to force a vulnerable server to perform HTTP requests to internal resources or remote endpoints.
This can bypass firewalls, access metadata, or exfiltrate secrets.

## Example (Python)

```python
import requests

def fetch_url(url):
    response = requests.get(url)
    return response.text

url = input('Enter a URL: ')
print(fetch_url(url))
```

Exploit input:

```text
http://localhost/admin
```

The server requests the internal admin page and returns it to the attacker.

## Tips

- Try localhost targets like `http://127.0.0.1`, `http://169.254.169.254`, or `file:///etc/passwd`.
- Detect SSRF via DNS rebinding or external collaborator interactions.
- Encode payloads or use redirects for blind SSRF exploitation.
- Useful tools: `interactsh`, `dnslog.cn`, `ngrok` for callback confirmation.
