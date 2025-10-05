---
title: "Text Steganography"
description: "Extract hidden or compressed messages embedded in whitespace or Unicode characters using stegsnow and modern zero-width character tools."
date: 2025-10-05
draft: false
weight: 30
categories: ["Steganography"]
tags: ["text steganography", "stegsnow", "unicode", "zero-width", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn to uncover hidden data in plain text — using whitespace, Unicode characters, or invisible metadata encoding."
aliases: ["/steganography/text/"]
slug: "text"
resources:
  - name: stegsnow
    src: "stegsnow.png"
    title: "Example: Stegsnow encoded file"
toc: true
---

## Introduction

Text steganography hides information within seemingly ordinary pieces of text.

Instead of modifying pixels or sound waves, data is concealed using spaces, tabs, or invisible Unicode characters.

Common encoding tricks:

- Whitespace or tab padding after lines (Stegsnow classic method).
- Zero-width characters in Unicode (e.g., U+200B or U+200C).
- Encoding binary patterns in typesetting, punctuation, or character order.

## Quick Reference

- Extract hidden text: `stegsnow input.txt`
- Extract compressed hidden data: `stegsnow -C input.txt`
- Check for zero-width Unicode characters: `cat -A input.txt`
- Inspect hex dump for anomalies: `xxd input.txt`
- Show hidden spaces/tabs in VS Code: View -> Render Whitespace

## Tools

| Tool                                                  | Purpose                                                                                                                            |
|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| `stegsnow` | CLI tool that extracts (and encodes) hidden messages in text using spacing and tabs. |
| [Unicode Steganography with Zero-Width Characters](https://330k.github.io/misc_tools/unicode_steganography.html) | Browser-based tool that hides or reveals data encoded with zero-width joiners (ZWSP, ZWNJ). |
| [Stegcloak](https://stegcloak.surge.sh/) | Modern stego tool using zero-width Unicode to conceal text inside other messages (supports encryption). |
| `cat -A`, `xxd` | Terminal utilities to detect control characters, spacing, or non-printable Unicode anomalies. |

## Stegsnow

`stegsnow` hides or extracts data using trailing whitespace at the end of lines or within text blocks.

Whitespace is invisible in most editors, making it ideal for subtle steganography.

Usage:

```bash
# Extract hidden content
stegsnow input.txt

# Extract compressed hidden content
stegsnow -C input.txt
```

{{< img name="stegsnow" size="small" >}}

## Detecting Zero-Width Characters

Zero-width Unicode characters include:

- U+200B (Zero Width Space)
- U+200C (Zero Width Non-Joiner)
- U+200D (Zero Width Joiner)

These cannot be seen directly but can be revealed using:

```bash
cat -A input.txt
xxd input.txt
```
