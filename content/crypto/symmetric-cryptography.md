---
title: "Symmetric Cryptography"
description: "Learn about symmetric encryption schemes like AES, DES, and RC4. Understand key reuse issues, stream cipher weaknesses, and how to break improperly used symmetric systems in CTF challenges."
date: 2025-10-05
draft: false
categories: ["Cryptography"]
tags: ["symmetric cryptography", "block cipher", "stream cipher", "aes", "rc4", "crypto", "cryptanalysis"]
authors: ["CTF.Support Team"]
summary: "Understand symmetric encryption fundamentals and explore common block and stream ciphers such as AES and RC4, including analysis and exploitation techniques used in CTF challenges."
aliases: ["/crypto/symmetric-cryptography/"]
slug: "symmetric-cryptography"
toc: false
---

## Introduction

**Symmetric cryptography** uses *one shared secret key* for both encryption and decryption.
It’s fast, efficient, and forms the basis of most real‑world encryption (e.g., TLS record layer, disk encryption, VPNs) and many CTF crypto tasks.

Typically, challenges involve:

- Recovering plaintext when an algorithm or mode is mis‑used
- Exploiting key reuse (especially in stream ciphers)
- Identifying encryption modes by pattern analysis
- Re‑implementing simple ciphers from network forensic data

---

### Tools

| Tool                                                                      | Purpose                                                      |
|---------------------------------------------------------------------------|--------------------------------------------------------------|
| [CyberChef](https://gchq.github.io/CyberChef/)                            | RC4 decryption, base64 processing, stream cipher experiments |
| [PyCryptodome](https://pycryptodome.readthedocs.io/)                      | Implementation of RC4 (`ARC4`) for scripting CTF challenges  |
| [RC4 Implementation Playground](https://asecuritysite.com/encryption/rc4) | Visual demonstration of KSA and PRGA                         |

---

## Block Ciphers

Block ciphers operate on fixed‑size blocks of data (usually 128 bits).

Common block ciphers:

- **AES (Advanced Encryption Standard)**: modern standard for secure encryption
- **DES / 3DES**: legacy algorithms still seen in older challenges
- **Blowfish, Twofish, IDEA**: alternative historical implementations

Encryption example:

```text
ciphertext = Encrypt(key, plaintext)
plaintext  = Decrypt(key, ciphertext)
```

### Modes of Operation

Because block ciphers work on fixed blocks, they’re combined with **modes of operation** to handle arbitrary‑length data.

| Mode | Description                                        | Common Weakness in CTFs                                        |
|------|----------------------------------------------------|----------------------------------------------------------------|
| ECB  | Each block encrypted independently.                | Leaks patterns, identical plaintexts -> identical ciphertexts. |
| CBC  | XORs each block with previous ciphertext.          | Padding oracle attacks via IV or padding error oracle.         |
| CTR  | Turns block cipher into stream cipher via counter. | Key/nonce reuse causes keystream leakage.                      |
| GCM  | Authenticated encryption (AEAD).                   | Usually secure, unless nonces are reused.                      |

---

## Stream Ciphers

Stream ciphers encrypt data byte‑by‑byte using a pseudorandom keystream generated from a secret key.

Typical pattern:

```text
ciphertext[i] = plaintext[i] XOR keystream[i]
```

If the same keystream is used for two messages (with same key and nonce), you can recover plaintexts by XORing ciphertexts together:

```text
c1 ⊕ c2 = p1 ⊕ p2
```

Key reuse is fatal for stream ciphers, a popular CTF theme.

---

## RC4

### Overview

**RC4 (Rivest’s Cipher 4)** is a stream cipher designed in 1987 by Ron Rivest.
It became widely used in protocols like SSL/TLS and WEP, though it’s now considered insecure.

RC4 is simple and fast, using two reversible steps:

1. **Key‑Scheduling Algorithm (KSA)**: initializes a permutation of 256 bytes based on the key.
2. **Pseudo‑Random Generation Algorithm (PRGA)**: outputs one byte of keystream per loop.

### Algorithm Outline

```python
def rc4_encrypt(key, plaintext):
    # Key-Scheduling Algorithm (KSA)
    S = list(range(256))
    j = 0
    for i in range(256):
        j = (j + S[i] + key[i % len(key)]) % 256
        S[i], S[j] = S[j], S[i]
    # Pseudo-Random Generation Algorithm (PRGA)
    i = j = 0
    out = []
    for ch in plaintext:
        i = (i + 1) % 256
        j = (j + S[i]) % 256
        S[i], S[j] = S[j], S[i]
        K = S[(S[i] + S[j]) % 256]
        out.append(ch ^ K)
    return bytes(out)
```

Encryption and decryption are identical operations.

---

### Common Weaknesses

| Weakness                   | Description                                                                 | Exploitation Idea                                   |
|----------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|
| Key reuse                  | Using the same key/IV across messages leaks plaintext relationships.        | XOR ciphertexts to eliminate keystream.             |
| Keystream bias             | Early RC4 output bytes are biased (non‑random).                             | Statistical attacks recover key bytes.              |
| Known plaintext            | If you know part of the plaintext, you can recover corresponding keystream. | Generate keystream → decrypt rest of message.       |
| Predictable key derivation | Weak keys with small entropy (e.g., short passwords).                       | Brute‑force keystream and compare known ciphertext. |

---

### Sample CTF Decryption Workflow

```python
from Crypto.Cipher import ARC4
import base64

key = b"ctfsecret"
cipher_b64 = "O1oJVlA1CDU1Ih1KPw=="
cipher = base64.b64decode(cipher_b64)

rc4 = ARC4.new(key)
plaintext = rc4.decrypt(cipher)
print(plaintext.decode())
```

You can replace the `key` when testing different possible passwords or derived keys.
