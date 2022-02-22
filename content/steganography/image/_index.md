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
`steghide`

## Stegsolve
[Stegsolve](http://www.caesum.com/handbook/Stegsolve.jar) is used to solve steganography challenges. It can be used to view different bit planes, extract data from bit planes.

{{< img name="stegsolve-normal" size="small" >}}

{{< img name="stegsolve-revealed" size="small" >}}
