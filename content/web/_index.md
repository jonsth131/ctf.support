---
title: "Web Exploitation"
description: "Comprehensive reference for web vulnerabilities and exploitation techniques used in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["web", "exploitation", "vulnerabilities", "ctf web", "pentesting"]
authors: ["CTF.Support Team"]
summary: "Explore common web vulnerabilities and exploitation methods, from injections and misconfigurations to language‑specific attacks, used in Capture The Flag challenges."
aliases: ["/web/"]
slug: "web"
toc: false
---

## Introduction

Web exploitation in **CTF challenges** involves finding and abusing weaknesses in how web applications handle user input, authentication, or file access.
This section covers the **most common attack surfaces**, how to analyze them efficiently, and which tools to use in a competition setting.

{{< toc >}}

## Categories & Techniques

### General

- **[General](general/)**: reconnaissance and discovery tools for finding directories, sensitive files, and exposed configurations.

---

### Injections

- **[Code Injection](code-injection/)**: execute arbitrary code via vulnerable functions like `eval` or `exec`.
- **[Command Injection](command-injection/)**: run OS-level commands through unsanitized system calls.
- **[SQL Injection](sql-injection/)**: manipulate SQL queries to retrieve or modify database entries.
- **[NoSQL Injection](nosql-injection/)**: exploit JSON‑based databases (e.g., MongoDB) to bypass filters.
- **[Server‑Side Template Injection (SSTI)](ssti/)**: abuse template engines (Jinja2, Twig) to execute code.

---

### Logic & Access Flaws

- **[IDOR (Insecure Direct Object Reference)](idor/)**: manipulate identifiers to access unauthorized data.
- **[SSRF (Server‑Side Request Forgery)](ssrf/)**: force a server to make internal requests on behalf of the user.
- **[XXE (XML External Entity)](xxe/)**: read files or cause server requests via crafted XML entities.
- **[GraphQL](graphql/)**: exploit introspection or poorly secured API queries to extract sensitive data.

---

### Client‑Side Attacks

- **[XSS (Cross‑Site Scripting)](xss/)**: inject JavaScript into web pages for client‑side code execution.
- **[JavaScript](javascript/)**: analyze and deobfuscate JavaScript for client‑side validation, encoding, or prototype pollution vulnerabilities.

---

### File & Inclusion Vulnerabilities

- **[LFI (Local File Inclusion)](lfi/)**: read or inject local files via unsanitized include paths.
- **[PHP](php/)**: explore common PHP language flaws such as type juggling, weak comparisons, and unsafe methods.
- **[Python](python/)**: exploit Flask cookie signatures or insecure pickle deserialization.

---

## Related Tools

| Tool                                       | Purpose                                |
|--------------------------------------------|----------------------------------------|
| [Burp Suite](https://portswigger.net/burp) | Intercept and modify HTTP/S requests   |
| [OWASP ZAP](https://www.zaproxy.org/)      | Open‑source web proxy and scanner      |
| [Caido](https://www.caido.io/)             | Modern proxy alternative with clean UI |

---

## Tips for Web CTF Challenges

- Start with **enumeration**: identify technologies, directories, and hidden parameters.
- Review **public files** (e.g., `robots.txt`, `.git/`, `.env`).
- Use **proxy tools** (Burp, Caido, ZAP) to observe request/response patterns.
