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

Format string vulnerabilities allow an attacker to manipulate the format parameter of printing functions (`printf`, `fprintf`, etc.).

By injecting custom specifiers, you can read stack values or write to memory (when `%n` is used).

## Common Specifiers

| Specifier    | Description                       |
|--------------|-----------------------------------|
| `%s`         | String                            |
| `%p`         | Address of pointer to void void * |
| `%x` or `%X` | Hexadecimal                       |

## Stack Position Syntax

```text
%1$p   # Dump first stack pointer
%7$s   # Interpret 7th arg as C‑string
```

These are useful for information leaks and calculating offsets for writes.
