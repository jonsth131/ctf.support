---
title: "Android"
description: "Reverse engineer Android applications by unpacking, decompiling, and analyzing APK or AAB packages for secrets, code, and embedded data."
date: 2025-10-06
draft: false
categories: ["Reverse Engineering"]
tags: ["android", "apk", "aab", "jadx", "apktool", "bundletool", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn to reverse engineer Android apps using Apktool, JADX, and Bundletool to reveal hidden logic and resources in APK or AAB files."
aliases: ["/reverse/android/"]
slug: "android"
toc: true
---

## Introduction

Android applications are distributed as either **APK** (Android Package) or **AAB** (Android App Bundle) files.
In CTF reverse engineering, analyzing these files can expose hardcoded keys, credential checks, or base64‑encoded secrets.

## Quick Reference

- Decompile APK: `jadx -d output/ sample.apk`
- Decode resources & smali: `apktool d sample.apk -o output/`
- Build smali back to APK: `apktool b output/ -o rebuilt.apk`
- Extract APKs from AAB:

```bash
bundletool build-apks --bundle=app.aab --output=output.apks
unzip output.apks -d extracted/
```

## Tools

| Tool                                                         | Purpose                                       |
|--------------------------------------------------------------|-----------------------------------------------|
| [JADX](https://github.com/skylot/jadx)                       | Decompile APK/Dex files to Java source        |
| [APKTool](https://ibotpeaches.github.io/Apktool/)            | Decode resources, manifests, and rebuild APKs |
| [bundletool](https://developer.android.com/tools/bundletool) | Unpack AAB bundles into individual APKs       |

## Tips

- Focus on code under `smali/com/.../MainActivity.smali` or equivalent Java packages for flag logic.
- Inspect `AndroidManifest.xml` for exported components or permissions leakage.
- Search compiled code (`grep`) for known variable names like *flag*, *api_key*, or *secret*.
- Use **JADX** and **APKTool** together, **JADX** for readable code, **APKTool** for precise resource mapping.
