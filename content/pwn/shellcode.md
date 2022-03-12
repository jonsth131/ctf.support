---
Title: Shellcode
---

## Pwntools

Pwntools can be used to generate shellcode. The following example generates x64 shellcode for linux to execute `sh`.

``` python
from pwnlib import *

context.context(arch='amd64', os='linux')
shellcode = asm.asm(shellcraft.amd64.linux.sh())
```
