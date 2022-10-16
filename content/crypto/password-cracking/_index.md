---
title: Password Cracking
---

## General
Hash cracking tools such as **John the Ripper** and **Hashcat** can be used when cracking passwords.

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
`hashcat -a 0 -m 13400 <worlist> <hash>`
{{< /tab >}}
