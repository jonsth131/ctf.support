---
title: "Command Injection"
description: "Exploit unsanitized parameters in system commands to execute arbitrary OS commands on a web server."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["command injection", "rce", "subprocess", "os", "web security"]
authors: ["CTF.Support Team"]
summary: "Discover how web applications vulnerable to OS-level command injection allow arbitrary command execution."
aliases: ["/web/command-injection/"]
slug: "command-injection"
toc: true
---

## Introduction

**Command Injection** occurs when user input is concatenated into system commands executed by the application.  
In CTF challenges, this often grants full control over the server environment.

## Example (Python)

```python
import subprocess

def process_file(filename):
    return subprocess.check_output(['cat', filename])

filename = input('Enter a filename: ')
print(process_file(filename))
```

### Exploit

```text
filename = "file.txt; ls"
```

Resulting command:

```bash
cat file.txt; ls
```

## Tips

- Chain commands with `;`, `&&`, or `|`.
- Use blind command injection (e.g., `ping -c 1 <collaborator>`) to confirm execution.
- Try OS-specific payloads if Linux/Windows detection is uncertain.
