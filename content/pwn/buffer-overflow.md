---
title: "Buffer Overflow"
description: "Exploit stack-based memory corruption to control program execution and redirect code flow in pwn challenges."
date: 2025-10-05
draft: false
categories: ["Pwn"]
tags: ["pwn", "buffer overflow", "pattern", "cyclic", "overflow"]
authors: ["CTF.Support Team"]
summary: "Learn to find and exploit stack-based buffer overflows using pattern generators, cyclic offsets, and crafted payloads."
aliases: ["/pwn/buffer-overflow/"]
slug: "buffer-overflow"
toc: true
---

## Introduction

A buffer overflow occurs when a program writes more data to a buffer than it can hold, overwriting adjacent memory such as return addresses.

By carefully crafting input, you can redirect execution flow to arbitrary code.

## Pattern Generation

- [Buffer overflow pattern generator](https://wiremask.eu/tools/buffer-overflow-pattern-generator/): Web utility for 32‑bit & 64‑bit patterns
- Pwntools: `cyclic(n)` -> generate pattern / `cyclic_find(offset)` -> find crash offset

Example (64‑bit pattern):

```python
from pwn import *
pattern = cyclic(200)
print(pattern)
```
