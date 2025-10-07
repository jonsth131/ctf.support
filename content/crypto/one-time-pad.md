---
title: "One‑Time Pad (OTP)"
description: "Understand how the One‑Time Pad achieves perfect secrecy and why key reuse (Two‑Time Pad) compromises security in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Cryptography"]
tags: ["one‑time‑pad", "otp", "two‑time‑pad", "xor", "stream cipher", "ctf", "cryptanalysis"]
authors: ["CTF.Support Team"]
summary: "Learn the theory behind the One‑Time Pad and exploit the Two‑Time Pad weakness when keys are reused — a common symmetric vulnerability in CTF crypto challenges."
aliases: ["/crypto/one-time-pad/"]
slug: "one-time-pad"
toc: true
---

## Introduction

The **One‑Time Pad (OTP)** is a symmetric encryption technique that provides *perfect secrecy* when implemented correctly.

First described by **Frank Miller (1882)** and later patented by **Gilbert Vernam (1917)**, it remains a theoretical benchmark for absolute security.

---

## Concept

OTP uses a **key that is at least as long as the message** and combines it with plaintext using the bitwise **XOR (⊕)** operation.

### Process Overview

| Step                   | Description                                                   |
|------------------------|---------------------------------------------------------------|
| **1 - Key Generation** | Generate a truly random key equal in length to the plaintext. |
| **2 - Encryption**     | Each plaintext byte ⊕ key byte = ciphertext byte.             |
| **3 - Decryption**     | Ciphertext ⊕ key = plaintext (recover original message).      |

### Example (Python)

```python
from os import urandom

msg = b"CTF{perfect_secrecy}"
key = urandom(len(msg))  # random key of same length
enc = bytes([m ^ k for m, k in zip(msg, key)])
dec = bytes([e ^ k for e, k in zip(enc, key)])
print("Cipher:", enc.hex())
print("Recovered:", dec.decode())
```

Because `x ⊕ x = 0` and `x ⊕ 0 = x`, encryption and decryption are identical operations.

---

## Perfect Secrecy

OTP provides theoretical security because:

- Each bit of ciphertext can correspond to any possible plaintext bit given some key.
- There’s no correlation between ciphertext and plaintext without the key.

However, in practice, OTP is hard to use safely due to key distribution and randomness requirements.

---

## Limitations

- **Key Length**: Must be at least as long as the plaintext.
- **Key Randomness**: Must be truly random (not pseudorandom).
- **Key Reuse**: A key may be used only once; otherwise, secrecy collapses.
- **Distribution**: Secure exchange and storage of one‑time keys are difficult.

---

## Exploiting Two‑Time Pad

A **Two‑Time Pad** occurs when the same OTP key is reused for multiple messages.

This creates exploitable relationships between ciphertexts.

Given two ciphertexts:

```text
c1 = m1 ⊕ k
c2 = m2 ⊕ k
```

XOR the two ciphertexts:

```text
c1 ⊕ c2 = (m1 ⊕ k) ⊕ (m2 ⊕ k) = m1 ⊕ m2
```

The key cancels out, leaving only `m1 ⊕ m2`.

If part of `m1` is known or guessable (known plaintext), you can recover `m2`.

### Exploit Example

```python
def two_time_pad_attack(c1, c2, known_plain):
    xored = bytes([a ^ b for a, b in zip(c1, c2)])
    recovered = bytes([x ^ y for x, y in zip(xored, known_plain)])
    return recovered

# Suppose attacker knows plaintext of first message
plaintext1 = b"flag{"
recovered = two_time_pad_attack(cipher1, cipher2, plaintext1)
print("Recovered fragment:", recovered)
```

Using language patterns or known tokens (`"CTF{"`, `"flag{"`, etc.), an attacker can iteratively reconstruct the other message.
