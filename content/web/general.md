---
title: "General"
description: "Essential tools and common reconnaissance steps for discovering web vulnerabilities in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["recon", "web enumeration", "burpsuite", "directories", "git", "ctf"]
authors: ["CTF.Support Team"]
summary: "Quick reference for web reconnaissance, discover endpoints, exposed files, and hidden repositories using proxies and scanners."
aliases: ["/web/general/"]
slug: "general"
toc: true
---

## Introduction

Before exploiting web vulnerabilities, information gathering is key.
Using proxies, directory enumeration, and version control leaks, you can identify potential entry points or secrets in a challenge’s web application.

## Quick Reference

| Task                        | Tool / Command                           |
|-----------------------------|------------------------------------------|
| Intercept & modify requests | **Burp Suite**, **OWASP ZAP**, **Caido** |
| Check sensitive files       | `/robots.txt`, `/.git/`, `/.DS_Store`    |
| Dump leaked Git repos       | `git-dumper`, `GitTools`                 |

## Tools

| Tool                                                  | Purpose                                        |
|-------------------------------------------------------|------------------------------------------------|
| [Burp Suite](https://portswigger.net/burp)            | Comprehensive web proxy and exploitation suite |
| [OWASP ZAP](https://www.zaproxy.org/)                 | Open‑source proxy for scanning and fuzzing     |
| [Caido](https://www.caido.io/)                        | Modern proxy alternative with clean UI         |
| [GitTools](https://github.com/internetwache/GitTools) | Download and recover `.git` repository leaks   |
| [git-dumper](https://github.com/arthaud/git-dumper)   | Clone `.git` directories exposed via web       |

## Tips

- Always begin by mapping the site using a proxy.
- Test direct file access (`robots.txt`, `.env`, `.git/HEAD`, `.htaccess`).
- Reconstruct repositories found online via `.git`.
- Check for API endpoints or commented URLs in HTML or JS source.
