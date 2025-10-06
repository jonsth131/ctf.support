---
title: "Cross-Site Scripting (XSS)"
description: "Exploit reflected, stored, or DOM-based XSS vulnerabilities to execute JavaScript in the victim’s browser during CTF challenges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["xss", "cross-site scripting", "web", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn XSS techniques to inject JavaScript payloads through input fields, parameters, or stored content to exfiltrate data or trigger flags."
aliases: ["/web/xss/"]
slug: "xss"
toc: true
---

## Introduction

**Cross-Site Scripting (XSS)** vulnerabilities allow attackers to inject JavaScript into a webpage viewed by other users.  
They commonly appear in parameters, forms, or stored data without proper output sanitization.

## Types

| Type          | Description                                                                      |
|---------------|----------------------------------------------------------------------------------|
| **Reflected** | Payload included in the request and immediately reflected back to the user.      |
| **Stored**    | Payload stored permanently on the server (e.g., in a database).                  |
| **DOM-Based** | Payload executed in the browser without server interaction by modifying the DOM. |

## Quick Reference

| Vector             | Example Payload                   |
|--------------------|-----------------------------------|
| HTML Attribute     | `"><script>alert(1)</script>`     |
| Event Handler      | `<img src=x onerror=alert(1)>`    |
| JavaScript Context | `');alert(1);//`                  |
| URL Payload        | `/search?q=<svg/onload=alert(1)>` |

## Tips

- Use the browser’s developer console to observe reflected parameters.  
- Encode payloads for bypass: `&lt;script&gt;alert(1)&lt;/script&gt;`  
- Bypass filters with alternate encodings or event handlers (`onfocus`, `onmouseover`).  
