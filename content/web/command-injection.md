---
title: Command Injection
---

Command injection is a vulnerability that allows an attacker to execute arbitrary commands on a server. This can be used to read, modify, or delete data, or even to execute commands on the underlying operating system.

For example, consider a web application that allows users to upload files. The application might use a command to process the uploaded file, like so:

```python
import subprocess

def process_file(filename):
    return subprocess.check_output(['cat', filename])

filename = input('Enter a filename: ')
print(process_file(filename))
```

If the application does not properly sanitize the input, an attacker could provide a filename like `file.txt; ls` and the resulting command would be:

```sh
cat file.txt; ls
```

This command would read the contents of `file.txt` and then list the files in the current directory.
