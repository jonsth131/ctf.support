---
title: "Nintendo DS"
description: "Extract and disassemble Nintendo DS ROMs to explore ARM9/ARM7 code and embedded resources."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["nintendo ds", "nds", "arm", "emulation", "ndsfactory", "desmume"]
authors: ["CTF.Support Team"]
summary: "Learn to unpack and reverse engineer Nintendo DS ROMs by analyzing ARM binaries and resources."
aliases: ["/reverse/roms/nintendo-ds/"]
slug: "nintendo-ds"
toc: true
---

## Introduction

Nintendo DS games often package graphics, sound, and two ARM executables (ARM7 and ARM9).
By unpacking them, you can disassemble the logic or modify in-game text to retrieve hidden CTF flags.

## Quick Reference

- Unpack ROM: `nds_unpacker.py game.nds`
- Load `arm9.bin` in Ghidra -> Processor: ARM v5 -> Base: 0x02000000
- Test execution in [DeSmuME](https://github.com/TASEmulators/desmume)

## Tools

| Tool                                                        | Purpose                     |
|-------------------------------------------------------------|-----------------------------|
| [DeSmuME](https://github.com/TASEmulators/desmume/releases) | NDS emulator with debugging |
| [NDSFactory](https://github.com/Luca1991/NDSFactory)        | Unpack and repack NDS ROMs  |
| [Ghidra](https://ghidra-sre.org/)                           | Disassemble ARM binaries    |

## Tips

- Focus on the **ARM9** code, it usually holds logic and flag comparisons.
- Extract embedded assets like `.bin` or `.dat`, they may include hint text or images.
- Run inside **DeSmuME debugger** to inspect VRAM or register states.
