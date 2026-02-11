---
title: "Hashes"
date: 2026-02-11
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

## Tools & Resources

| Tool                                              | Purpose                |
|---------------------------------------------------|------------------------|
| [John the Ripper](https://www.openwall.com/john/) | Hash cracking tool.    |
| [Hashcat](https://hashcat.net/hashcat/)           | Hash cracking tool.    |
| [CrackStation](https://crackstation.net)          | Look up hashes online. |

## Cracking Hashes

```bash
# Using John the Ripper
john -wordlist=rockyou.txt hash.txt

# Using Hashcat
hashcat -a 0 -m 0 hash.txt rockyou.txt
```

---

## Length Extension Attack

A **length extension attack** exploits how hashes such as **MD5**, **SHA‑1**, and **SHA‑256** process input data in blocks (the Merkle–Damgård construction).  
If a system computes a message authentication code as `hash(secret + message)`, and you know the hash output and the length of `secret`, you can append extra data and forge a valid hash for `secret + message + extra` without knowing the secret.

---

### How It Works

1. The hash algorithm processes data in fixed‑size blocks (e.g. 512 bits for MD5/SHA‑1).  
2. When the final hash is produced, internal state depends only on the number of bits hashed so far.  
3. An attacker can continue the hash calculation from this intermediate state by injecting data as though more blocks followed.  
4. The prepended “secret” is never known but its length must be guessed.

This attack **does not** work if the application uses HMAC (`HMAC(secret, message)`), which protects against length extension.

---

### CTF Use Case

A web service might validate a query string with a hash:

```text
/verify?user=guest&sig=6d5f807e23db210bc254a28be2d6759a0f5f5d99
```

where `sig = MD5(secret || data)`

You can forge a different valid signature for `user=guest&admin=true` by performing a length extension attack.

---

### Example Using `hashpump`

[`hashpump`](https://github.com/miekrr/HashPump) implements this automatically.

```bash
HashPump -s 6d5f807e23db210bc254a28be2d6759a0f5f5d99 --data count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo -a &waffle=liege -k 14
```

Output:

```text
0e41270260895979317fff3898ab85668953aaa2 count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02(&waffle=liege
```

Use the new hash and payload in place of the original to bypass security checks.
