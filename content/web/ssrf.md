---
title: SSRF
---

SSRF (Server Side Request Forgery) is a vulnerability that allows an attacker to force a server to issue requests on their behalf. This can be used to access internal resources, scan ports, and more.

For example, consider a web application that allows users to upload images. The application might fetch the image from a URL provided by the user. If the application does not properly validate the URL, an attacker could provide a URL that points to an internal resource, such as `http://localhost/admin`, and the server would fetch that resource on behalf of the attacker.

Consider the following example:

```python
import requests

def fetch_url(url):
    response = requests.get(url)
    return response.text

url = input('Enter a URL: ')
print(fetch_url(url))
```

If the application does not properly validate the URL, an attacker could provide a URL like `http://localhost/admin` and the server would fetch the admin page and return it to the attacker.
