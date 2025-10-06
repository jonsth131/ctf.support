---
title: "Godot"
description: "Reverse engineer Godot games by unpacking resources and analyzing GDScript for hidden logic."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["godot", "gdscript", "gdsdecomp", "reverse engineering", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn to decompile and explore Godot-based games to extract scripts, configuration files, and embedded secrets."
aliases: ["/reverse/games/godot/"]
slug: "godot"
toc: true
---

## Introduction

[Godot](https://godotengine.org/) is an open-source engine that stores assets and scripts in `.pck` package files.  
In CTF challenges, the flags or validation logic are frequently found in the decompiled **GDScript** files.

## Quick Reference

- Extract resources: `gdsdecomp -p game.pck -d output/`
- Inspect unpacked `res://` directory
- Decompile `.gd` scripts to readable GDScript

## Tools

| Tool                                                | Purpose                                       |
|-----------------------------------------------------|-----------------------------------------------|
| [gdsdecomp](https://github.com/GDRETools/gdsdecomp) | Extracts and decompiles Godot `.pck` archives |
| [Godot Engine](https://godotengine.org/)            | Used to test and rebuild extracted projects   |
| [GDRETools Suite](https://github.com/GDRETools)     | Toolkit for broader Godot resource analysis   |

## Tips

- Check `project.godot` for engine version (important for decompression).  
- Search `.gd` scripts for function names like `check_flag()` or `validate_input()`.  
- Text-based configuration and JSON files inside the package often conceal encoded strings.  
- You can rebuild extracted content in Godot for easier navigation.
