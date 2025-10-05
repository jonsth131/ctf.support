---
title: "Logs & System Artifacts"
description: "Analyze Windows and Linux logs, Event Logs, Thumbcache, and RDP Bitmap Cache to uncover hidden activity and artifacts in digital forensics challenges."
date: 2025-10-05
draft: false
weight: 30
categories: ["Forensics"]
tags: ["logs", "system artifacts", "event logs", "thumbcache", "rdp cache", "chainsaw", "registry", "windows registry", "linux logs"]
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

| Tool                                                                  | Purpose                                                                         |
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Event Viewer (Windows)                                                | View Windows Event Logs (GUI)                                                   |
| [evtx](https://github.com/omerbenamram/evtx) (Cross platform)         | View and Parse Windows Event Logs                                               |
| [evtx_dump](https://github.com/williballenthin/python-evtx)           | Command-line Windows Event Logs Parser                                          |
| [chainsaw](https://github.com/WithSecureLabs/chainsaw)                | Identify Threats in Windows Event Logs                                          |
| [Eric Zimmerman's Tools](https://ericzimmerman.github.io/#!index.md)  | Multiple Tools to Parse Different Logs and System Artifacts                     |
| [Thumbcache Viewer](https://thumbcacheviewer.github.io/)              | GUI tool that can parse and extract images directly from thumbcache_*.db files. |
| [BMC-Tools](https://github.com/ANSSI-FR/bmc-tools)                    | Extract and reconstruct bitmaps from RDP Bitmap Cache                           |
| [Registry Explorer](https://ericzimmerman.github.io/#!index.md)       | GUI Registry browser for artifact timelines and bookmarkable keys               |
| [RECmd](https://ericzimmerman.github.io/#!index.md)                   | Command‑line companion to Registry Explorer with batch query support            |
| [RegRipper](https://github.com/keydet89/RegRipper3.0)                 | Parse individual hives using plugins (e.g. runkeys, software, network)          |
| [python-registry](https://github.com/williballenthin/python-registry) | Parse registry hives via Python (useful for automation or CTFs)                 |
| [MFTECmd](https://ericzimmerman.github.io/#!index.md)                 | Parses `$J` along with `$MFT` and `$LogFile` to CSV                             |
| [LECmd](https://ericzimmerman.github.io/#!index.md)                   | Parse `.lnk` files to CSV, extracts metadata, timestamps, and linked paths      |
| [PECmd](https://ericzimmerman.github.io/#!index.md)                   | Parse Prefetch files and extract execution metadata                             |
| `regedit` / `reg query`                                               | Native Windows tools for viewing live registries                                |
| `grep` / `awk` / `sed`                                                | Parse logs                                                                      |
| `strings`                                                             | Extract readable text from binaries                                             |

## Step-by-Step Guide

### Windows Event Logs

Windows Event Logs contain system, security, and application records. Common useful logs in CTFs:

- **Security**: login attempts, privilege changes
- **System**: device events, service changes
- **Application**: program execution, errors
- **PowerShell**: powershell commands and scriptblocks

Windows Event Logs are commonly found in `C:\Windows\System32\winevt\Logs\`.

Common Event IDs:

| Event ID | Description             | Use Case             |
|----------|-------------------------|----------------------|
| 4104     | PowerShell script block | Code execution       |
| 4624     | Successful logon        | Initial access       |
| 4625     | Failed logon            | Brute force          |
| 4688     | Process creation        | Execution monitoring |
| 4720     | Account creation        | Persistence          |
| 7045     | Service installation    | Rootkits/malware     |

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

### Windows Registry Analysis

The **Windows Registry** stores configuration and activity data, including user behavior, persistence mechanisms, and system changes.
In forensics challenges, analyzing registry hives can reveal malware persistence, executed programs, connected devices, and user accounts.

#### Common Registry Hives

| Hive         | Location                                                       | Contains                                                    |
|--------------|----------------------------------------------------------------|-------------------------------------------------------------|
| SYSTEM       | `C:\Windows\System32\config\SYSTEM`                            | Hardware, services, network configuration, and control sets |
| SOFTWARE     | `C:\Windows\System32\config\SOFTWARE`                          | Installed applications, versioning, execution history       |
| SAM          | `C:\Windows\System32\config\SAM`                               | Local user accounts and password hashes                     |
| SECURITY     | `C:\Windows\System32\config\SECURITY`                          | Security policy and audit configuration                     |
| NTUSER.DAT   | `C:\Users\<user>\NTUSER.DAT`                                   | Per‑user settings, recent documents, Run keys               |
| USRCLASS.DAT | `C:\Users\<user>\AppData\Local\Microsoft\Windows\USRCLASS.DAT` | User shellbags (folders visited, UI states)                 |

#### Persistence and Execution Artefacts

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

### $J (USN Journal) Analysis

The **USN Journal** (`$Extend\$J`) is part of the NTFS file system and records *every change* made to files and directories, creations, deletions, renames, and data writes.  
Examining `$J` can reconstruct activity timelines even after logs or prefetch files have been cleared.

On NTFS volumes, the journal file resides at:

```text
C:\$Extend\$J
```

#### Useful Information Stored

Each journal record includes:

| Field                    | Description                                         |
|--------------------------|-----------------------------------------------------|
| Timestamp                | When the event or update occurred                   |
| File Reference Number    | Internal file ID used by NTFS                       |
| Reason Flags             | What action occurred (create, delete, rename, etc.) |
| Parent File Ref          | Directory the file belonged to                      |
| Security ID / Attributes | Permission or metadata modifications                |

Common **Reason Flags** include:

| Flag       | Meaning                |
|------------|------------------------|
| 0x00000100 | File Create            |
| 0x00000200 | File Delete            |
| 0x00002000 | Data Overwrite         |
| 0x00004000 | Data Extend            |
| 0x00008000 | Data Truncation        |
| 0x00010000 | File Rename (New Name) |

#### Example Usage (Eric Zimmerman Tools)

```bash
# Parse the $J journal into CSV for analysis
MFTECmd.exe -f "C:\$Extend\$J" --csv "out/"

# Combine Multiple Sources (MFT + J)
MFTECmd.exe -f "C:\$MFT" --csv "out/"
MFTECmd.exe -f "C:\$Extend\$J" --csv "out/"
```

### LNK (Shortcut) File Analysis

Windows automatically creates **shortcut files (.LNK)** whenever a user opens a document, application, or folder.  
These files record **file locations, access times, drive information, and original paths**, even if the originals were deleted or on removable media.

In digital forensics and CTF challenges, analyzing `.LNK` artifacts can reveal:

- File or flag names previously opened by a user  
- The original storage device or network share a file resided on  
- When specific files were accessed (creation / modification / access times)  
- Links between user actions and `$J` journal or registry entries

#### Typical Locations

| Location                                                    | Purpose                             |
|-------------------------------------------------------------|-------------------------------------|
| `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\` | User shortcut cache (recent files)  |
| `C:\Users\<user>\Recent\AutomaticDestinations\`             | Jump Lists (binary .lnk containers) |

#### Using LECmd

```bash
# Parse all recent shortcut files
LECmd.exe -d "C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent" --csv "out/"
```

Resulting CSV fields include:

| Column                                                  | Meaning                                              |
|---------------------------------------------------------|------------------------------------------------------|
| SourceFile                                              | Path of the .LNK parsed                              |
| TargetFile                                              | File or folder linked to                             |
| MachineID                                               | Host where the .LNK was created                      |
| VolumeSerialNumber                                      | Serial of the drive that contained the original file |
| OpenedTime / AccessedTime / ModifiedTime / CreationTime | Timeline metadata useful for correlation             |

### Prefetch Analysis

Windows **Prefetch** files (`.pf`) record information about executed applications for performance optimization.  
They also provide **evidence of program execution**, even after binaries have been deleted.

#### Location

```text
C:\Windows\Prefetch\
```

Example files: `CMD.EXE‑A6294E76.pf`, `MIMIKATZ.EXE‑B29D8C74.pf`

#### Key Fields

| Field         | Description                           |
|---------------|---------------------------------------|
| Run Count     | Number of times the program executed  |
| Last Run Time | Most recent execution timestamp (UTC) |
| File List     | Files accessed by this executable     |

#### Example

```bash
PECmd.exe -d C:\Windows\Prefetch --csv out/
```

### TeamViewer Log Analysis

**TeamViewer** is a Remote Access & Management (RMM) tool that maintains multiple local log files.  
When compromised or abused, these logs record valuable artifacts such as connection timestamps, user IDs, internal IP addresses, and file transfer events.

#### Common Log Locations

| Path                                                           | Description                                           |
|----------------------------------------------------------------|-------------------------------------------------------|
| `C:\Program Files\TeamViewer\Connections_incoming.txt`         | Text file tracking incoming sessions                  |
| `C:\Program Files\TeamViewer\Connections.txt`                  | Outgoing connections from the host                    |
| `C:\Program Files\TeamViewer\TeamViewer\[version]_Logfile.log` | Main runtime log (contains session and transfer data) |
| `%AppData%\Local\Temp\TeamViewer*`                             | Temporary session files or crash dumps                |

#### Key Fields and Artifacts

**Connections_incoming.txt** (CSV‑like plain text):

| Column                   | Meaning                                                          |
|--------------------------|------------------------------------------------------------------|
| Session ID               | Connection identifier                                            |
| Partner Name / User Name | The remote user initiating the session                           |
| Start Time / End Time    | Session bounds (local timezone; UTC offset from System registry) |
| Type                     | `RemoteControl`, `FileTransfer`, `Meeting`, etc.                 |
| Comment / UUID           | Optional metadata used internally by TeamViewer                  |

Example entry:

```text
514162531   James Moriarty    20‑08‑2025  09:58:25   20‑08‑2025  10:14:27   Cogwork_Admin   RemoteControl   {7ca6431e‑30f6‑45e3‑9ac6‑0ef1e0cecb6a}
```

**TeamViewerLogfile.log**:

Key terms to search with `grep`:

- `We left session`: Session end indicator and UUID reference  
- `UDPv4:`: Internal IP and port (e.g., `UDPv4: punch received a=192.168.69.213:55408`)  
- `Write file` / `Send file`: File transfer direction and path  
- `RoutingSessions:`: Session closure or network disconnect  

Example snippets:

```text
2025/08/20 10:12:07.902 1052 5128 G1 Send file C:\Windows\Temp\flyover\COG-HR-EMPLOYEES.pdf
2025/08/20 10:14:27.585 2804 3076 S0 Net: RoutingSessions: We left session, SLID=2, SessionUUID={d92f133b‑846b‑45aa‑b512‑e24ccd7a84b4}
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
