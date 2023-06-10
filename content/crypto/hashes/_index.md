---
title: Hashes
---

## MD5
Cracking MD5 hashes can be done using **John The Ripper** or **Hashcat**.

[Crackstation](https://crackstation.net) can be used to lookup hash values.

## Zip files
To crack password protected zip files, **fcrackzip** can be used.

To crack a zip file using a wordlist, the following command can be used.

`fcrackzip -u -v -D -p rockyou.txt <zip-file>`

Another way to crack a password protected zip file is to use **zip2john** to extract the hash and then using **John the Ripper** to crack the hash.


## PDF files
To crack password protected PDF files, **pdf2john** can be used to extract the hash.

After the hash is extracted, **John the Ripper** can be used to crack the hash.
