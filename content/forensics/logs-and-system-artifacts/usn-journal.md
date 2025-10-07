---
title: "USN Journal Analysis"
description: "Analyze the NTFS $J (USN Journal) to reconstruct file activity timelines and detect deletions, renames, and other changes."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["usn journal", "ntfs", "mft", "windows", "mftecmd", "filesystem"]
authors: ["CTF.Support Team"]
summary: "Understand and parse the NTFS Update Sequence Number Journal ($J) to trace file modifications, creations, and deletions during forensic investigations."
aliases: ["/forensics/logs-and-system-artifacts/usn-journal/"]
slug: "usn-journal"
toc: true
---

## Introduction

The **USN Journal** (`$Extend\$J`) is part of the NTFS file system and records *every change* made to files and directories, creations, deletions, renames, and data writes.
Examining `$J` can reconstruct activity timelines even after logs or prefetch files have been cleared.

On NTFS volumes, the journal file resides at:

```text
C:\$Extend\$J
```

## Tools

| Tool                                                  | Purpose                                             |
|-------------------------------------------------------|-----------------------------------------------------|
| [MFTECmd](https://ericzimmerman.github.io/#!index.md) | Parses `$J` along with `$MFT` and `$LogFile` to CSV |

## Useful Information Stored

Each journal record includes:

| Field                    | Description                                         |
|--------------------------|-----------------------------------------------------|
| Timestamp                | When the event or update occurred                   |
| File Reference Number    | Internal file ID used by NTFS                       |
| Reason Flags             | What action occurred (create, delete, rename, etc.) |
| Parent File Ref          | Directory the file belonged to                      |
| Security ID / Attributes | Permission or metadata modifications                |

Common **Reason Flags** include:

| Flag       | Meaning                |
|------------|------------------------|
| 0x00000100 | File Create            |
| 0x00000200 | File Delete            |
| 0x00002000 | Data Overwrite         |
| 0x00004000 | Data Extend            |
| 0x00008000 | Data Truncation        |
| 0x00010000 | File Rename (New Name) |

## Example Usage

```bash
# Parse the $J journal into CSV for analysis
MFTECmd.exe -f "C:\$Extend\$J" --csv "out/"

# Combine Multiple Sources (MFT + J)
MFTECmd.exe -f "C:\$MFT" --csv "out/"
MFTECmd.exe -f "C:\$Extend\$J" --csv "out/"
```
