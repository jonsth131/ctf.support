---
title: "Encodings"
description: "Identify and decode common and obscure data encodings such as Base64, Base32, URL, Hex, and Binary in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Cryptography"]
tags: ["encoding", "base64", "base32", "base58", "base91", "base65536", "url", "hex", "binary", "unicode", "cyberchef"]
authors: ["CTF.Support Team"]
summary: "Detect, recognize, and decode a wide range of data encodings — including base systems, URL encoding, and text transformations — often chained together in CTF puzzles."
aliases: ["/crypto/encodings/"]
slug: "encodings"
toc: true
---

## Introduction

Encodings convert data representation without providing cryptographic security.  
They are used to safely transmit, store, or display binary data as text.  
In CTFs, encodings are stacked to confuse solvers and obscure flags.

Common objective: **identify each encoding layer** > **decode** > **repeat** until plaintext or flag appears.

Example:

```text
UGF5bG9hZCBpcyBjb21wbGV0ZWx5IGJhc2U2NCBlbmNvZGVkLg==
```

Decoded: “Payload is completely base64 encoded.”

---

## Quick Reference – Common Encodings

| Encoding                  | Alphabet / Structure                      | Recognition Hints                                       |
|---------------------------|-------------------------------------------|---------------------------------------------------------|
| **Base64**                | A–Z a–z 0–9 +/ (= for padding)            | Ends with `=`  / `==`. ASCII letters + slashes          |
| **Base32**                | A–Z 2–7                                   | Uppercase letters and digits 2–7, often ends with `=`   |
| **Base58**                | BTC alphabet (excludes 0 O l I)           | No `+/=`, mixed case letters & digits                   |
| **Base85 / Ascii85**      | ASCII 33–117, `<~ … ~>`                   | `<~` prefix                                             |
| **Base91**                | Compact form, 94 printable chars          | No padding, very dense string                           |
| **Base100/256/65536**     | Emoji or Unicode characters               | Non‑ASCII glyphs, emojis, Chinese chars, text looks odd |
| **URL‑encoding**          | `%20`, `%41`, `+`: space                  | Percentage signs                                        |
| **Hex**                   | 0–9 A–F  pairs                            | Even length, only hex digits                            |
| **Binary (ASCII)**        | 0 / 1 (8‑bit groups)                      | Only 0s and 1s, usually multiple of 8                   |
| **Octal**                 | 0–7 digits                                | Text of 7 and 3 digits separated by spaces              |
| **Unicode/UTF‑16**        | Null bytes between letters (`h\x00i\x00`) | Appears in hexdumps/UTF‑16 files                        |
| **MIME/Quoted‑Printable** | `=XX` hex sequences                       | Email style encoding                                    |

---

## Detection and Layered Decoding

1. **Inspect text pattern**
   - Ends with `=`: Base64/Base32.  
   - Only 0–9A–F: Hex.  
   - `%20`, `%3D`: URL encoded.  
   - Binary (0/1 bytes): ASCII binary or bit cipher.

2. **Use automated detectors**
   - [CyberChef > Magic](https://gchq.github.io/CyberChef/)

3. **Apply layer‑by‑layer logic**
   - Always decode one layer > inspect > repeat:

```bash
echo "<string>" | base64 -d | xxd -p -r
```

---

## Tools

| Tool                                                                                              | Purpose                                              |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------|
| [CyberChef](https://gchq.github.io/CyberChef/)                                                    | Universal encode/decode platform (Magic auto‑detect) |
| [Better‑Converter Base65536](https://www.better-converter.com/Encoders-Decoders/Base65536-Decode) | Base65536 encoder and decoder                        |
| [`xxd`](https://linux.die.net/man/1/xxd)                                                          | Hex dump ↔ Binary converter                          |
| [`iconv`](https://linux.die.net/man/1/iconv)                                                      | Change text encodings (UTF‑16 ↔ UTF‑8)               |

---
