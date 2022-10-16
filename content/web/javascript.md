---
title: JavaScript
---

## Obfuscation
Multiple obfuscation techniques can be deobfuscated using [de4js](https://lelinhtinh.github.io/de4js/).

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
