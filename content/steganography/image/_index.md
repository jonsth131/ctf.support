---
title: Image
resources:
  - name: stegsolve-normal
    src: "stegsolve-1.png"
    title: Normal Image
  - name: stegsolve-revealed
    src: "stegsolve-2.png"
    title: Revealed Flag
---

## Resources
[https://aperisolve.fr/](https://aperisolve.fr/)

[https://stegonline.georgeom.net/](https://stegonline.georgeom.net/i)

[https://futureboy.us/stegano/decinput.html](https://futureboy.us/stegano/decinput.html)

[https://stylesuxx.github.io/steganography/](https://stylesuxx.github.io/steganography/)

## zsteg
Extract data from BMP and PNG images.

Installation:
``` text
gem install zsteg
```

To use **zteg** run `zteg <filename>`.

To check all bytes run `zteg -a <filename>`.

To show specific bytes run `zteg -E b8,rgb,lsb,xy <filename>`.

## steghide
`steghide extract -sf <image-file>`

## stegseek
Stegseek is used to crack `steghide` passwords.

Usage with wordlist:
`stegseek -sf <image-file> -wl rockyou.txt`

## iSteg
[iSteg](https://github.com/rafiibrahim8/iSteg) is a image LSB steganography tool that exists in a CLI and a GUI version.

## Stegsolve
[Stegsolve](http://www.caesum.com/handbook/Stegsolve.jar) is used to solve steganography challenges. It can be used to view different bit planes, and extract data from bit planes.

{{< img name="stegsolve-normal" size="small" >}}

{{< img name="stegsolve-revealed" size="small" >}}
