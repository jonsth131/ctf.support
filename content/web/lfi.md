---
title: LFI
---

LFI (Local File Inclusion) is a vulnerability that occurs when an application includes a file based on user-supplied input. As a result, an attacker can read sensitive files on the server.

For example, consider the following URL:

```
https://example.com/?page=home
```

An attacker can change the `page` parameter to access sensitive files:

```
https://example.com/?page=../../../../etc/passwd
```

### Exploitation

To exploit LFI, follow these steps:

1. Identify the vulnerable parameter.
2. Change the parameter value to access sensitive files.
3. Analyze the response to confirm the vulnerability.

### Interesting files

Here are some interesting files to check for during LFI:

``` text
/etc/passwd
/etc/shadow
/etc/hosts
/var/log/auth.log
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/www/html/index.php
/var/www/html/config.php
/var/www/html/.env
/var/www/html/.htaccess
/var/www/html/.git/config
/var/www/html/.git/HEAD
/proc/self/environ
/proc/self/cmdline
```
