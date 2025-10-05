---
title: "Password Cracking"
description: "Crack password‑protected archives, databases, and PDFs using John the Ripper, Hashcat, and specialized tools."
categories: ["Cryptography"]
tags: ["password cracking", "zip", "pdf", "keepass", "fcrackzip", "john", "hashcat"]
authors: ["CTF.Support Team"]
summary: "Recover passwords from protected files using common cracking suites and wordlists."
aliases: ["/crypto/password-cracking/"]
slug: "password-cracking"
toc: true
---

## Introduction

Password‑protected archives and documents are frequent CTF challenges.

This section covers offline cracking methods for ZIP, PDF, and KeePass vaults.

## ZIP files

```bash
fcrackzip -u -v -D -p rockyou.txt secret.zip
```

Or extract hashes with `zip2john` and‑crack using **John the Ripper** or **Hashcat**.

For legacy ZIP encryption, try [bkcrack](https://github.com/kimci86/bkcrack).

## PDF Files

```bash
pdf2john secret.pdf > hashes.txt
john hashes.txt
```

## KeePass Database

```bash
keepass2john vault.kdbx > keepass.hash

# Crack using John the Ripper
john -wordlist=rockyou.txt keepass.hash

# Crack using hashcat
hashcat -a 0 -m 13400 rockyou.txt keepass.hash
```
