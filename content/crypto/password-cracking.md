---
title: "Password Cracking"
description: "Crack password-protected archives, databases, and encrypted files using John the Ripper, Hashcat, and specialized utilities."
date: 2025-10-06
draft: false
categories: ["Cryptography"]
tags: ["password cracking", "zip", "pdf", "keepass", "7zip", "office", "ssh", "wordlists", "john", "hashcat", "brute force"]
authors: ["CTF.Support Team"]
summary: "Recover passwords from protected files and applications using common cracking tools, targeted wordlists, and automation scripts."
aliases: ["/crypto/password-cracking/"]
slug: "password-cracking"
toc: true
---

## Introduction

Password-protected archives and encrypted files appear in many CTF challenges.  
Your goal is to recover the cleartext password or use a derived key to unlock the flag.  

Typical targets include:

- ZIP / RAR / 7‑Zip archives  
- PDF and Office documents  
- Password databases (KeePass, Bitwarden, etc.)  
- System hashes (/etc/shadow, SAM dumps)  
- SSH private keys with passphrases

---

## Methodology

| Phase                     | Action                                                                            |
|---------------------------|-----------------------------------------------------------------------------------|
| **1 – Identification**    | Determine file type (`file`, `binwalk`, `7z l`, `pdfinfo`, `john --list=formats`) |
| **2 – Hash Extraction**   | Dump hash representation (e.g. `zip2john`, `pdf2john`, `keepass2john`)            |
| **3 – Analysis & Attack** | Use `john` or `hashcat` with wordlists or rules.                                  |
| **4 – Validation**        | Verify decrypted file / flag successfully opens.                                  |

Common wordlists: `rockyou.txt`, `darkc0de.txt`, `SecLists/Passwords/`.

---

## Tools

| Tool                                                                                | Purpose                                   |
|-------------------------------------------------------------------------------------|-------------------------------------------|
| [John the Ripper](https://www.openwall.com/john/)                                   | Multi-format hash and archive cracker     |
| [Hashcat](https://hashcat.net/hashcat/)                                             | GPU‑accelerated cracking tool             |
| [fcrackzip](https://github.com/hyc/fcrackzip)                                       | Fast ZIP brute‑forcer                     |
| [bkcrack](https://github.com/kimci86/bkcrack)                                       | Legacy ZIP cipher plaintext attack        |
| [7z2hashcat](https://github.com/philsmd/7z2hashcat)                                 | Extract hashes from 7‑Zip archives        |
| [keepass2john](https://www.openwall.com/john/)                                      | Extract hash from KeePass .kdbx           |
| [ssh2john.py](https://github.com/openwall/john/blob/bleeding-jumbo/run/ssh2john.py) | Convert SSH private key to crackable hash |

---

## Archive Files

### ZIP

#### 1. Dictionary Attack

```bash
fcrackzip -u -v -D -p rockyou.txt secret.zip
```

#### 2. John / Hashcat

```bash
zip2john secret.zip > zip.hash
john --wordlist=rockyou.txt zip.hash
# or
hashcat -m 17200 -a 0 zip.hash rockyou.txt
```

#### 3. Known‑Plaintext (One Known File)

```bash
bkcrack -C encrypted.zip -c cipher -P plain.zip -p plain
```

### 7‑Zip (.7z)

```bash
7z2hashcat secret.7z > hash.txt
hashcat -m 11600 -a 0 hash.txt rockyou.txt
```

---

## PDF Files

```bash
pdf2john secret.pdf > pdf.hash
john --wordlist=rockyou.txt pdf.hash
# or
hashcat -m 10500 -a 0 pdf.hash rockyou.txt
```

---

## KeePass Database

```bash
keepass2john vault.kdbx > keepass.hash
john --wordlist=rockyou.txt keepass.hash
# or
hashcat -m 13400 -a 0 keepass.hash rockyou.txt
```

---

## SSH Private Keys

```bash
python3 ssh2john.py id_rsa > ssh.hash
john --wordlist=rockyou.txt ssh.hash
# or
hashcat -m 22911 -a 0 ssh.hash rockyou.txt
```

---

## System Hashes

### Linux Shadow File

```bash
john --wordlist=rockyou.txt --format=sha512crypt shadow.txt
```

### Windows SAM / NTLM

```bash
hashcat -m 1000 -a 0 hashes.txt rockyou.txt
```

---

## Strategies

| Technique                 | Description                                                                     |
|---------------------------|---------------------------------------------------------------------------------|
| Hybrid Attack             | Combine wordlists + mutations (john --rules or `hashcat -r rules/best64.rule`). |
| Mask Attack               | Specify pattern (e.g., `?l?l?l?d?d` - 3 letters + 2 digits).                    |
| Brute Force               | Use for short passwords (< 6 chars), limit length for efficiency.               |
| Probability‑Based Guesses | Use rules mirroring human choices (CapFirst123!).                               |
