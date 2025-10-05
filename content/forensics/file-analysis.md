---
title: File Analysis
description: "Learn how to identify file types, inspect headers, detect embedded data, and extract metadata for digital forensics and CTF challenges."
date: 2025-10-05
draft: false
weight: 10
categories: ["Forensics"]
tags: ["file analysis", "forensics", "magic bytes", "binwalk", "exiftool", "file", "hexdump", "xxd", "strings"]
authors: ["CTF.Support Team"]
toc: true
aliases: ["/forensics/file-analysis/"]
summary: "Identify, extract, and analyze files in CTF forensics challenges."
slug: "file-analysis"
---

## Introduction

File identification is one of the first steps in a forensics challenge. Often, files may have misleading extensions, be corrupted, or contain hidden data. Knowing how to determine a file’s true type and contents is crucial for further analysis.

Goals:

- Identify file types reliably
- Detect mismatched extensions and polyglots
- Extract useful metadata

## Quick Reference

- Identify file type: `file mystery_file`
- View magic bytes: `hexdump -C file | head`
- Check for embedded data: `binwalk file`
- View metadata: `exiftool file`
- Search for strings: `strings file | grep "CTF{"`

## Tools

| Tool              | Purpose                                    |
|-------------------|--------------------------------------------|
| `file`            | Detects file type based on magic bytes     |
| `hexdump` / `xxd` | Inspect raw bytes of a file                |
| `binwalk`         | Analyze embedded files and compressed data |
| `exiftool`        | Extract metadata from media and documents  |
| `strings`         | Extract readable text                      |

## Step-by-Step Guide

### Using `file`

The simplest method is the `file` command. It looks at the "magic bytes" of a file (the first few bytes that identify a file type).

```bash
file mystery_file
```

Example:

```text
$ file file.zip
file.zip: Zip archive data, at least v2.0 to extract, compression method=deflate
```

Even if the extension is `.jpg`, `file` will detect the true type.

In case of [polyglots](https://en.wikipedia.org/wiki/Polyglot_(computing)), using `file` with the flag `-k/--keep-going` will identify multiple filetypes.

### Inspecting the Header with `hexdump` / `xxd`

If file `fails`, manually check the first few bytes.

Using `hexdump`:

```bash
hexdump -C mystery_file | head -n 10
```

Using `xxd`:

```bash
xxd -l 32 mystery_file
```

**Common Magic Bytes:**

| File Type | Hex Signature                |
|-----------|------------------------------|
| PNG       | 89 50 4E 47 0D 0A 1A 0A      |
| ZIP       | 50 4B 03 04                  |
| PDF       | 25 50 44 46                  |
| GIF       | 47 49 46 38 37 61 / 38 39 61 |

A comprehensive list can be found on the Wikipedia page [List of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures).

### Using `binwalk` for Embedded Files

Some files may contain hidden data or multiple embedded files (polyglots).

Using `binwalk`:

```bash
binwalk mystery_file
```

Example:

```text
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 1920 x 1080
24576         0x6000          Zip archive data, at least v2.0 to extract
```

This can detect compressed archives, images, or encrypted blobs inside another file.

**NOTE:** These tools sometimes return false positives, identifying false headers.

### Extracting Metadata with `exiftool`

For documents or media, metadata often reveals hidden information.

```bash
exiftool mystery_image.jpg
```

Check for:

- Author or device info
- Comments
- Encoded/hex strings
- "FLAG" patterns

### Searching for Strings

Human-readable text might contain hints or embedded flags.

```bash
strings mystery_file | less
```

Look for:

- URLs
- Encoded/hex strings
- "FLAG" patterns

## Tips

- Never rely solely on file extensions, they can be faked.
- Hidden content can exist anywhere, not just in headers (use `binwalk` or `hexdump` / `xxd`).
- Compressed or encrypted embedded files may require extra tools for extraction.
- Some files may be corrupted deliberately to confuse solvers, understanding headers is key.
