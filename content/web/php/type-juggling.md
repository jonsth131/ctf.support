---
title: Type Juggling
---

## strcmp
If `strcmp` is used like this

```php
if (strcmp($_POST["pass"], $flag) == 0) {
    echo "Flag: {$flag}";
} else {
    echo "Fail";
}
```

It's vulnerable to type juggling. Changing the `pass` parameter to an array like `pass[]=1` will execute the `true` block.

## Different inputs, matching hashes
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

In this example we can get to the `print 'flag';` statement by type juggling the parameters. For example changing the parameters to arrays, `?input1[]=1&input2[]=2`. This will satisfy all checks and print the flag.
