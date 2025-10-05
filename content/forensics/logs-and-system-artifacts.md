---
title: "Logs & System Artifacts"
description: "Analyze Windows and Linux logs, Event Logs, Thumbcache, and RDP Bitmap Cache to uncover hidden activity and artifacts in digital forensics challenges."
date: 2025-10-05
draft: false
weight: 30
categories: ["Forensics"]
tags: ["logs", "system artifacts", "event logs", "thumbcache", "rdp cache", "chainsaw", "linux logs"]
authors: ["CTF.Support Team"]
toc: true
summary: "Learn to analyze system logs and artifacts, from Windows Event Logs to Linux syslogs, thumbnail caches, and RDP session data, to trace activity and uncover hidden evidence."
aliases: ["/forensics/logs-and-system-artifacts/"]
slug: "logs-and-system-artifacts"
---

## Introduction

Logs and system artifacts provide a chronological record of system activity. In CTF challenges, they often contain flags, hidden commands, or traces of malicious activity.

Key goals:

- Understand and analyze logs
- Detect anomalies or hidden artifacts

## Tools

| Tool                                                                 | Purpose                                                                         |
|----------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Event Viewer (Windows)                                               | View Windows Event Logs (GUI)                                                   |
| [evtx](https://github.com/omerbenamram/evtx) (Cross platform)        | View and Parse Windows Event Logs                                               |
| [evtx_dump](https://github.com/williballenthin/python-evtx)          | Command-line Windows Event Logs Parser                                          |
| [chainsaw](https://github.com/WithSecureLabs/chainsaw)               | Identify Threats in Windows Event Logs                                          |
| [Eric Zimmerman's Tools](https://ericzimmerman.github.io/#!index.md) | Multiple Tools to Parse Different Logs and System Artifacts                     |
| [Thumbcache Viewer](https://thumbcacheviewer.github.io/)             | GUI tool that can parse and extract images directly from thumbcache_*.db files. |
| [BMC-Tools](https://github.com/ANSSI-FR/bmc-tools)                   | Extract and reconstruct bitmaps from RDP Bitmap Cache                           |
| `grep` / `awk` / `sed`                                               | Parse logs                                                                      |
| `strings`                                                            | Extract readable text from binaries                                             |

## Step-by-Step Guide

### Windows Event Logs

Windows Event Logs contain system, security, and application records. Common useful logs in CTFs:

- **Security**: login attempts, privilege changes
- **System**: device events, service changes
- **Application**: program execution, errors
- **PowerShell**: powershell commands and scriptblocks

Common Event IDs:

- 4624 – Successful logon
- 4625 – Failed logon
- 4688 – Process creation
- 4104 – PowerShell script block

View logs using GUI:

1. Open Event Viewer or `evtx`
2. Navigate to Windows Logs -> Security / System / Application
3. Filter by Event IDs or keywords

Command line extraction using `evtx_dump`:

```bash
evtx_dump Security.evtx > security.xml
```

Command line extraction using [EvtxECmd](https://github.com/EricZimmerman/evtx) (part of Eric Zimmerman's Tools):

```cmd
EvtxECmd.exe -f "C:\Temp\Application.evtx" --csv "c:\temp\out"
```

Analyze logs using `chainsaw`:

```bash
Hunt with Sigma and Chainsaw Rules:
    ./chainsaw hunt evtx_attack_samples/ -s sigma/ --mapping mappings/sigma-event-logs-all.yml -r rules/

Hunt with Sigma rules and output in JSON:
    ./chainsaw hunt evtx_attack_samples/ -s sigma/ --mapping mappings/sigma-event-logs-all.yml --json

Search for the case-insensitive word 'mimikatz':
    ./chainsaw search mimikatz -i evtx_attack_samples/

Search for Powershell Script Block Events (EventID 4014):
    ./chainsaw search -t 'Event.System.EventID: =4104' evtx_attack_samples/
```

### Thumbcache (Thumbnail Cache)

Windows automatically generates thumbnail previews for images, documents, and videos when browsing folders. These are stored in the thumbcache database files located under:

```text
C:\Users\<username>\AppData\Local\Microsoft\Windows\Explorer\
```

Common filenames:

```text
thumbcache_32.db
thumbcache_96.db
thumbcache_256.db
thumbcache_1024.db
thumbcache_idx.db
```

Each file corresponds to a specific thumbnail size.

Using these files, images that the user viewed can be recovered, even if the originals have been deleted.

View and recover images using Thumbcache Viewer:

1. Open a thumbcache file, for example `thumbcache_256.db`.
2. Browse through the extracted thumbnails.
3. Export any recovered images for further analysis (`.png` or `.jpg`).

### RDP Bitmap Cache

When a user connects to another system using **Remote Desktop Protocol (RDP)**, Windows caches small bitmap fragments of the remote desktop to speed up subsequent sessions. These fragments can reveal partial or complete images of the remote desktop.

Location:

```text
C:\Users\<username>\AppData\Local\Microsoft\Terminal Server Client\Cache\
```

Common file patterns:

```text
bcache*.bmc
cache0000.bin
cache0001.bin
```

Recover images using BMC-Tools:

```bash
./bmc-tools.py -s Cache -d Output
```

### Linux / Unix Logs

Key logs in Linux systems:

- `/var/log/syslog` – general system events
- `/var/log/auth.log` – authentication attempts
- `/var/log/messages` – kernel and system messages
- `~/.bash_history` – command history of users

Example: Find login activity

```bash
grep "Accepted password" /var/log/auth.log
```

Check for flags:

```bash
grep "CTF{" /var/log/*
```

## Tips

- Normalize timestamps before correlation across systems.
- Use keyword searches (`grep`, `chainsaw search`) to quickly locate flag patterns.
