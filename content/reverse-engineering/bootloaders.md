---
title: Bootloaders
---

To reverse egineer a bootloader, QEMU and GDB can be used.

For example, if you have a bootloader of `DOS/MBR boot sector` type, you can use the following command to run it in QEMU:

```bash
qemu-system-i386 -drive file=bootloader.bin,format=raw
```

To debug the bootloader, you can use the following command:

```bash
qemu-system-i386 -drive file=bootloader.bin,format=raw -s -S
```

Then, you can connect GDB to QEMU using the following command:

```bash
gdb
target remote localhost:1234
```
