---
title: "Shellcode"
description: "Generate and inject shellcode to achieve arbitrary code execution in pwn challenges using Pwntools or MSFvenom."
date: 2025-10-05
draft: false
categories: ["Pwn"]
tags: ["pwn", "shellcode", "pwntools", "asm", "msfvenom", "payload"]
authors: ["CTF.Support Team"]
summary: "Generate assembly payloads and inject shellcode to spawn shells or execute arbitrary commands in exploitation tasks."
aliases: ["/pwn/shellcode/"]
slug: "shellcode"
toc: true
---

## Introduction

**Shellcode** is binary code written in assembly used to execute specific operations, usually to spawn a shell or modify system state.

In CTFs, you inject shellcode into an overflow or overwrite vulnerability to gain control over execution.

## Pwntools

`pwntools` can assemble and inject shellcode quickly.

Example to generate Linux x64 shellcode to run sh:

``` python
from pwnlib import *

context.context(arch='amd64', os='linux')
shellcode = asm.asm(shellcraft.amd64.linux.sh())
print(shellcode)
```

## MSFvenom

`msfvenom` (from Metasploit) is another common generator.

Generate payload:

```bash
msfvenom -p <payload> -f raw
```

List available payloads:

```bash
msfvenom --list payloads
```

Common payloads:

```text
linux/x86/shell_reverse_tcp
windows/x64/shell_bind_tcp
```
