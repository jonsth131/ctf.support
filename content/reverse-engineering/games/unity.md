---
title: "Unity"
description: "Reverse engineer Unity games by decompiling managed or IL2CPP assemblies and extracting serialized assets."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["unity", "il2cpp", "mono", "assetripper", "bepinex", "reverse engineering"]
authors: ["CTF.Support Team"]
summary: "Analyze and extract code, metadata, and assets from Unity-based games using decompilers and asset unpackers."
aliases: ["/reverse/games/unity/"]
slug: "unity"
toc: true
---

## Introduction

Unity is one of the most frequently used game engines in CTF reverse engineering challenges.  
Flags or credentials often appear in embedded scripts, asset bundles, or metadata resources.

## Quick Reference

- Managed (Mono): `Assembly-CSharp.dll`
- IL2CPP games: use `IL2CPP Dumper` or `Cpp2IL`
- Decompile assemblies: `ILSpy`, `dnSpyEx`, or `JADX`
- Extract assets: `AssetRipper`, `UABE`

## Tools

| Tool                                                                  | Purpose                                                 |
|-----------------------------------------------------------------------|---------------------------------------------------------|
| [IL2CPP Dumper](https://github.com/Perfare/Il2CppDumper)              | Extracts symbols and metadata from IL2CPP binaries      |
| [Cpp2IL](https://github.com/SamboyCoding/Cpp2IL)                      | Recreates readable C# from compiled C++ code            |
| [AssetRipper](https://github.com/AssetRipper/AssetRipper)             | Extracts Unity assets/resources from projects or builds |
| [Unity Assets Bundle Extractor](https://github.com/SeriousCache/UABE) | Legacy asset editor for Unity bundles                   |
| [MelonLoader](https://melonloader.net/)                               | Loads plugins for runtime inspection                    |
| [BepInEx](https://github.com/BepInEx/BepInEx)                         | Plugin injector and modding framework                   |
| [UnityExplorer](https://github.com/sinai-dev/UnityExplorer)           | Runtime object explorer for managed Unity assemblies    |

## Tips

- Mono games are easily decompiled — check `Managed/Assembly-CSharp.dll`.  
- IL2CPP games separate metadata (`global-metadata.dat`) and binary logic (`GameAssembly.dll`).  
- Extraction tools can reveal text assets or JSON configs containing hints for flag validation.  
- Plugin loaders like **BepInEx** or **MelonLoader** allow executing your own C# scripts inside the running game.
