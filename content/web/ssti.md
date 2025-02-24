---
title: SSTI
---

SSTI (Server Side Template Injection) is a vulnerability that allows an attacker to inject code into a server-side template. This can result in remote code execution on the server.

For example, consider a web application that uses a template engine to render HTML. The application might allow users to provide input that is rendered in the template. If the application does not properly sanitize the input, an attacker could provide a template that includes code to execute on the server.

Consider the following example:

```python
from jinja2 import Template

def render_template(template, **context):
    t = Template(template)
    return t.render(**context)

template = input('Enter a template: ')
context = {'user': 'Alice'}
print(render_template(template, **context))
```

If the application does not properly sanitize the template, an attacker could provide a template like `{{ 7 * 7 }}` and the server would execute the code and return `49` to the attacker.
