---
title: "Decompilers"
description: "Learn how to recover human-readable source code from binaries, bytecode, and compiled scripts in CTF reverse engineering challenges."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["decompilers", "java", "python", "dotnet", "flash", "jadx", "ilspy"]
authors: ["CTF.Support Team"]
summary: "Overview of tools for decompiling .NET, Java, Python, and other compiled formats to assist reverse engineering in CTFs."
aliases: ["/reverse/decompilers/"]
slug: "decompilers"
toc: true
---

## Introduction

Decompilers allow you to recover source-like representations from compiled code such as .NET assemblies, Java archives, Python bytecode, or Flash objects.
In CTF reverse engineering challenges, this step often exposes authentication routines, flag checks, or encryption functions hidden in compiled applications.

## Quick Reference

| Purpose                          | Command / Tool                                         |
|----------------------------------|--------------------------------------------------------|
| Decompile Java `.class` / `.jar` | `jadx-gui file.apk` or `java -jar JD-GUI.jar file.jar` |
| Decompile .NET binaries          | `ilspycmd` or `dnSpyEx`                                |
| Decompile Python `.pyc`          | `uncompyle6 file.pyc`                                  |
| Extract PyInstaller executables  | `python pyinstxtractor.py target.exe`                  |

## Tools

| Tool                                                                                         | Platform       | Purpose                                  |
|----------------------------------------------------------------------------------------------|----------------|------------------------------------------|
| [ILSpy](https://github.com/icsharpcode/ILSpy)                                                | .NET           | Decompile assemblies and view C#/IL code |
| [ilspy-vscode](https://marketplace.visualstudio.com/items?itemName=icsharpcode.ilspy-vscode) | .NET           | Decompile .NET assemblies in VS Code     |
| [AvaloniaILSpy](https://github.com/icsharpcode/AvaloniaILSpy)                                | .NET           | Avalonia fork of ILSpy                   |
| [dnSpy](https://github.com/dnSpy/dnSpy)                                                      | .NET           | .NET debugger and decompiler             |
| [dnSpyEx](https://github.com/dnSpyEx/dnSpy)                                                  | .NET           | Advanced .NET debugger and decompiler    |
| [dotPeek](https://www.jetbrains.com/decompiler/)                                             | .NET           | .NET decompiler from JetBrains           |
| [JADX](https://github.com/skylot/jadx)                                                       | Java / Android | Decompile Dex or APKs                    |
| [Bytecode Viewer](https://www.bytecodeviewer.com/)                                           | Java / Android | Decompile Dex or APKs                    |
| [JD‑GUI](https://java-decompiler.github.io/)                                                 | Java           | GUI viewer for `.class` / `.jar` source  |
| [uncompyle6](https://github.com/rocky/python-uncompyle6)                                     | Python ≤ 3.8   | Decompile `.pyc` bytecode                |
| [Decompyle++](https://github.com/zrax/pycdc)                                                 | Python ≥ 3.9   | Alternative modern Python decompiler     |
| [pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor)                         | Python EXE     | Decompile PyInstaller executables        |
| [JPEXS Decompiler](https://github.com/jindrapetrik/jpexs-decompiler)                         | Flash          | Decompile `.swf` (ActionScript) files    |

## Tips

- Prefer GUI tools when exploring multiple assemblies, ILSpy and JADX integrate tree views for navigation.
- Some CTFs hide flags in string constants or hardcoded URLs visible only after decompilation.
