---
title: "Network Forensics"
description: "Analyze PCAP captures to extract hidden data, reconstruct network sessions, and detect covert channels in CTF challenges using Wireshark, tshark, and Scapy."
date: 2025-10-05
draft: false
weight: 50
categories: ["Forensics"]
tags: ["network forensics", "pcap", "wireshark", "tshark", "scapy", "dns tunneling", "covert channels", "usb hid"]
authors: ["CTF.Support Team"]
toc: true
summary: "Learn how to analyze network traffic, extract files, detect covert channels, and decode unusual protocols in CTF and digital forensics challenges."
aliases: ["/forensics/network-forensics/"]
slug: "network-forensics"
---

## Introduction

Network forensics involves analyzing network traffic captures (PCAPs) to uncover hidden data, communication patterns, or malicious activity.

In CTFs, network forensics challenges often contain:

- Flags hidden in protocols (HTTP, DNS, FTP, etc.)
- File transfers over the network
- Covert or encoded communication channels

Goal: Extract flags, reconstruct sessions, and detect anomalies.

---

## Quick Reference

- Inspect PCAP: `tshark -r capture.pcap`
- Follow HTTP streams: Wireshark -> “Follow -> TCP Stream”
- Extract DNS queries: `tshark -r capture.pcap -T fields -e dns.qry.name`
- Filter by protocol: `tshark -r capture.pcap -Y "ftp or http"`

---

## Tools

| Tool                                                              | Purpose                                            |
|-------------------------------------------------------------------|----------------------------------------------------|
| [Wireshark](https://www.wireshark.org/)                           | Graphical PCAP analysis, protocol dissection       |
| `tshark`                                                          | Command-line Wireshark for scripting               |
| `tcpdump`                                                         | Capture or filter traffic                          |
| `pcapfix` / [Online Utility](https://f00l.de/hacking/pcapfix.php) | Repair or reconstruct corrupted PCAP capture files |
| [Crackle](https://github.com/mikeryan/crackle)                    | Crack Bluetooth Low Energy (BLE) encrypted traffic |
| [NTLMRawUnHide](https://github.com/cddmp/ntlmrawunhide)           | Extract NTLM/NetNTLMv2 hashes from pcaps           |
| Scapy / Python                                                    | Parse, manipulate, or extract custom protocol data |

---

## Step-by-Step Guide

### Inspect the PCAP

Start by opening the PCAP in Wireshark:

- Look at protocols used
- Identify unusual ports or traffic
- Apply display filters, e.g.,

```text
http.request
dns
tcp.port == 1337
```

Command-line alternative:

```bash
tshark -r capture.pcap
```

---

### Reconstruct Sessions

Reconstruct streams to extract files or messages:

HTTP files: Follow -> TCP Stream -> Save as file

FTP / SMTP transfers: follow streams to extract payloads

---

### Search for Hidden Flags

Flags may be embedded in:

- HTTP headers or GET/POST requests
- DNS queries (DNS tunneling)
- Base64 or hex-encoded payloads
- Unusual TCP/UDP payloads

---

### Analyze Covert Channels

Some CTFs hide flags in protocol misuse:

- DNS Tunneling: data encoded in subdomain labels
- ICMP / Ping: payloads hidden in ping packets
- Timing channels: flags encoded in packet timing
- Custom protocols: analyze using Scapy to parse payloads

Example: Extract DNS query names

```bash
tshark -r capture.pcap -T fields -e dns.qry.name
```

---

### Authentication Traffic & Credential Recovery

Network captures from corporate environments or Windows‑based systems often include **NTLM, Kerberos, or HTTP authentication** exchanges.
These can be analyzed to recover usernames and crack captured password hashes.

#### NTLM Authentication in Network Traffic

NTLM (Network LAN Manager) is a challenge‑response authentication protocol.
When captured in PCAPs (especially over SMB, WebDAV, or HTTP Negotiate requests), it often appears in Base64 strings within an `Authorization` or `WWW‑Authenticate` header.

##### Example – WebDAV NTLM Transaction

```text
PROPFIND /share HTTP/1.1
User-Agent: Microsoft-WebDAV-MiniRedir/10.0.19045
Host: dcc01
Authorization: Negotiate TlRMTVNTUAADAAAAGAAYAHQAAAA2ATYBjAAAAA...
```

You’ll typically see three stages:

1. **Negotiate**: client requests authentication types.
2. **Challenge**: server issues nonce and challenge.
3. **Authenticate**: client responds with hashed credentials.

#### Extracting Hashes from PCAPs

To recover these hash strings for offline cracking:

- Use [`NTLMRawUnHide`](https://github.com/cddmp/ntlmrawunhide) to parse the capture and extract NetNTLMv1/v2 hashes:

```bash
python3 NTLMRawUnHide.py -i capture.pcap -o ntlmhashes.txt
```

---

#### Cracking NTLM Hashes

Once collected, crack with **John the Ripper** or **Hashcat**.

```bash
# Using John the Ripper
john ntlmhashes.txt --wordlist=rockyou.txt --format=netntlmv2

# Using Hashcat
hashcat -m 5600 -a 0 ntlmhashes.txt rockyou.txt
```

Sample John output indicating successful cracks:

```text
Loaded 13 password hashes ( netntlmv2 )
[RHODE_ISLAND_Z] ( CTF )
13g 0:00:00:00 DONE
```

---

#### Common Encapsulation Points

| Protocol                             | Typical Port | Authentication Header / Mechanism                   |
|--------------------------------------|--------------|-----------------------------------------------------|
| **SMB**                              | 445 / 139    | NTLM handshake (negotiate, challenge, authenticate) |
| **HTTP / WebDAV**                    | 80 / 443     | `Authorization: Negotiate TlRMTV...`                |
| **POP3 / IMAP / SMTP (MS Exchange)** | Various      | NTLM Auth responses                                 |
| **LDAP / Kerberos**                  | 389 / 88     | May contain tickets or hash patterns                |

---

### Exporting Files

Extract HTTP files using Wireshark: `File -> Export Objects -> HTTP`

---

### Fix Broken Capture Files

Sometimes challenge PCAPs are intentionally truncated or corrupted.
Use **pcapfix** to repair header and packet data:

```bash
pcapfix broken.pcap -o repaired.pcap
```

An online version is available: [https://f00l.de/hacking/pcapfix.php](https://f00l.de/hacking/pcapfix.php)

---

### Analyze Bluetooth Captures

Bluetooth Low Energy (BLE) network captures may contain pairing or data exchange information.

Use [Crackle](https://github.com/mikeryan/crackle) to crack BLE‑encrypted connections and recover encryption keys:

```bash
crackle -i ble_capture.pcap -o decrypted.pcap
```

You can then load **decrypted.pcap** in Wireshark for further analysis.

---

### Analyze USB HID Data

Sometimes the challenge is to make sense of captured USB Human Interface Device (HID) data, such as USB keyboard and mouse captures.

The tool [USB Capture Decoder](https://github.com/jonsth131/ctf-tools/tree/main/usb_capture_decode) is used to decode HID packets.

Documentation for analyzing USB HID data can be found at [HID Usage Tables](https://www.usb.org/sites/default/files/hut1_21.pdf)

---

## Tips

- Always filter protocols to reduce noise
- Look for unusual or nonstandard port usage
- Flags may be encoded or compressed, try base64, hex, gzip
- Combine session reconstruction + payload extraction for full analysis
- If a PCAP is corrupted or truncated, repair it using `pcapfix` before analysis.
- Bluetooth captures sometimes require decryption, use `crackle` for BLE traffic.

## Related Topics

- [Password Cracking](/crypto/password-cracking/) - Wordlists and hash cracking workflows.
