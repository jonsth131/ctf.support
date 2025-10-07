---
title: "Hashes"
description: "Learn to identify, crack, and verify hashes such as MD5, SHA‑1, and SHA‑256 using online and offline tools."
categories: ["Cryptography"]
tags: ["hashes", "md5", "sha1", "hashcat", "john", "crackstation"]
authors: ["CTF.Support Team"]
summary: "Crack or look up hashes with wordlists and online databases."
aliases: ["/crypto/hashes/"]
slug: "hashes"
toc: true
---

## Introduction

Hashes are **one‑way functions** that transform data into fixed‑length strings.
They are widely used for **password storage, integrity verification, and digital signatures**, but in CTFs, they often protect flags or credentials.

Unlike encryption, hashing is non‑reversible.
To retrieve the original input, attackers must use **bruteforce**, **dictionary attacks**, or **precomputed lookup tables**.

CTF challenges commonly include hash strings from algorithms such as `MD5`, `SHA‑1`, and `SHA‑256`.
These can often be cracked using **wordlists** (`rockyou.txt`, `SecLists`) with tools like **John the Ripper**, **Hashcat**, or online databases such as [CrackStation](https://crackstation.net).

Typical goals in a hash challenge:

- Identify the hash algorithm (length and structure give hints)
- Lookup or crack the hash value
- Verify the output against flag patterns or password formats

## Tools

| Tool                                     | Purpose                |
|------------------------------------------|------------------------|
| `John the Ripper` / `hashcat`            | Hash cracking tools.   |
| [CrackStation](https://crackstation.net) | Look up hashes online. |

## Example

```bash
# Using John the Ripper
john -wordlist=rockyou.txt hash.txt

# Using Hashcat
hashcat -a 0 -m 0 hash.txt rockyou.txt
```
