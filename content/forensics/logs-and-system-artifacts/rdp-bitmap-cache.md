---
title: "RDP Bitmap Cache"
description: "Extract cached Remote Desktop bitmap fragments to reconstruct remote session images and identify on-screen activity."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["rdp", "bitmap cache", "bmc tools", "remote desktop", "session analysis"]
authors: ["CTF.Support Team"]
summary: "Use the RDP Bitmap Cache to visualize remnants of remote desktop sessions and recover evidence of on-screen activity."
aliases: ["/forensics/logs-and-system-artifacts/rdp-bitmap-cache/"]
slug: "rdp-bitmap-cache"
toc: true
---

## Introduction

When a user connects to another system using **Remote Desktop Protocol (RDP)**, Windows caches small bitmap fragments of the remote desktop to speed up subsequent sessions. These fragments can reveal partial or complete images of the remote desktop.

## Tools

| Tool                                               | Purpose                                               |
|----------------------------------------------------|-------------------------------------------------------|
| [BMC-Tools](https://github.com/ANSSI-FR/bmc-tools) | Extract and reconstruct bitmaps from RDP Bitmap Cache |

## Location

```text
C:\Users\<username>\AppData\Local\Microsoft\Terminal Server Client\Cache\
```

## Common file patterns

```text
bcache*.bmc
cache0000.bin
cache0001.bin
```

## Recover images using BMC-Tools

```bash
./bmc-tools.py -s Cache -d Output
```
