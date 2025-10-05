---
title: "OSINT"
description: "Open Source Intelligence (OSINT) for CTFs, use public data sources and search techniques to gather and analyze information from the web, images, and social platforms."
date: 2025-10-05
draft: false
categories: ["OSINT"]
tags: ["osint", "reconnaissance", "information gathering", "geolocation", "reverse image search", "password leaks", "ctf"]
authors: ["CTF.Support Team"]
summary: "Learn how to perform open-source intelligence gathering (OSINT) for CTF challenges, including geolocation, image search, password leaks, and web archiving."
aliases: ["/osint/"]
slug: "osint"
toc: true
---

## Introduction

**Open Source Intelligence (OSINT)** is about gathering data from publicly available sources, websites, images, maps, and social media, to uncover hidden information.

In CTF challenges, OSINT tasks may include:

- Identifying a location from a photo
- Tracing a username across platforms
- Searching for password leaks
- Recovering deleted web content through archives

This section summarizes the most practical OSINT resources and techniques used in CTF and cybersecurity investigations.

## General Tools

| Tool                                                  | Purpose                           |
|-------------------------------------------------------|-----------------------------------|
| [OSINT Framework](https://osintframework.com/)        | Find free OSINT resources.        |
| [IntelTechniques](https://inteltechniques.com/tools/) | Public collection of OSINT tools. |

## Geolocation

Goal: Identify a location based on visual or textual clues.

Common data points to analyze:

- Architecture, terrain, and language on signs.
- Vehicle types and license plates.
- Shadow direction and sun position.
- Road markings and natural landmarks (mountains, coastlines).

Tools:

| Tool                                                                         | Purpose                                                                                  |
|------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| [GeoTips](https://geotips.net/)                                              | Comprehensive training guide for geolocation challenges (especially Google Street View). |
| [Google Earth](https://earth.google.com/) / [Maps](https://maps.google.com/) | Examine landscapes, buildings, and coordinates.                                          |
| [SunCalc](https://www.suncalc.org/)                                          | Estimate sun position and shadow direction for time-of-day deductions.                   |
| [Mapillary](https://www.mapillary.com/)                                      | Street-level imagery alternative to Google Street View.                                  |
| [What3Words](https://what3words.com/)                                        | Coordinate locator dividing the world into unique 3‑word addresses.                      |

Approach:

1. **Identify language or signage**: note writing systems, road markings, and symbols.  
2. **Analyze environmental features**: terrain, flora, weather, and vehicles.  
3. **Verify coordinates**: use map matching on Google Earth or Street View.  
4. **Confirm accuracy**: align shadows using SunCalc or landmark geometry.

## Images

Goal: Investigate and trace images to their origin or related content.

Reverse image searches are often used to find where a photo first appeared, identify people, or locate objects and symbols.

Tools:

| Tool                                        | Purpose                                                                         |
|---------------------------------------------|---------------------------------------------------------------------------------|
| [Google Images](https://images.google.com/) | Standard reverse image search from URLs or uploads.                             |
| [Yandex Images](https://yandex.com/images/) | Often more effective for faces, buildings, or non‑Western content.              |
| [TinEye](https://tineye.com/)               | Reverse image search focused on image matching rather than content.             |
| `exiftool`                                  | Extract embedded metadata such as GPS coordinates, timestamps, and camera info. |

Before uploading, always check the image locally with `exiftool` or `strings image.jpg`, flags and coordinates are often left in metadata.

For faces or unique landmarks, try both Google and Yandex, each indexes different datasets.

## Passwords

Goal: Identify re‑used or compromised passwords, usernames, and email addresses in public leaks.

These tools provide access to leaked credentials (for ethical research use only).

| Tool                                          | Purpose                                                         |
|-----------------------------------------------|-----------------------------------------------------------------|
| [dehashed](https://dehashed.com/)             | Search database leaks by email, username, or password hash.     |
| [LeakCheck](https://leakcheck.io/)            | Search and API-based credential checking service.               |
| [SnusBase](https://snusbase.com/)             | Large-scale breach search with hash lookups.                    |
| [HaveIBeenPwned](https://haveibeenpwned.com/) | Free public API to check if an email appears in known breaches. |

Tips:

- Search usernames or emails found in metadata, social media, or archives.
- Use partial hashes or common passwords in combination with CTF clues.

## Web & Social Media

Goal: Uncover deleted or hidden information from websites and public data sources.

Common tasks:

- Recover deleted web pages or past site states.
- Search usernames or domains across platforms.

Tools:

| Tool                                                     | Purpose                                                               |
|----------------------------------------------------------|-----------------------------------------------------------------------|
| [Wayback Machine](https://web.archive.org/)              | View historical versions of websites.                                 |
| [Hunter.io](https://hunter.io/)                          | Discover email patterns and addresses linked to domains.              |
| [Social Searcher](https://www.social-searcher.com/)      | Search multiple social platforms for usernames and keywords.          |
| [Shodan](https://www.shodan.io/)                         | Search for exposed machines on the internet.                          |
| [Sherlock](https://github.com/sherlock-project/sherlock) | Command-line username search across hundreds of websites.             |
| [Maigret](https://github.com/soxoj/maigret)              | OSINT tool for aggregating social accounts tied to a single identity. |
