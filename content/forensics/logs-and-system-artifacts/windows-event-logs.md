---
title: "Windows Event Logs"
description: "Parse and analyze Windows Event Logs to detect execution, logons, and suspicious activity in forensic investigations."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["windows", "event logs", "security", "chainsaw", "evtx"]
authors: ["CTF.Support Team"]
summary: "Extract and analyze Windows event logs (.evtx) with tools like Chainsaw, EvtxECmd, and Eric Zimmerman's utilities to uncover attacker actions and flag traces."
aliases: ["/forensics/logs-and-system-artifacts/windows-event-logs/"]
slug: "windows-event-logs"
toc: true
---

## Introduction

Windows Event Logs contain system, security, and application records. Common useful logs in CTFs:

- **Security**: login attempts, privilege changes
- **System**: device events, service changes
- **Application**: program execution, errors
- **PowerShell**: powershell commands and scriptblocks

## Tools

| Tool                                                                 | Purpose                                                     |
|----------------------------------------------------------------------|-------------------------------------------------------------|
| Event Viewer (Windows)                                               | View Windows Event Logs (GUI)                               |
| [evtx](https://github.com/omerbenamram/evtx) (Cross platform)        | View and Parse Windows Event Logs                           |
| [evtx_dump](https://github.com/williballenthin/python-evtx)          | Command-line Windows Event Logs Parser                      |
| [chainsaw](https://github.com/WithSecureLabs/chainsaw)               | Identify Threats in Windows Event Logs                      |
| [Eric Zimmerman's Tools](https://ericzimmerman.github.io/#!index.md) | Multiple Tools to Parse Different Logs and System Artifacts |

## Common Log Locations

```text
C:\Windows\System32\winevt\Logs\
```

## Common Event IDs

| Event ID | Description             | Use Case             |
|----------|-------------------------|----------------------|
| 4104     | PowerShell script block | Code execution       |
| 4624     | Successful logon        | Initial access       |
| 4625     | Failed logon            | Brute force          |
| 4688     | Process creation        | Execution monitoring |
| 4720     | Account creation        | Persistence          |
| 7045     | Service installation    | Rootkits/malware     |

## Examples

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
