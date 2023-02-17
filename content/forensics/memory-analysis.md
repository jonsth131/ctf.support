---
title: Memory Analysis
---

## Memory dumps
To analyze memory dumps, [Volatility2](https://www.volatilityfoundation.org/releases) or [Volatility3](https://www.volatilityfoundation.org/releases-vol3) can be used.

## Volatility 2

### Image Identification
To identify and get a memory profile for a memory dump using Volatility 2, run `volatility -f <memdump> imageinfo`.
