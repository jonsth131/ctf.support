---
title: "Discord"
description: "Investigate Discord artifacts such as message links, snowflake IDs, and API data used in CTF challenges."
date: 2025-10-06
draft: false
categories: ["Misc"]
tags: ["discord", "snowflake", "api", "metadata", "ctf"]
authors: ["CTF.Support Team"]
summary: "Analyze Discord message links, extract metadata from snowflake IDs, and understand how to gather open server information for CTF challenges."
aliases: ["/misc/discord/"]
slug: "discord"
toc: true
---

## Introduction

Discord is a popular communication platform used by many CTFs for announcements, challenges, or hidden flags.
Links and files from Discord can reveal useful data even if the invite or message is inactive.

This page covers how to interpret **Discord links**, and work with **snowflake IDs**.

---

## Discord Links and Structure

Discord resources (guilds, channels, messages) are identified by numeric IDs called **Snowflakes**.
These IDs encode both a timestamp and unique object ID bits.

### Common URL Patterns

| Purpose              | Example URL Structure                                               |
|----------------------|---------------------------------------------------------------------|
| **Guild / Server**   | `https://discord.com/channels/<guild_id>`                           |
| **Channel**          | `https://discord.com/channels/<guild_id>/<channel_id>`              |
| **Specific Message** | `https://discord.com/channels/<guild_id>/<channel_id>/<message_id>` |
| **Invite Link**      | `https://discord.gg/<invite_code>`                                  |

An invalid or expired link may still leak IDs that can be decoded to determine when the guild/channel/message was created.

---

## Understanding Discord Snowflakes

**Snowflakes** are unique id's containing a timestamp.

Useful tools:

- [Snowstamp](https://snowsta.mp/)
