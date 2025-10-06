---
title: "TeamViewer Logs"
description: "Examine TeamViewer connection logs and session artifacts to identify remote access activity and file transfers."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["teamviewer", "rmm", "remote access", "logs", "connection history"]
authors: ["CTF.Support Team"]
summary: "Locate and parse TeamViewer session logs to uncover remote connections, file transfers, and potential data exfiltration events."
aliases: ["/forensics/logs-and-system-artifacts/teamviewer-logs/"]
slug: "teamviewer-logs"
toc: true
---

## Introduction

**TeamViewer** is a Remote Access & Management (RMM) tool that maintains multiple local log files.  
When compromised or abused, these logs record valuable artifacts such as connection timestamps, user IDs, internal IP addresses, and file transfer events.

## Common Log Locations

| Path                                                           | Description                                           |
|----------------------------------------------------------------|-------------------------------------------------------|
| `C:\Program Files\TeamViewer\Connections_incoming.txt`         | Text file tracking incoming sessions                  |
| `C:\Program Files\TeamViewer\Connections.txt`                  | Outgoing connections from the host                    |
| `C:\Program Files\TeamViewer\TeamViewer\[version]_Logfile.log` | Main runtime log (contains session and transfer data) |
| `%AppData%\Local\Temp\TeamViewer*`                             | Temporary session files or crash dumps                |

## Key Fields and Artifacts

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
