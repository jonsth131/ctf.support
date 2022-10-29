---
Title: Shellcode
---

## Pwntools

Pwntools can be used to generate shellcode. The following example generates x64 shellcode for Linux to execute `sh`.

``` python
from pwnlib import *

context.context(arch='amd64', os='linux')
shellcode = asm.asm(shellcraft.amd64.linux.sh())
```

## MSFvenom

Another tool to generate shellcode is `msfvenom`, which is a part of the **Metasploit** framework.

To generate shellcode just run the command `msfvenom -p <payload>`.
To list all available payloads run `msfvenom --list payloads`.

For other options run `msfvenom --help`.
