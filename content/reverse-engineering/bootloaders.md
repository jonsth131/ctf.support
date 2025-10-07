---
title: "Bootloaders"
description: "Analyze and debug bootloader binaries such as MBR boot sectors using QEMU and GDB in reverse engineering challenges."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["bootloader", "qemu", "gdb", "mbr", "bios", "assembly"]
authors: ["CTF.Support Team"]
summary: "Learn to emulate and debug bootloader binaries with QEMU and GDB for low-level reverse engineering tasks."
aliases: ["/reverse/bootloaders/"]
slug: "bootloaders"
toc: true
---

## Introduction

Bootloaders are the first code executed during system startup, responsible for loading an OS or initializing firmware.
In low-level reversing or CTF tasks, bootloaders may contain custom routines, hidden data, or password checks embedded in assembly.

## Quick Reference

- Run a bootloader in QEMU:

```bash
qemu-system-i386 -drive file=bootloader.bin,format=raw
```

- Launch QEMU for debugging:

```bash
qemu-system-i386 -drive file=bootloader.bin,format=raw -s -S
```

- Attach GDB to debug session:

```bash
gdb
(gdb) target remote localhost:1234
```

## Tools

| Tool                                     | Purpose                                                         |
|------------------------------------------|-----------------------------------------------------------------|
| [QEMU](https://www.qemu.org/)            | Full-system emulator for running bootloaders or firmware images |
| [GDB](https://www.gnu.org/software/gdb/) | Debugger for symbol inspection and step-debugging               |
