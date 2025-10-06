---
title: "QR & Barcodes"
description: "Reconstruct and decode fragmented or partial QR and barcode data from images or binaries."
categories: ["Misc"]
tags: ["qr", "barcode", "zxing", "stegsolve", "ctf"]
authors: ["CTF.Support Team"]
summary: "Use image manipulation and decoding tools to reassemble and read QR codes and barcodes from CTF puzzle artifacts."
aliases: ["/misc/qr-barcode/"]
slug: "qr-barcode"
toc: true
---

## Introduction

QR and barcode challenges appear in many CTFs — sometimes as direct image puzzles, hidden data within files, or partial fragments embedded in binaries or spectrograms.
Your task is usually to **reconstruct** the complete code, **decode** it accurately, and extract a hidden message or flag.

Common scenarios include:

- Damaged or cropped QR codes (split across multiple images)
- Barcode identification and scanning

---

## Quick Reference

| Task                                     | Command / Tool                                         |
|------------------------------------------|--------------------------------------------------------|
| Decode standard QR                       | `zbarimg code.png`                                     |
| Decode multiple QR at once               | `zbarimg *.png`                                        |
| Decode barcodes (EAN / Code128 / PDF417) | `zxing code.jpg`                                       |
| Capture code from camera                 | `zbarcam /dev/video0`                                  |
| Reconstruct pieces                       | Use `ImageMagick`, `GIMP`, or `ffmpeg` grid assembling |

## Tools

| Tool                                      | Purpose                                                          |
|-------------------------------------------|------------------------------------------------------------------|
| [ZBar](http://zbar.sourceforge.net/)      | CLI tool for decoding QR and various barcodes from images        |
| [ZXing](https://github.com/zxing/zxing)   | Cross‑platform barcode library with GUI & CLI decoders           |
| [QRazyBox](https://merri.cx/qrazybox/)    | Online QR code analysis, reconstruction, and error‑level editing |
| [ImageMagick](https://imagemagick.org/)   | Merge or enhance partial code segments                           |
| [GIMP / Photoshop](https://www.gimp.org/) | Manual alignment of code fragments                               |

---
