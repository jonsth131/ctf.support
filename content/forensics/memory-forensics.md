---
title: "Memory Forensics"
description: "Analyze volatile memory (RAM) to extract processes, credentials, and hidden artifacts using Volatility 3, strings, and file recovery tools in CTF and forensic investigations."
date: 2025-10-05
draft: false
weight: 40
categories: ["Forensics"]
tags: ["memory forensics", "volatility", "RAM analysis", "malfind", "linux symbols", "dwarf2json"]
authors: ["CTF.Support Team"]
toc: true
summary: "Learn to analyze memory dumps using Volatility 3, recover embedded files, search for flags, and detect injected code in CTF forensics challenges."
aliases: ["/forensics/memory-forensics/"]
slug: "memory-forensics"
---

## Introduction

Memory forensics involves analyzing volatile memory (RAM) to extract information about:

- Running processes
- Network connections
- User activity
- Hidden or injected code

Unlike disk forensics, memory analysis can reveal live secrets (passwords, encryption keys, flags) that aren’t stored on disk.

In CTFs, memory dumps are often provided as challenges to extract hidden data or flags.

## Quick Reference

- Identify OS profile (Windows): `vol -f memory_dump.raw windows.info`
- Identify OS profile (Linux): `vol -f memory_dump.raw banner`
- Extract processes: `vol -f memory_dump.raw windows.pslist`
- Search for flags: `strings memory_dump.raw | grep "CTF{"`
- Recover files: `foremost -i memory_dump.raw -o recovered/`

## Tools

| Tool                                                                | Purpose                                                          |
|---------------------------------------------------------------------|------------------------------------------------------------------|
| [Volatility 3](https://github.com/volatilityfoundation/volatility3) | Extract processes, network info, and artifacts from memory dumps |
| [dwarf2json](https://github.com/volatilityfoundation/dwarf2json)    | Convert Linux kernel debug symbols to Volatility 3 JSON files    |
| `strings`                                                           | Extract readable text from memory                                |
| `foremost` / `binwalk`                                              | Recover embedded files from memory                               |

## Step-by-Step Guide

### Identify Profile (OS)

Windows:

```bash
vol -f memory_dump.raw windows.info
```

Linux:

```bash
vol -f memory_dump.raw banner
```

Example output:

```text
Offset  Banner

0x67200200      Linux version 5.10.0-35-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.237-1 (2025-05-19)
0x7f40ba40      Linux version 5.10.0-35-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.237-1 (2025-05-19)
0x94358280      Linux version 5.10.0-35-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.237-1 (2025-05-19)
0xa9fc5ac0      Linux version 5.10.0-35-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.237-1 (2025-05-19)
0x12ee9c300     Linux version 5.10.0-35-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.237-1 (2025-05-19)
```

### Creating a Linux Symbols File

After identifying the Linux kernel version, a symbol file needs to be created. To create a correct symbol file for the current memory dump a debug version for the correct kernel is needed.

For example, using the information from the `banner` output in the previous section, the identified kernel is `5.10.0-35-amd64` from a Debian distro. Debian has an archive of older packages in the [debian-security](https://snapshot.debian.org/archive/debian-security/20250520T200947Z/pool/updates/main/l/linux/) repository. The kernel files needed are the `dbg` version of the kernel, in this case `linux-image-5.10.0-35-amd64-dbg_5.10.237-1_amd64.deb`.

After downloading the debug version of the kernel, extract it by using:

```bash
dpkg-deb -x linux-image-5.10.0-35-amd64-dbg_5.10.237-1_amd64.deb dbg-out
```

The symbols needed exists in the `vmlinux` file, in this case it is located at `dbg-out/usr/lib/debug/lib/modules/5.10.0-35-amd64/vmlinux`.

Extract the symbols using `dwarf2json`:

```bash
dwarf2json linux --elf dbg-out/usr/lib/debug/lib/modules/5.10.0-35-amd64/vmlinux > outputvmlinux-5.10.0-35-amd64.json
```

Put the json file in Volatility's `symbol/linux` directory. It should be something like `~/.local/pipx/venvs/volatility3/lib/python3.10/site-packages/volatility3/symbols` if Volatility is installed using `pipx`.

### Using Volatility 3

Volatility 3 can extract processes, network connections, and artifacts:

Useful Volatility Plugins:

- `windows.hashdump` – Extract password hashes
- `windows.cmdline` – Display process command-line arguments
- `linux.bash` – Recover bash command history
- `linux.proc.Maps` – View process memory maps

List plugins:

```bash
vol --help
```

List running processes (Windows):

```bash
vol -f memory_dump.raw windows.pslist
vol -f memory_dump.raw windows.psscan
vol -f memory_dump.raw windows.pstree
```

List running processes (Linux):

```bash
vol -f memory_dump.raw linux.pslist
vol -f memory_dump.raw linux.psscan
vol -f memory_dump.raw linux.pstree
```

### Strings

Use `strings` to search for flags or readable data:

```bash
strings memory_dump.raw | less
strings memory_dump.raw | grep "CTF{"
```

Look for command history, credentials, or hidden messages.

### Recovering Embedded Files

Memory may contain entire files:

```bash
foremost -i memory_dump.raw -o recovered/
binwalk -e memory_dump.raw
```

Extract JPEGs, ZIPs, or other artifacts, analyze files with previous file analysis techniques.

### Detecting Hidden / Injected Code

Some CTF challenges hide flags inside injected processes or shellcode.

Use Volatility plugins like:

- `malfind`: detect injected or hidden code
- `ldrmodules`: enumerate loaded modules in memory
