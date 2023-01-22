---
title: ElGamal
---

ElGamal is an assymetric key encryption algorithm for public-key cryptography. More info in [Wikipedia](https://en.wikipedia.org/wiki/ElGamal_encryption).

Detailed information on the algorithm can be found on [GeeksforGeeks](https://www.geeksforgeeks.org/elgamal-encryption-algorithm/).

Example decryption routine:

``` py
def decrypt(c1, c2, x, p):
    pt_int = (c2 * pow(c1, p-1-x, p)) % p
    pt_bytes = pt_int.to_bytes((pt_int.bit_length() + 7) // 8, "big")
    return pt_bytes.decode()
```
