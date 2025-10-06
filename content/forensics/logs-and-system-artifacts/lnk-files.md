---
title: "LNK Shortcut Files"
description: "Investigate Windows LNK (shortcut) files to uncover recently accessed documents, external drives, and user activity."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["lnk", "windows", "shortcuts", "lecmd", "user activity"]
authors: ["CTF.Support Team"]
summary: "Analyze .LNK shortcut files for file paths, access timestamps, and link metadata that reveal user behavior and file history."
aliases: ["/forensics/logs-and-system-artifacts/lnk-files/"]
slug: "lnk-files"
toc: true
---

## Introduction

Windows automatically creates **shortcut files (.LNK)** whenever a user opens a document, application, or folder.  
These files record **file locations, access times, drive information, and original paths**, even if the originals were deleted or on removable media.

In digital forensics and CTF challenges, analyzing `.LNK` artifacts can reveal:

- File or flag names previously opened by a user  
- The original storage device or network share a file resided on  
- When specific files were accessed (creation / modification / access times)  
- Links between user actions and `$J` journal or registry entries

## Tools

| Tool                                                | Purpose                                                                    |
|-----------------------------------------------------|----------------------------------------------------------------------------|
| [LECmd](https://ericzimmerman.github.io/#!index.md) | Parse `.lnk` files to CSV, extracts metadata, timestamps, and linked paths |

## Typical Locations

| Location                                                    | Purpose                             |
|-------------------------------------------------------------|-------------------------------------|
| `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\` | User shortcut cache (recent files)  |
| `C:\Users\<user>\Recent\AutomaticDestinations\`             | Jump Lists (binary .lnk containers) |

## Using LECmd

```bash
# Parse all recent shortcut files
LECmd.exe -d "C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent" --csv "out/"
```

Resulting CSV fields include:

| Column                                                  | Meaning                                              |
|---------------------------------------------------------|------------------------------------------------------|
| SourceFile                                              | Path of the .LNK parsed                              |
| TargetFile                                              | File or folder linked to                             |
| MachineID                                               | Host where the .LNK was created                      |
| VolumeSerialNumber                                      | Serial of the drive that contained the original file |
| OpenedTime / AccessedTime / ModifiedTime / CreationTime | Timeline metadata useful for correlation             |
