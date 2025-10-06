---
title: "Assembly"
description: "Understand compiler-generated assembly by analyzing source code output using Compiler Explorer and similar tools."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["assembly", "disassembly", "compiler explorer", "x86", "reverse engineering"]
authors: ["CTF.Support Team"]
toc: true
summary: "Learn to interpret compiler output and understand low-level assembly instructions for CTF reverse engineering tasks."
aliases: ["/reverse/assembly/"]
slug: "assembly"
resources:
  - name: compiler-explorer-input
    src: "compiler-explorer-input.png"
    title: Compiler Explorer C++ Input
  - name: compiler-explorer-x86-64-output
    src: "compiler-explorer-x86-64-output.png"
    title: Compiler Explorer x86-64 Output
---

## Introduction

In reverse engineering challenges, understanding assembly is essential to analyze compiled executables and recover program behavior.  
By viewing compiler output from short snippets, you can quickly learn how code constructs translate into CPU instructions, an important skill in static analysis and binary reversing.

## Quick Reference

- Interactive compiler tool: [Compiler Explorer](https://godbolt.org)
- Common compilers: `gcc`, `clang`, `msvc`
- Disassemble binaries: `objdump -d <binary> | less`

## Tools

| Tool | Purpose |
|------|----------|
| [Compiler Explorer](https://godbolt.org) | Compare high-level code to generated assembly across compilers |
| `objdump` | Disassemble binaries to examine CPU code |
| `ndisasm`, `radare2`, `ghidra` | Additional low-level disassembly analysis |

## Using Compiler Explorer

Open [Compiler Explorer](https://godbolt.org).  
Enter the following sample code:

```cpp
int square(int num) {
    return num * num;
}
```

{{< img name="compiler-explorer-input" size="medium" >}}

Then, choose a compiler (e.g., x86-64 gcc 12.1) to view the disassembly output.

{{< img name="compiler-explorer-x86-64-output" size="medium" >}}

The generated assembly shows stack setup (push, mov) and calculation using the imul instruction.

## Tips

- Use -O2 or -O3 flags to see how optimizations affect assembly.
- Match functions by their symbol names or signatures.
- Recognize common compiler patterns, e.g., stack frame setup, register calling conventions, and return sequences (`pop rbp`, `ret`).
