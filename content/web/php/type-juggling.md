---
title: "Type Juggling"
description: "Exploit weak comparisons and type coercion bugs in PHP applications to bypass equality or authentication checks."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["php", "type juggling", "loose comparison", "weak typing"]
authors: ["CTF.Support Team"]
summary: "Manipulate PHP's type comparison logic to satisfy weak conditions and trigger flag exposure."
aliases: ["/web/php/type-juggling/"]
slug: "type-juggling"
toc: true
---

## strcmp() Vulnerability

```php
if (strcmp($_POST["pass"], $flag) == 0) {
    echo "Flag: {$flag}";
} else {
    echo "Fail";
}
```

Supplying an array (`pass[]=1`) instead of a string bypasses the check, comparison fails silently and triggers the if block.

## Hash Comparison Example

``` php
if (isset($_GET['input1']) and isset($_GET['input2'])) {
  if ($_GET['input1'] == $_GET['input2']) {
    print 'Inputs can\'t be the same!';
  } else if (hash("sha256", $_GET['input1']) === hash("sha256", $_GET['input2'])) {
    print 'flag';
  } else {
    print 'Inputs can\'t match!';
  }
}
```

Passing arrays (`?a[]=1&b[]=2`) satisfies both conditions due to PHP’s weak typing.

## Tips

- Watch for `==` instead of `===`.
- Use arrays or null bytes for bypass.
- Exploit loose comparisons in login checks, hashes, and token verification.
