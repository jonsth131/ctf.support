---
title: "Asymmetric Cryptography"
description: "Understand and exploit asymmetric encryption systems like RSA and ElGamal used in CTF challenges. Learn key generation, factorization, and private key recovery techniques."
date: 2025-10-05
draft: false
categories: ["Cryptography"]
tags: ["asymmetric cryptography", "rsa", "elgamal", "public key", "crypto", "cryptanalysis"]
authors: ["CTF.Support Team"]
summary: "Learn the fundamentals of asymmetric encryption and how to analyze and attack RSA and ElGamal implementations in CTF cryptography challenges."
aliases: ["/crypto/asymmetric-cryptography/"]
slug: "asymmetric-cryptography"
toc: false
---

## Introduction

Asymmetric cryptography uses **two keys**, a **public key** for encryption and a **private key** for decryption.
This principle underpins secure communication, digital signatures, and many CTF crypto challenges.

Common implementations include:

- **RSA (Rivest–Shamir–Adleman):** uses large primes and modular arithmetic
- **ElGamal:** relies on the Discrete Logarithm Problem in modular groups

CTF tasks may involve factoring RSA `n`, finding weak keys, recovering messages from partial information, or reversing ElGamal ciphertext with known parameters.

## RSA

**RSA (Rivest - Shamir - Adleman)** is based on the mathematical difficulty of factoring the product of two large prime numbers `p` and `q`.

A public key consists of:

- `n = p × q` - the modulus
- `e` - the public exponent

The private key uses:

- `d = e⁻¹ mod φ(n)` where `φ(n) = (p‑1)(q‑1)`

Encryption and decryption:

```text
c = m^e mod n
m = c^d mod n
```

### Tools

| Tool                                                                     | Purpose                                                                |
|--------------------------------------------------------------------------|------------------------------------------------------------------------|
| [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)                     | Multi‑attack suite for RSA (factoring, Wiener's, common‑modulus, etc.) |
| [FactorDB](http://factordb.com/)                                         | Online database of factored numbers.                                   |
| [Integer factorization calculator](https://www.alpertron.com.ar/ECM.HTM) | Quickly factor moderate‑sized RSA moduli.                              |
| [RSA calculator](https://www.tausquared.net/pages/ctf/rsa.html)          | Key generation, encryption, decryption in browser.                     |

### Example: Decrypting with Known p and q

```python
#!/usr/bin/env python3

from Crypto.Util.number import long_to_bytes

if __name__ == "__main__":
    c = 8533139361076999596208540806559574687666062896040360148742851107661304651861689
    p = 1617549722683965197900599011412144490161
    q = 475693130177488446807040098678772442581573
    n = p * q
    e = 65537

    phi = (p - 1) * (q - 1)
    d = pow(e, -1, phi)
    m = pow(c, d, n)
    pt = long_to_bytes(m).decode()

    print(pt)
```

## ElGamal

ElGamal encryption is another asymmetric system based on the Discrete Logarithm Problem (DLP) in modular arithmetic.

It is often used in cryptographic schemes that require probabilistic encryption (randomization on each encryption).

Key elements:

- `p`: large prime number (defines group)
- `g`: generator of the multiplicative group
- `x`: private key (random)
- `h = g^x mod p`: public key component

Encryption of plaintext `m`:

```text
Choose random k.
c1 = g^k mod p
c2 = (m × h^k) mod p
Ciphertext = (c1, c2)
```

Decryption:

```text
m = c2 × (c1^(p‑1‑x)) mod p
```

### Example Decryption Routine

```python
def decrypt(c1, c2, x, p):
    pt_int = (c2 * pow(c1, p-1-x, p)) % p
    pt_bytes = pt_int.to_bytes((pt_int.bit_length() + 7) // 8, "big")
    return pt_bytes.decode()
```

Usage:

```python
p  = 2357
c1 = 2185
c2 = 2005
x  = 1751

plaintext = decrypt(c1, c2, x, p)
print(plaintext)
```

When `p`, `g`, `h`, and either `x` (private key) or `k` (ephemeral key) are leaked, ElGamal encryption becomes breakable.
