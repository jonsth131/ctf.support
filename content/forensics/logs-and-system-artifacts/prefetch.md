---
title: "Prefetch Analysis"
description: "Parse Windows Prefetch (.pf) files to determine program execution history and frequency of runs."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["prefetch", "windows", "executable", "program execution", "peCmd"]
authors: ["CTF.Support Team"]
summary: "Recover evidence of program execution through analysis of Windows Prefetch files using tools such as PECmd and Eric Zimmerman's suite."
aliases: ["/forensics/logs-and-system-artifacts/prefetch/"]
slug: "prefetch"
toc: true
---

## Introduction

Windows **Prefetch** files (`.pf`) record information about executed applications for performance optimization.  
They also provide **evidence of program execution**, even after binaries have been deleted.

## Tools

| Tool                                                | Purpose                                             |
|-----------------------------------------------------|-----------------------------------------------------|
| [PECmd](https://ericzimmerman.github.io/#!index.md) | Parse Prefetch files and extract execution metadata |

## Location

```text
C:\Windows\Prefetch\
```

Example files: `CMD.EXE‑A6294E76.pf`, `MIMIKATZ.EXE‑B29D8C74.pf`

## Key Fields

| Field         | Description                           |
|---------------|---------------------------------------|
| Run Count     | Number of times the program executed  |
| Last Run Time | Most recent execution timestamp (UTC) |
| File List     | Files accessed by this executable     |

## Example

```bash
PECmd.exe -d C:\Windows\Prefetch --csv out/
```
