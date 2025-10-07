---
title: "Debuggers"
description: "Use debuggers to trace execution, inspect memory, and patch binaries in CTF reverse engineering and exploitation challenges."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["debugging", "gdb", "x64dbg", "radare2", "pwndbg", "ollydbg"]
authors: ["CTF.Support Team"]
summary: "Learn to use popular debuggers like GDB, Pwndbg, x64dbg, and Radare2 for analyzing and modifying program execution in real time."
aliases: ["/reverse/debuggers/"]
slug: "debuggers"
toc: true
---

## Introduction

Debuggers are essential tools for analyzing program execution, inspecting memory, and understanding runtime logic.
In CTF reversing or exploitation tasks, they reveal hidden logic paths, encryption routines, or validation functions and are vital for dynamic analysis.

## Quick Reference

- Attach GDB to a process: `gdb -p $(pidof <program>)`
- Launch binary in GDB: `gdb ./binary`
- Radare2 debug mode: `r2 -d ./binary`
- x64dbg Windows GUI debugger: `File -> Open -> Run`

## Tools

| Tool                                                                | Purpose                                                            |
|---------------------------------------------------------------------|--------------------------------------------------------------------|
| [GDB](https://www.gnu.org/software/gdb/)                            | Standard Linux debugger for low-level program inspection           |
| [Pwndbg](https://github.com/pwndbg/pwndbg)                          | GDB plugin with enhanced UI and exploitation-focused features      |
| [Radare2](https://rada.re)                                          | Open-source reverse engineering framework with integrated debugger |
| [x64dbg](https://x64dbg.com/)                                       | GUI debugger for Windows (32 / 64 bit)                             |
| [Immunity Debugger](https://www.immunityinc.com/products/debugger/) | Scriptable Windows debugger with Python support                    |
| [OllyDbg](http://www.ollydbg.de/)                                   | Classic 32-bit Windows debugger for inline patching and analysis   |

## Tips

- Use **breakpoints** before key functions (like `strcmp`, `recv`, `decrypt`) to observe intermediate values.
- Combine static and dynamic analysis, inspect the binary structure in a disassembler before stepping through execution.
- Use **Pwndbg** or **GEF** extensions with GDB to enhance usability during CTF reversing tasks.
