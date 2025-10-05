---
title: "File Carving & Recovery"
description: "Learn how to recover deleted or hidden files from disk images, memory dumps, and corrupted data using tools like foremost, binwalk, and dd."
date: 2025-10-05
draft: false
weight: 20
categories: ["Forensics"]
tags: ["file carving", "data recovery", "foremost", "binwalk", "dd", "hex editor"]
authors: ["CTF.Support Team"]
toc: true
summary: "Recover deleted or hidden files from disk images and memory dumps using signature-based and manual carving techniques."
aliases: ["/forensics/file-carving-recovery/"]
slug: "file-carving-recovery"
---

## Introduction

File carving is the process of recovering files from raw data (disk images, memory dumps, or corrupted files) without relying on file system metadata. In CTFs, this is useful for:

- Recovering deleted files
- Extracting files from memory or disk dumps
- Analyzing partially corrupted archives

**Key concept:** Carving relies on file signatures/magic bytes and structure, not file tables.

## Quick Reference

- Extract data using `foremost`: `foremost -i disk_image.img -o recovered/`
- Extract data using `binwalk`: `binwalk -e mystery_file`
- Extract data using `dd`: `dd if=disk_image.img of=recovered_file.bin bs=1 skip=1024 count=1024`

## Tools

| Tool                                                                                      | Purpose                                           |
|-------------------------------------------------------------------------------------------|---------------------------------------------------|
| `foremost`                                                                                | Carves files from raw data by signature           |
| `binwalk`                                                                                 | Extracts embedded files in images, firmware, etc. |
| `dd`                                                                                      | Extract raw sections from disk or memory dumps    |
| `hexdump` / `xxd`                                                                         | Inspect raw bytes                                 |
| [ImHex](https://imhex.werwolv.net/) / [010 Editor](https://www.sweetscape.com/010editor/) | Hex editors to edit files                         |

## Step-by-Step Guide

### Using `foremost`

Foremost is a signature-based carving tool. Example:

```bash
foremost -i disk_image.img -o recovered/
```

- `-i`: input file (raw disk, memory dump, or corrupted file)
- `-o`: output directory
- Default configuration can carve common file types (jpg, png, gif, pdf, zip, etc.)

### Using `binwalk`

Binwalk can be used to extract files and data that have been embedded inside of other files. Example:

```bash
binwalk -e mystery_file
```

- `-e`: extract embedded files automatically
- Check output folder for newly recovered files

### Manual Carving with `dd`

Sometimes files don’t match a standard signature or need precise extraction.

**Example:** Extract bytes 1024–2047 from disk_image.img

```bash
dd if=disk_image.img of=recovered_file.bin bs=1 skip=1024 count=1024
```

- `if`: input file
- `of`: output file
- `bs`: block size
- `skip`: number of blocks to skip
- `count`: number of blocks to copy

Use `hexdump` to verify the header before and after extraction.

### Fixing file headers

Sometimes the file headers are corrupt or invalid. To fix the headers, a hex editor like ImHex or 010 Editor can be used to edit the files.

A list of file headers can be found on [Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures)
