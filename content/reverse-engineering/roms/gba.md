---
title: "Game Boy Advance"
description: "Analyze and reverse engineer Game Boy Advance ROMs to uncover in-game assets and embedded logic."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["gba", "gameboy advance", "mgba", "arm", "emulation"]
authors: ["CTF.Support Team"]
summary: "Use emulators and disassemblers to inspect Game Boy Advance ROMs for hidden strings and flag routines."
aliases: ["/reverse/roms/gba/"]
slug: "gba"
toc: true
---

## Introduction

Game Boy Advance (GBA) ROMs contain ARM code and asset data (sprites, maps, text).  
Reverse engineering them helps reveal game logic or check conditions used to validate flags in CTF tasks.

## Quick Reference

- Run ROM in [mGBA](https://mgba.io/)  
- Debug address space with **no$gba** or **arm‑none‑eabi‑gdb**
- Extract assets using `binwalk -e file.gba`

## Tools

| Tool                                             | Purpose                                   |
|--------------------------------------------------|-------------------------------------------|
| [mGBA](https://mgba.io/)                         | Emulator supporting debugging and tracing |
| [Ghidra](https://ghidra-sre.org/)                | Disassemble ARM and THUMB code            |
| [binwalk](https://github.com/ReFirmLabs/binwalk) | Detect embedded resources                 |

## Tips

- Look for ASCII text blocks in the ROM, they often encode hints or flags.  
- Set breakpoints on string comparison functions to track input validation.  
- Some GBA ROMs use simple checksums or crypto that can be patched with a hex editor.
