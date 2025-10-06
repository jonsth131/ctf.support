---
title: "JavaScript"
description: "Analyze, deobfuscate, and exploit JavaScript code in web CTF challenges, focusing on client‑side logic and vulnerable packages."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["javascript", "web exploitation", "deobfuscation", "snyk", "prototype pollution"]
authors: ["CTF.Support Team"]
summary: "Techniques for reversing or exploiting JavaScript in CTFs, deobfuscating logic, analyzing dependencies, and identifying vulnerable packages."
aliases: ["/web/javascript/"]
slug: "javascript"
toc: true
---

## Introduction

Many CTF web challenges rely on **JavaScript** for client‑side validation, obfuscation, or logic hiding.  
Understanding and reversing it can reveal hidden cryptographic routines, flag validators, or exploitable vulnerabilities (like **prototype pollution**).

---

## Quick Reference

| Task                                             | Tool / Method                                                                                    |
|--------------------------------------------------|--------------------------------------------------------------------------------------------------|
| Deobfuscate JS                                   | [de4js](https://lelinhtinh.github.io/de4js/), [JavaScript Deobfuscator](https://deobfuscate.io/) |
| Analyze Obfuscator.io output                     | [Obfuscator.io Deobfuscator](https://obf-io.deobfuscate.io/)                                     |
| Scan `package-lock.json` for vulnerable packages | `npm audit`                                                                                      |
| Alternative vulnerability scanner                | [Snyk CLI](https://github.com/snyk/cli)                                                          |

---

## Obfuscation Analysis

JavaScript challenges often contain obfuscated logic meant to slow down analysis or hide flags.  
Several tools can **unpack** and **beautify** code automatically:

| Tool                                                         | Description                                         |
|--------------------------------------------------------------|-----------------------------------------------------|
| [de4js](https://lelinhtinh.github.io/de4js/)                 | Browser-based universal JS deobfuscator             |
| [deobfuscate.io](https://deobfuscate.io/)                    | Online service to unpack obfuscator‑style JS        |
| [Obfuscator.io Deobfuscator](https://obf-io.deobfuscate.io/) | Specialized tool for reversing obfuscator.io output |
| [JSNice](https://www.jsnice.org/)                            | AI-based beautifier with variable name recovery     |

---

## Vulnerable Packages

If a `package-lock.json` or `yarn.lock` file is exposed, you can scan dependencies for security issues:

```bash
npm audit
```

Example output:

```text
# npm audit report

ion-parser  *
Severity: critical
ion-parser Prototype Pollution when malicious INI file submitted to application that parses with `parse` - https://github.com/advisories/GHSA-7vrv-5m2h-rjw9
fix available via `npm audit fix`
node_modules/ion-parser

1 critical severity vulnerability
```

Alternative scanners:

- [Snyk](https://github.com/snyk/cli)
- [Retire.js](https://retirejs.github.io/retire.js/)

## Prototype Pollution

Prototype Pollution lets attackers modify an object’s `__proto__`, influencing all derived objects which can lead to RCE in certain contexts.

Example vulnerable code:

```javascript
const merge = require('deepmerge');

let obj = {};
let payload = '{"__proto__": {"polluted": true}}';

let result = merge(obj, JSON.parse(payload));

console.log(obj.polluted); // true
```

The payload pollutes the base prototype, giving every object a new `polluted` property.

In real attacks, this can corrupt configuration data or inject arbitrary code paths.

## Tips

- Use DevTools Pretty Print to quickly format JavaScript in the browser.
- In CTFs, search for suspicious variable names or encoded strings (`atob`, `btoa`, `fromCharCode`).
- If a script performs Base64 decoding or XOR operations, reverse it using Node.js or Python to extract the hidden flag.
