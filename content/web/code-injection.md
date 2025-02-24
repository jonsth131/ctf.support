---
title: Code Injection
---

Code injection is a vulnerability that allows an attacker to execute arbitrary code on a server. This can be used to read, modify, or delete data, or even to execute commands on the underlying operating system.

For example, consider a web application that uses a function to process user input. The application might call the function with user input, like so:

```python
def process_input(data):
    exec(data)
    return

data = input('Enter some data: ')
process_input(data)
```

If the application does not properly sanitize the input, an attacker could provide data like `import os;os.system('ls')` and the server would execute the command and list the files in the current directory.

