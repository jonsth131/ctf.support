---
title: "Signals"
description: "Identify and decode signals including SSTV, Morse code, PSK, and DTMF tones."
categories: ["Misc"]
tags: ["signals", "sstv", "morse", "dtmf", "radio", "ctf"]
authors: ["CTF.Support Team"]
summary: "Identify unknown signals and decode them using popular radio and digital communication tools."
aliases: ["/misc/signals/"]
slug: "signals"
toc: true
---

## Introduction

Signal challenges involve encoded radio or sound transmissions, requiring the solver to identify modulation types and decode data.

## Signal Identification

Use [SigidWiki](https://www.sigidwiki.com/wiki/Signal_Identification_Guide) to identify unknown signals before decoding.

## Tools

| Tool                                                                                       | Purpose                                                                |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| [fldigi](https://www.w1hkj.org/)                                                           | Software suite for amateur radio decoding (PSK, RTTY, MFSK, and more). |
| [DigiPan](https://bpsk31.com/apps/)                                                        | Free PSK31 and PSK63 signal decoder.                                   |
| [QSSTV](https://github.com/ON4QZ/QSSTV) / [RX-SSTV](https://www.qsl.net/on6mu/rxsstv.htm)  | Linux and Windows tools for decoding SSTV images.                      |
| [dtmf-decoder](https://github.com/ribt/dtmf-decoder)                                       | Python CLI application for DTMF tone decoding.                         |
| [Detect DTMF Tones](http://dialabc.com/sound/detect/index.html)                            | Online DTMF audio decoder.                                             |
| [Morse Decoder](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) | Web-based Morse audio decoder.                                         |
| [minimodem](https://www.whence.com/minimodem/)                                             | CLI tool to demodulate FSK modem signals (300–9600 baud) from audio.   |

## Modem Signals

Sound files that resemble old dial‑up or data‑transfer tones may actually contain **modem transmissions**, often encoded at 300 or 1200 baud.  
These can be decoded directly with [`minimodem`](https://www.whence.com/minimodem/).

```bash
minimodem --rx 1200 -f transmissions.wav
```

Sample output:

```text
### CARRIER 1200 @ 1200.0 Hz ###
Greetings, Professor Falken.

Would you like to play a game?

flag{f28d133e7174c412c1e39b4a84158fa3}

Thanks for playing the Huntress CTF!

        @
       @@
       @@@@
  @@@@  @@@@@@  @@ @@@@@@@@
   @@@@@@ @@@@@@@   @  @  @@@@
     @@@@@@@@@@@@@@ @@@ @@   @@@
 @@@@@@# @@@@@@@@@@@ @@@@@@@@  @@@
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@ @@@
  @@         @@@@@@@@@@@@@@@     @@
  @@@@@@@@@@@@/@@@@@@@@@     @(~ @@
  @@    @@@@@@@@@@@ @@@@ @@@@  @@@
  @@@ @@@@.@@@@@@@@ @@@@@ @@@@  @@
   @@@  @@@*%@@@@ @@@ @@@@ @@@@@
     @@@    @   @@@@@@@@@ @@@@@
       @@@@.     @@@@@@@@ @@@@
           @@@@@@@@@ @@@@@ @@
                        @@
                         @ -dk

### NOCARRIER ndata=663 confidence=4.773 ampl=1.001 bps=1200.00 (rate perfect) ###
```
