---
title: "Logs & System Artifacts"
description: "Analyze system and application artifacts like Event Logs, Registry, Prefetch, and RDP Cache for forensic insights in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["logs", "artifacts", "windows", "linux", "event logs", "registry", "prefetch"]
authors: ["CTF.Support Team"]
summary: "Understand how different system artifacts store operational and forensic data. Break down Windows and Linux evidence sources across sub‑pages for targeted analysis."
aliases: ["/forensics/logs-and-system-artifacts/"]
slug: "logs-and-system-artifacts"
toc: false
---

## Introduction

System logs and digital artifacts record traces of almost every action on a device.  
In CTFs, these files can hide flags, reveal the attacker’s activity, or expose persistence mechanisms.

Common targets include:

- **Windows Event Logs**: execution, logons, privilege escalation  
- **Registry hives**: persistence and configuration data  
- **Prefetch / LNK / Thumbcache**: file and program execution evidence  
- **RDP & TeamViewer logs**: remote access traces  
- **Linux/Unix logs**: authentication, system events, shell history  

{{< toc-tree >}}
