---
title: "Disassemblers"
description: "Convert machine code into human-readable assembly and analyze program structure using tools like Ghidra, IDA, Binary Ninja, and Cutter."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["disassemblers", "ghidra", "ida", "binary ninja", "cutter"]
authors: ["CTF.Support Team"]
summary: "Learn how disassemblers translate binaries into assembly, helping you locate functions, variables, and control flow in CTF reverse engineering."
aliases: ["/reverse/disassemblers/"]
slug: "disassemblers"
toc: true
---

## Introduction

Disassemblers transform compiled binaries into their corresponding assembly instructions.  
They are essential in reverse engineering to read the underlying logic, even when source code or symbols are missing.  

In CTF challenges, examining assembly helps identify encryption loops, password checks, and hidden key comparisons.

## Quick Reference

- Basic binary disassembly: `objdump -d <binary> | less`
- Batch disassemble: `r2 -A -q -c "pd 50" ./binary`

## Tools

| Tool                                       | Platform       | Purpose                                                      |
|--------------------------------------------|----------------|--------------------------------------------------------------|
| [Ghidra](https://ghidra-sre.org/)          | Cross‑platform | Comprehensive RE suite for static analysis and decompilation |
| [IDA Free](https://hex-rays.com/ida-free/) | Cross‑platform | Disassembler with decompiler                                 |
| [Binary Ninja](https://binary.ninja/)      | Cross‑platform | Modern, scriptable binary analysis environment               |
| [Cutter](https://cutter.re/)               | Cross‑platform | GUI frontend for Radare2, open‑source alternative            |
| `objdump`                                  | Linux          | Command‑line disassembler for ELF binaries                   |

## Tips

- Enable function auto‑analysis in Ghidra before browsing disassembly to recover call graphs automatically.  
- Cross‑reference functions (XREFs) to find where flag checks are called.  
- IDA and Ghidra can generate pseudocode (“decompile” mode) for faster comprehension.  
- Cutter integrates Radare2 backends, combining manual patching with visualization.  
- For quick checks in CLI‑only environments, `objdump` is reliable and portable.
