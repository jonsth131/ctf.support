---
title: PHP
---

## LFI

### php://filter
To return the base64 of a file, like the source code of a page, `php://filter` can be used like this:

`view.php?page=php://filter/convert.base64-encode/resource=index.php`

## Type Juggling
### strcmp
If `strcmp` is used like this

```php
if (strcmp($_POST["pass"], $flag) == 0) {
    echo "Flag: {$flag}";
} else {
    echo "Fail";
}
```

It's vulnerable to type juggling. Changing the `pass` parameter to an array like `pass[]=1` will execute the `true` block.

