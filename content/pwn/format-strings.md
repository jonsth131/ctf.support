---
title: "Format Strings"
description: "Exploit format string vulnerabilities to leak or overwrite memory using positional parameters with printf-style specifiers."
date: 2025-10-05
draft: false
categories: ["Pwn"]
tags: ["pwn", "format strings", "memory leak", "printf", "exploit"]
authors: ["CTF.Support Team"]
summary: "Master format string exploits, leak memory, read stack data, and overwrite values using printf-style vulnerabilities."
aliases: ["/pwn/format-strings/"]
slug: "format-strings"
toc: true
---

## Introduction

**Format string vulnerabilities** occur when user-controlled input is passed directly into printing functions like `printf` without specifying a format string.

These issues often appear in CTF *pwn* challenges and can lead to **information leaks**, **stack inspection**, or even **memory modification** under the right conditions.

Example:

```c
printf(user_input); // Vulnerable
printf("%s", user_input); // Safe
```

When uncontrolled input reaches `printf`, attackers can inject format specifiers such as `%x`, `%p`, or `%n` to access stack values or write data to memory.

---

## Common Specifiers

| Specifier   | Description                                                     |
|-------------|-----------------------------------------------------------------|
| `%x` / `%X` | Print integers in hexadecimal format.                           |
| `%p`        | Print pointers (memory addresses).                              |
| `%s`        | Interpret memory as a C‑string.                                 |
| `%n`        | Write the number of bytes printed so far into a memory address. |

By combining these with **positional parameters**, you can move through stack arguments to leak or analyze data.

---

## Stack Access Syntax

Stack access syntax can be used by using numbered format parameters to control which stack slot you read from.

```text
%1$p - Print first stack pointer as an address
%7$s - Interpret 7th stack value as a string
```

This allows precise control when searching for stack data, addresses, or strings in memory.

---

## Information Leaks

One of the first goals in a format string challenge is to **leak addresses**, this helps you bypass Address Space Layout Randomization (ASLR) by discovering where libraries or stacks are mapped in memory.

### Typical Leak Process

1. **Dump stack values** using `%p` or `%x` until you find something resembling a memory address.
2. **Identify interesting data:** pointers, strings, return addresses, or GOT entries.
3. **Correlate leaks** with known offsets from tools such as `objdump`, `readelf`, or `pwndbg`.

Leaked pointers can reveal:

- **Return addresses** (showing where the program will jump)
- **Stack or libc addresses** (used to calculate base addresses)
- **Static/global variables**

---

## Calculating Offsets

Once an address leak is obtained, use it to determine base addresses of libraries or the executable.

Example concept:

```text
Leaked printf address:  0x7ffff7a33450
Known printf offset:   0x64e50
--------------------------------
libc base = 0x7ffff7a33450 - 0x64e50 = 0x7ffff79ce000
```
