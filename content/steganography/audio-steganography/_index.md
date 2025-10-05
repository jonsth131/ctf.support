---
title: "Audio Steganography"
description: "Inspect sound files and spectrograms for hidden data or LSB-encoded messages using Audacity, Sonic Visualiser, and stegolsb."
date: 2025-10-05
draft: false
weight: 20
categories: ["Steganography"]
tags: ["audio steganography", "spectrogram", "lsb", "stegolsb", "audacity", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn how to reveal hidden messages in audio through spectral inspection, waveform analysis, and LSB extraction techniques."
aliases: ["/steganography/audio/"]
slug: "audio"
resources:
  - name: spectrogram
    src: "spectrogram.png"
    title: "Spectrogram View in Audacity"
toc: true
---

## Introduction

Audio steganography hides information inside sound files by manipulating either the frequency domain (spectrogram) or the sample data (LSB).

In CTF challenges, hidden data may appear visually in a spectrogram or may need to be extracted from the waveform itself.

Common techniques:

- Spectrogram encoding: text or images are modulated into frequencies.
- Least Significant Bit (LSB): tiny bit changes inside PCM samples carry a binary payload.
- Appended data: extra files hidden after the audio data.

## Quick Reference

- View spectrogram: Audacity -> View -> Spectrogram
- View spectrogram (alternative): Sonic Visualiser -> Layer -> Add Spectrogram
- Extract LSB data: `stegolsb wavsteg -i input.wav -o output.txt -b 100`
- Inspect with `binwalk` for appended files: `binwalk sound.wav`
- Search raw strings: `strings sound.wav`

## Tools

| Tool                                                  | Purpose                                                                                                                            |
|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Audacity                                              | Free, cross-platform audio editor that can display frequency-domain spectrograms. Excellent for spotting hidden messages visually. |
| Sonic Visualiser                                      | Dedicated waveform and frequency viewer for deeper spectral inspection and multichannel analysis.                                  |
| [stegolsb](https://github.com/ragibson/Steganography) | Command-line utility to encode or decode information from audio using Least Significant Bit manipulation. Ideal for .wav files.    |
| `strings`, `xxd`, `binwalk`                           | Raw data inspection and extraction of appended payloads.                                                                           |

## Spectrogram Analysis

In many cases, the flag will appear as text or a visible structure in the spectrogram.

Method 1: Audacity

1. Open the audio file (File -> Open).
2. Click on the track name -> Spectrogram View.
3. Adjust the scale via Spectrogram Settings for better contrast.
4. Look for visible text, patterns, or QR codes.

{{< img name="spectrogram" size="small" >}}

Method 2: Sonic Visualiser

1. Open file and select Layer -> Add Spectrogram.
2. Use color maps to enhance visual clarity.
3. Export frame regions if required.

## LSB Extraction

If nothing appears in the spectrogram, check for bit-level payloads.

`stegolsb` can extract raw binary embedded in waveforms.

Example usage:

```bash
stegolsb wavsteg -i sound.wav -o output.txt -b 100
```

Options:

- `-i`: input WAV file
- `-o`: output destination
- `-b`: number of bytes to extract

Once extracted, inspect the result with `file`, open it as text, or use `xxd` to interpret the data.

## Tips

- Always check file metadata using `exiftool sound.wav`, sometimes clues reside in metadata.
- Spectrogram hints might direct you to LSB extraction or another method.
