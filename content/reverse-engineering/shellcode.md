---
title: "Shellcode"
description: "Run and debug shellcode safely using harnesses or emulators in CTF reverse engineering challenges."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["shellcode", "debugging", "reverse engineering", "blobrunner"]
authors: ["CTF.Support Team"]
summary: "Learn to execute and debug raw shellcode securely using tools like BlobRunner and controlled Windows/Linux environments."
aliases: ["/reverse/shellcode/"]
slug: "shellcode"
toc: true
---

## Introduction

Shellcode is small executable machine code, often used in exploitation or CTF challenges.  
Instead of running it directly (which can crash your system), debugging tools enable safe inspection of its behavior.

## Quick Reference

- Use **BlobRunner** to harness shellcode for debugging.  
- Load shellcode inside a sandboxed VM or emulator (e.g., QEMU, VirtualBox).  
- Disassemble and inspect flow using Ghidra or Radare2.

## Tools

| Tool                                                                | Purpose                                                 |
|---------------------------------------------------------------------|---------------------------------------------------------|
| [BlobRunner](https://github.com/OALabs/BlobRunner)                  | Run and debug shellcode safely inside a Windows process |
| [Immunity Debugger](https://www.immunityinc.com/products/debugger/) | Debug shellcode in Windows sandbox environment          |
| [x64dbg](https://x64dbg.com/)                                       | Visual debugger for injecting and analyzing payloads    |
| [Ghidra](https://ghidra-sre.org/)                                   | Disassemble and analyze shellcode statically            |

## Example: Debugging with BlobRunner

```bash
BlobRunner.exe shellcode.bin
```

This loads the shellcode into a simple harness program so you can attach **x64dbg** or **Immunity Debugger** for step‑by‑step analysis.
