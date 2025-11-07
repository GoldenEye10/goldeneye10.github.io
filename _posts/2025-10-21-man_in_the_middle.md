---
title: MITM
description: TryHackMe — MITM
author: Prabin Shrestha
date: 2025-11-01 11:33:00 +0800
categories: [Writeup, TryHackMe]
tags: [writeup]
pin: false
math: true
mermaid: true
image:
  path: ../assets/img/favicon/certification/sec+.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## Overview
This writeup summarizes the findings from the TryHackMe **Man-in-the-Middle Detection** room. The aim is to document how the MiTM attacks were identified using packet analysis (Wireshark), list the relevant observations and counts, and provide quick Wireshark filters and notes for replication.

---

## 1. ARP Analysis
**Objective:** Identify ARP-based MiTM activity (gratuitous ARP, ARP spoofing) and determine which hosts claimed the gateway IP.

### Findings
- **1.1 How many ARP packets from the gateway MAC Address were observed?** `10`
- **1.2 What MAC address was used by the attacker to impersonate the gateway?** `02:fe:fe:fe:55:55`
- **1.3 How many Gratuitous ARP replies were observed for 192.168.10.1?** `2`
- **1.4 How many unique MAC addresses claimed the same IP (192.168.10.1)?** `2`
- **1.5 How many ARP spoofing packets were observed in total from the attacker?** `14`

### Explanation & How it was detected
- Gratuitous ARP: observed as ARP replies where the sender IP equals the target IP (common in gratuitous announcements). Two such replies for `192.168.10.1` indicate repeated claims of the gateway address.
- Multiple MAC claims for a single IP (192.168.10.1) are a red flag — legitimate networks typically have one MAC per IP at a time.
- Repeated ARP replies from `02:fe:fe:fe:55:55` were correlated to the spoofing behaviour and used to calculate the total spoofing packet count (`14`).

**Useful Wireshark filters:**
- `arp` — show only ARP traffic
- `arp.opcode == 2` — show ARP replies
- `eth.addr == 02:fe:fe:fe:55:55` — show frames involving the attacker MAC
- `arp.src.proto_ipv4 == 192.168.10.1` — ARP packets that claim to be from the gateway IP

---

## 2. DNS Analysis
**Objective:** Detect DNS poisoning or forged DNS responses returned by the attacker.

### Findings
- **2.1 How many DNS responses were observed for the domain `corp-login.acme-corp.local`?** `211`
- **2.2 How many DNS requests were observed from IPs other than `8.8.8.8`?** `2`
- **2.3 What IP did the attacker’s forged DNS response return for the domain?** `192.168.10.55`

### Explanation & How it was detected
- An unusually large number of DNS responses for the same internal domain (`211`) is suspicious and may indicate repeated forged replies or replayed responses.
- DNS requests coming from sources other than the legitimate recursive resolver (`8.8.8.8` in this scenario) reveal that either hosts are misconfigured or an attacker is injecting responses.
- The forged DNS responses returned `192.168.10.55`, which is likely the attacker-controlled host (or a phishing host) intended to capture credentials.

**Useful Wireshark filters:**
- `dns and (dns.qry.name contains "corp-login.acme-corp.local")` — focus on DNS traffic for the target domain
- `ip.addr != 8.8.8.8 and dns` — DNS packets not from the expected DNS server
- `dns.flags.response == 1` — only DNS responses

---

## 3. HTTP / SSL Stripping Analysis
**Objective:** Identify evidence of HTTP downgrade/SSL stripping and capture plaintext credentials.

### Findings
- **3.1 How many POST requests were observed for our domain `corp-login.acme-corp.local`?** `1`
- **3.2 What’s the password of the victim found in plaintext after successful SSL stripping attack?** `Secret123!`

### Explanation & How it was detected
- A single `POST` request to the login domain that includes plaintext form data (username/password) indicates successful interception and downgrade of traffic — likely via sslstrip or a similar Man-in-the-Middle tool.
- The captured POST body contained the victim’s password in cleartext: `Secret123!`.

**Useful Wireshark filters:**
- `http.request.method == "POST" and http.host contains "corp-login.acme-corp.local"` — show POSTs to the target domain
- `http` — to see unencrypted HTTP content (form fields, POST bodies)
- Use `Follow TCP Stream` on the POST TCP stream to view the full plaintext request and extract credentials.

---

## Conclusion & Recommendations
**Summary:** The capture shows a classic ARP spoofing + DNS forging + SSL stripping chain of MiTM attacks. The attacker used ARP spoofing to position themselves between victim and gateway, forged DNS responses to redirect the victim domain to an attacker IP (`192.168.10.55`), and then performed SSL stripping to capture credentials (`Secret123!`).

**Immediate mitigations:**
- Implement static ARP entries for critical hosts where feasible (or use dynamic detection/alerting for ARP anomalies).
- Protect DNS with DNSSEC and use trusted, authenticated DNS resolvers.
- Enforce HTTPS with HSTS and deploy TLS pinning for sensitive applications where possible.
- Use network-based detection: monitor for multiple MACs claiming one IP, Gratuitous ARP spikes, and unusual DNS response counts.
- Deploy endpoint protections: educate users not to proceed with certificate warnings, ensure browsers and servers enforce secure transport.

**Further steps for practice labs:**
- Reproduce filters above in Wireshark and practice `Follow TCP Stream` and `Follow UDP Stream` to see the raw injected payloads and credentials.
- Use ARP monitoring tools (e.g., `arpwatch`) and IDS/IPS rules to alert on ARP spoofing signatures.

---

## Appendix — Quick reference of answers
- **1.1:** 10
- **1.2:** 02:fe:fe:fe:55:55
- **1.3:** 2
- **1.4:** 2
- **1.5:** 14
- **2.1:** 211
- **2.2:** 2
- **2.3:** 192.168.10.55
- **3.1:** 1
- **3.2:** Secret123!

---

*End of writeup.*

