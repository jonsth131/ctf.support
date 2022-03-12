---
title: Format Strings
---

## Format string types

|Specifier|Description|
|-|-|
|`%s`|String|
|`%p`|Address of pointer to void void * |
|`%x` or `%X`|Hexadecimal|

The format `%1$p` can be used to leak positional data on the stack, where `1` is the index.
