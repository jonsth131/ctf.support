---
title: "Windows Registry Analysis"
description: "Explore Windows Registry hives to find persistence, user behavior, and configuration artifacts."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["registry", "windows", "regripper", "recmd", "persistence"]
authors: ["CTF.Support Team"]
summary: "Analyze registry hives using tools like Registry Explorer, RECmd, and RegRipper to identify persistence keys, recent activity, and system configuration."
aliases: ["/forensics/logs-and-system-artifacts/registry-analysis/"]
slug: "registry-analysis"
toc: true
---

## Introduction

The **Windows Registry** stores configuration and activity data, including user behavior, persistence mechanisms, and system changes.
In forensics challenges, analyzing registry hives can reveal malware persistence, executed programs, connected devices, and user accounts.

## Tools

| Tool                                                                  | Purpose                                                                |
|-----------------------------------------------------------------------|------------------------------------------------------------------------|
| [Registry Explorer](https://ericzimmerman.github.io/#!index.md)       | GUI Registry browser for artifact timelines and bookmarkable keys      |
| [RECmd](https://ericzimmerman.github.io/#!index.md)                   | Command‑line companion to Registry Explorer with batch query support   |
| [RegRipper](https://github.com/keydet89/RegRipper3.0)                 | Parse individual hives using plugins (e.g. runkeys, software, network) |
| [python-registry](https://github.com/williballenthin/python-registry) | Parse registry hives via Python (useful for automation or CTFs)        |

## Common Registry Hives

| Hive         | Location                                                       | Contains                                                    |
|--------------|----------------------------------------------------------------|-------------------------------------------------------------|
| SYSTEM       | `C:\Windows\System32\config\SYSTEM`                            | Hardware, services, network configuration, and control sets |
| SOFTWARE     | `C:\Windows\System32\config\SOFTWARE`                          | Installed applications, versioning, execution history       |
| SAM          | `C:\Windows\System32\config\SAM`                               | Local user accounts and password hashes                     |
| SECURITY     | `C:\Windows\System32\config\SECURITY`                          | Security policy and audit configuration                     |
| NTUSER.DAT   | `C:\Users\<user>\NTUSER.DAT`                                   | Per‑user settings, recent documents, Run keys               |
| USRCLASS.DAT | `C:\Users\<user>\AppData\Local\Microsoft\Windows\USRCLASS.DAT` | User shellbags (folders visited, UI states)                 |

## Persistence and Execution Artefacts

Attackers often modify registry keys to achieve **persistence** or **command execution** at startup.

Common locations to inspect:

| Registry Path                                                         | Purpose                    | Notes                             |
|-----------------------------------------------------------------------|----------------------------|-----------------------------------|
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`                  | User-level autoruns        | Executes apps on logon            |
| `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`                  | System-wide autoruns       | Persists services                 |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit` | Logon initialization chain | Check for appended malware        |
| `HKLM\SYSTEM\CurrentControlSet\Services\`                             | Service persistence        | Lists drivers & rootkits          |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa`                           | Credential providers       | Impersonation or injection points |
| `HKLM\SYSTEM\CurrentControlSet\Services\PortProxy\v4tov4\tcp`         | Port proxy rules           | Used for internal proxy pivoting  |
