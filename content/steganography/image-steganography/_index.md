---
title: "Image Steganography"
description: "Analyze images to find hidden messages, payloads, and least significant bit (LSB) manipulations using tools like zsteg, Stegsolve, steghide, and stegseek."
date: 2025-10-05
draft: false
weight: 10
categories: ["Steganography"]
tags: ["image steganography", "zsteg", "stegsolve", "steghide", "stegseek", "lsb", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn how to detect and extract hidden data in images using zsteg, Stegsolve, steghide, stegseek, and other steganographic analysis tools."
aliases: ["/steganography/image/"]
slug: "image"
resources:
  - name: stegsolve-normal
    src: "stegsolve-1.png"
    title: "Normal Image - nothing visible."
  - name: stegsolve-revealed
    src: "stegsolve-2.png"
    title: "Green channel bit plane - hidden flag text revealed."
toc: true
---

## Introduction

Image steganography hides information inside pictures by manipulating pixels, color channels, or metadata in ways not visible to the naked eye.

In CTF challenges, this often involves:

- Least Significant Bit (LSB) encoding in image data
- Hidden payloads or files appended after the image’s end marker
- Text or patterns revealed through bit-plane or color channel analysis
- Data disguised in metadata fields (EXIF comments, Creator fields, etc.)

The goal is to reveal these hidden messages or recover embedded files.

This page outlines the most common techniques and tools used to detect and extract data from traditional image formats (e.g. PNG, BMP, JPG).

## Quick Reference

- Reveal hidden LSB content or embedded files: `zsteg -a image.png`
- Extract data hidden with `steghide`: `steghide extract -sf image.jpg`
- Crack steghide password: `stegseek -sf image.jpg -wl rockyou.txt`
- Visualize bit planes manually: `java -jar Stegsolve.jar`

## Tools

| Tool                                                                 | Purpose                             |
|----------------------------------------------------------------------|-------------------------------------|
| [Aperi'Solve](https://aperisolve.fr/)                                | Online tool for image steganography |
| [StegOnline](https://stegonline.georgeom.net/i)                      | Online tool for image steganography |
| [Steganographic Decoder](https://futureboy.us/stegano/decinput.html) | Online tool for image steganography |
| [Steganography Online](https://stylesuxx.github.io/steganography/)   | Online tool for image steganography |
| `zsteg`                                                              | Detect hidden payloads in PNG/BMP   |
| `steghide`                                                           | Detect hidden payloads in JPEG      |
| `stegseek`                                                           | Steghide password cracker           |
| [Stegsolve](http://www.caesum.com/handbook/Stegsolve.jar)            | View and analyze image color planes |
| [iSteg](https://github.com/rafiibrahim8/iSteg)                       | LSB steganography tool              |

## `zsteg`

`zsteg` detects and extracts LSB-encoded data in PNG or BMP images by testing various color-channel and bit combinations.

Installation:

```bash
gem install zsteg
```

Usage:

```bash
# Read common combinations
zteg image.png

# Read all combinations
zteg -a image.png

# Show a specific byte order and channel
zteg -E b8,rgb,lsb,xy image.png
```

## `steghide`

`steghide` embeds or extracts data from JPEG and BMP images.

Extraction may require the correct password used during embedding.

Usage:

```bash
steghide extract -sf image.jpg
```

You’ll be prompted for a password, if none was used, just press Enter.

The extracted data (e.g. txt, zip, or binary file) will appear in the working directory.

## `stegseek`

`stegseek` is a modern cracking tool that can test millions of passwords per second against `steghide`-protected files.

Usage:

```bash
stegseek -sf image.jpg -wl rockyou.txt
```

## iSteg

iSteg is a image LSB steganography tool that exists in a CLI and a GUI version.

## Stegsolve

Stegsolve is a Java-based GUI viewer that lets you explore the bit planes, RGB channels, and XOR layers of an image.

It’s particularly effective at revealing patterns or text hidden subtly inside color data.

{{< img name="stegsolve-normal" size="small" >}}

{{< img name="stegsolve-revealed" size="small" >}}
