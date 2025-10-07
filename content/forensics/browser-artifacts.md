---
title: "Browser Artifacts"
description: "Recover and analyze browser artifacts (history, cookies, passwords) from browser profiles."
date: 2025-10-05
draft: false
weight: 32
categories: ["Forensics"]
tags: ["browser forensics", "firefox", "password recovery", "profile", "dumpzilla", "firefox_decrypt"]
authors: ["CTF.Support Team"]
toc: true
summary: "Guide to extracting browser profile data, passwords, history, bookmarks and more."
aliases: ["/forensics/browser-artifacts/"]
slug: "browser-artifacts"
---

## Introduction

Browser artifacts often contain rich evidence: visited URLs, cookies, saved credentials, downloads and local storage.

## Quick Reference

- List Firefox profiles (Linux): `ls ~/.mozilla/firefox/*.default*`
- Dump logins: `python firefox_decrypt.py /path/to/profile`
- Extract profile artifacts with Dumpzilla: `dumpzilla /path/to/profile -o output_dir`
- Legacy Firepwd (older Firefox): `firepwd /path/to/profile` *(legacy tool, use firefox_decrypt for modern profiles)*

## Tools

| Tool                                                        | Purpose                                                                  |
|-------------------------------------------------------------|--------------------------------------------------------------------------|
| [firefox_decrypt](https://github.com/unode/firefox_decrypt) | Decrypts saved logins (logins.json) using `key4.db` / `key3.db`          |
| [dumpzilla](https://github.com/Busindre/dumpzilla)          | Extracts bookmarks, history, cookies, downloads and forms from a profile |
| [Firepwd](https://github.com/lclevy/firepwd)                | Legacy Firefox password recovery (old Fx versions)                       |
| `sqlite3`                                                   | Inspect SQLite DBs (places.sqlite, cookies.sqlite)                       |
| [DB Browser for SQLite](https://sqlitebrowser.org/)         | Inspect SQLite DBs (GUI)                                                 |

## Firefox

### Profile Locations

Typical profile paths:

- **Linux:** `~/.mozilla/firefox/<profile>.default/`
- **Windows:** `C:\Users\<user>\AppData\Roaming\Mozilla\Firefox\Profiles\<profile>\`
- **macOS:** `~/Library/Application Support/Firefox/Profiles/<profile>/`

Key files to look for:

- `logins.json` - encrypted saved credentials
- `key4.db` / `key3.db` - encryption keys for saved credentials
- `places.sqlite` - history & bookmarks
- `cookies.sqlite` - cookies
- `formhistory.sqlite` - saved form entries
- `sessionstore.jsonlz4` - session restore data (open tabs/windows)

### Extracting Saved Passwords

**Using `firefox_decrypt` (recommended for modern Firefox):**

1. Copy the profile directory locally (work on a copy).
2. Run:

```bash
python3 firefox_decrypt.py /path/to/profile
```

What it does: It uses `key4.db`/`key3.db` to decrypt `logins.json` and prints saved logins (URLs, usernames, passwords).

**Notes**:

- If the profile has a Master Password set, automated decryption will fail unless you supply the master password.
- `firepwd` is an older tool and may only work for legacy Firefox versions, prefer `firefox_decrypt`.

### Extracting Other Artifacts

Dumpzilla extracts a broad set of artifacts:

```bash
dumpzilla /path/to/profile -o ./dump_output
```

It can export bookmarks, history, cookies, downloads, and HTML reports.

Manual inspection with sqlite3:

```bash
sqlite3 places.sqlite "SELECT url, title, visit_count FROM moz_places ORDER BY last_visit_date DESC LIMIT 50;"
sqlite3 cookies.sqlite "SELECT host, name, value FROM moz_cookies;"
```

The SQLite databases can also be viewed with DB Browser for SQLite.
