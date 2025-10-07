---
title: "General"
description: "Basic reverse engineering techniques including string extraction and syscall tracing useful for analyzing unknown binaries in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["strings", "syscalls", "tracing", "strace", "ltrace", "static analysis"]
authors: ["CTF.Support Team"]
summary: "Learn core RE utilities like strings extraction, syscall tracing, and process inspection that reveal hidden functionality in CTF binaries."
aliases: ["/reverse/general/"]
slug: "general"
toc: true
---

## Introduction

Before diving into full decompilation, basic static and dynamic analysis can uncover valuable information from any binary.
In CTF challenges, tools like `strings`, `strace`, and `ltrace` can expose command execution, embedded text, or system calls that lead to the flag path.

## Quick Reference

| Task                              | Command            |
|-----------------------------------|--------------------|
| Display visible strings in binary | `strings <binary>` |
| Trace system calls                | `strace ./binary`  |
| Trace library calls               | `ltrace ./binary`  |

## Tools

| Tool                                             | Purpose                                     |
|--------------------------------------------------|---------------------------------------------|
| [`strings`](https://linux.die.net/man/1/strings) | Extract readable strings from binaries      |
| [`strace`](https://linux.die.net/man/1/strace)   | Trace system calls used by a program        |
| [`ltrace`](https://linux.die.net/man/1/ltrace)   | Trace library calls and function parameters |
| [xxd / hexdump](https://linux.die.net/man/1/xxd) | View or edit binary data at hex level       |
| [Ghidra](https://ghidra-sre.org/)                | Inspect code structure and disassembly      |

## Example: Tracing Syscalls

You can use `strace` to observe all system-level interactions:

```bash
strace ./encodedpayload
```

Example output (truncated):

```text
execve("./encodedpayload", ["./encodedpayload"], 0x7fffe...) = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_IP) = 3
connect(3, {af=AF_INET, port=1337, addr="127.0.0.1"}, 16) = -1 ECONNREFUSED
write(1, "HTB{PLz_strace_M333}\n", 21)
```

## Tips

- Combine `strings` + `grep` to locate likely flag strings: `strings binary | grep -i flag`
- Use `ltrace` when `strace` gives only syscall-level output, it can reveal internal function calls such as `strcmp()` or `printf()`.
- For stripped binaries, even short syscall traces can guide you to logic branches.
- Always run binaries inside sandboxes or containers to avoid harmful system effects.
