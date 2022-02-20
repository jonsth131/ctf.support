---
title: RSA
---

## Factoring
To factor n, the following websites can be used.

[FactorDB](http://factordb.com/)

[Integer factorization calculator](https://www.alpertron.com.ar/ECM.HTM)

## Known p and q
If p and q is known, either by factoring n or that they are given, the following Python script can decrypt the ciphertext.

``` python
#!/usr/bin/env python3

from Crypto.Util.number import inverse

c = 264927351071199256392067715088101727274736234498820
n = 324724323060034233289551751185171379596941511493891
e = 65537
p = 25001545096244227516337
q = 12988170203481337861511552243

phi = (p - 1) * (q - 1)
d = inverse(e, phi)

m = pow(c, d, n)

print(bytes.fromhex(hex(m)[2:]).decode('utf-8'))
```

## RsaCtfTool
RsaCtfTool is able to run a wide range of attacks on RSA ciphers.

More information on the attacks and installation instructions can be found on the GitHub page.

[RsaCtfTool GitHub](https://github.com/Ganapati/RsaCtfTool)