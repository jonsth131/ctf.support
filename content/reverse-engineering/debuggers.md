---
title: Debuggers
---

## gdb
The GNU Debugger is used to debug \*NIX applications.

Installation on debian based systems `apt install gdb`

### Usage
To attach *gdb* to a running process, the following command can be used.

``` text
gdb -p `pidof <name of program>`
```

### pwndbg
To make gdb more usable, [pwndbg](https://github.com/pwndbg/pwndbg) can be used.

To install pwndbg run the following.

``` text
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

## radare2
[Radare2](https://rada.re/) is a free re toolchain.

To install radare2, run the following.

``` text
git clone https://github.com/radareorg/radare2
cd radare2 ; sys/install.sh
```

## x64dbg
[x64dbg](https://x64dbg.com/) is an open-source x64/x32 debugger for Windows.

## Immunity Debugger
[Immunity Debugger](https://www.immunityinc.com/products/debugger/) is a Windows debugger.

## OllyDbg
[OllyDbg](http://www.ollydbg.de/) is a 32bit Windows debugger.
