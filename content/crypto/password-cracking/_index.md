---
title: Password Cracking
---

## General
Hash cracking tools such as **John the Ripper** and **Hashcat** can be used when cracking passwords.

## Zip files
To crack password protected zip files, **fcrackzip** can be used.

To crack a zip file using a wordlist, the following command can be used.

`fcrackzip -u -v -D -p rockyou.txt <zip-file>`

Another way to crack a password protected zip file is to use **zip2john** to extract the hash and then using **John the Ripper** to crack the hash.

If the zip file is encrypted using legacy zip encryption, [bkcrack](https://github.com/kimci86/bkcrack) can be used to crack the password.

## PDF files
To crack password protected PDF files, **pdf2john** can be used to extract the hash.

After the hash is extracted, **John the Ripper** can be used to crack the hash.

## Keepass
To crack a Keepass vault password, we need **John the Ripper** to generate the hash by running `keepass2john <keepass-db-file>`.

After running this command, we get the hash that looks someting like this.
`mySecret:$keepass$*2*60000*0*b8bb35396aa2cc7b81c8d1e68ef3baf23d20f781406946c280230d100173e739*<SNIP>`

Now we can use either **John the Ripper** or **Hashcat** to try to crack the hash.
If we want to use **Hashcat** we first need to delete the first identifier of the hash, `mySecret:` so our hash looks like this.
`$keepass$*2*60000*0*b8bb35396aa2cc7b81c8d1e68ef3baf23d20f781406946c280230d100173e739*<SNIP>`

{{< tabs "keepass-cracking" >}}
{{< tab "John the Ripper" >}}
`john -wordlist=<wordlist> <hash>`
{{< /tab >}}
{{< tab "Hashcat" >}}
`hashcat -a 0 -m 13400 <wordlist> <hash>`
{{< /tab >}}
