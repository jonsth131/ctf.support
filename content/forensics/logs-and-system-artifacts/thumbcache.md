---
title: "Thumbcache Analysis"
description: "Recover and view cached thumbnail images stored by Windows Explorer to identify viewed or deleted pictures."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["thumbcache", "windows", "image analysis", "thumbnail", "forensic recovery"]
authors: ["CTF.Support Team"]
summary: "Analyze and extract image thumbnails from Windows thumbcache databases to recover visual artifacts of recently accessed files."
aliases: ["/forensics/logs-and-system-artifacts/thumbcache/"]
slug: "thumbcache"
toc: true
---

## Introduction

Windows automatically generates thumbnail previews for images, documents, and videos when browsing folders.

## Tools

| Tool                                                     | Purpose                                                                         |
|----------------------------------------------------------|---------------------------------------------------------------------------------|
| [Thumbcache Viewer](https://thumbcacheviewer.github.io/) | GUI tool that can parse and extract images directly from thumbcache_*.db files. |

## Location

```text
C:\Users\<username>\AppData\Local\Microsoft\Windows\Explorer\
```

## Common filenames

```text
thumbcache_32.db
thumbcache_96.db
thumbcache_256.db
thumbcache_1024.db
thumbcache_idx.db
```

Each file corresponds to a specific thumbnail size.

Using these files, images that the user viewed can be recovered, even if the originals have been deleted.

## View and recover images using Thumbcache Viewer

1. Open a thumbcache file, for example `thumbcache_256.db`.
2. Browse through the extracted thumbnails.
3. Export any recovered images for further analysis (`.png` or `.jpg`).
