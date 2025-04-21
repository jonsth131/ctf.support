---
title: JavaScript
---

{{< toc >}}

## Obfuscation
Multiple obfuscation techniques can be deobfuscated using [de4js](https://lelinhtinh.github.io/de4js/), or [JavaScript Deobfuscator](https://deobfuscate.io/).

Deobfuscating [Obfuscator.io](https://obfuscator.io/) can be done using [Obfuscator.io Deobfuscator](https://obf-io.deobfuscate.io/).

## Vulnerable Packages
If there's access to `package-lock.json`, a check for vulnerable packages can be done by running the command `npm audit` in the same directory as `package-lock.json`.

``` text
# npm audit report

ion-parser  *
Severity: critical
ion-parser Prototype Pollution when malicious INI file submitted to application that parses with `parse` - https://github.com/advisories/GHSA-7vrv-5m2h-rjw9
fix available via `npm audit fix`
node_modules/ion-parser

1 critical severity vulnerability
```

Other tools like [Snyk CLI](https://github.com/snyk/cli) can be used to achieve the same results.

## Prototype Pollution
Prototype pollution is a vulnerability that allows an attacker to modify the prototype of an object and potentially execute arbitrary code.

For example, consider the following code:

```javascript
const merge = require('deepmerge');

let obj = {};
let payload = '{"__proto__": {"polluted": true}}';

let result = merge(obj, JSON.parse(payload));

console.log(obj.polluted); // true
```

In this example, the `merge` function merges the `obj` object with the `payload` object. The `payload` object has a property `__proto__` that modifies the prototype of the `obj` object. As a result, the `obj` object now has a `polluted` property.
