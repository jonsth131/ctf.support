---
title: "Linux Log Analysis"
description: "Inspect Linux and Unix log files to detect logins, system events, and user activity relevant to forensic investigations."
date: 2025-10-06
draft: false
categories: ["Forensics"]
tags: ["linux", "logs", "syslog", "auth.log", "bash_history"]
authors: ["CTF.Support Team"]
summary: "Search and correlate Linux system logs such as syslog, auth.log, and bash history to identify authentication events or hidden activity."
aliases: ["/forensics/logs-and-system-artifacts/linux-logs/"]
slug: "linux-logs"
toc: true
---

## Key logs in Linux systems

- `/var/log/syslog` – general system events
- `/var/log/auth.log` – authentication attempts
- `/var/log/messages` – kernel and system messages
- `~/.bash_history` – command history of users

## Example: Find login activity

```bash
grep "Accepted password" /var/log/auth.log
```

## Check for flags

```bash
grep "CTF{" /var/log/*
```
