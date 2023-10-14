---
title: Audio
resources:
  - name: spectrogram
    src: "spectrogram.png"
    title: Spectrogram in Audacity
---

## Spectrogram
Using either Audacity or Sonic Visualiser to show the spectrogram view of the audio.
{{< img name="spectrogram" size="small" >}}

## LSB
LSB steganography for wave-files can be decoded using the [stegolsb](https://github.com/ragibson/Steganography) tools.

The following command will extract 100 bytes from the input file.

`stegolsb wavsteg -i <input-file> -o output.txt -b 100`
