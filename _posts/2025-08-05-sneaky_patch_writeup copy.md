---
title: TryHackMe - Sneaky_Patch
description: Investigate the potential kernel backdoor implanted within the compromised system.
author: Prabin Shrestha
date: 2025-08-05 11:33:00 +0800
categories: [Writeup, TryHackMe]
tags: [writeup]
pin: false
math: true
mermaid: true
image:
  path: ../assets/img/favicon/writeup/thm/sneaky_patch/1.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

# TryHackMe - Sneaky Patch Writeup

## Room: [Sneaky Patch](https://tryhackme.com/room/hfb1sneakypatch)

### Step 1: Load Kernel Modules
Run:
```bash
lsmod
```
We find: spatch

![Desktop View](../assets/img/favicon/writeup/thm/evilgpt/7.png){: width="972" height="589" }

This stands out because:
- **spatch** is not a standard Linux kernel module.
- Its name resembles “sneaky patch” → which matches the room’s name.
- It is the only unknown, custom-looking module in the list.
- It’s 12 KB, typical for small rootkits.

---

### Step 2: Check Module Info
Run:
```bash
sudo modinfo spatch
```

![Desktop View](../assets/img/favicon/writeup/thm/evilgpt/2.png){: width="972" height="589" }

Clearly a suspicious kernel module → likely a rootkit.

---

### Step 3: Analyze the Module with Strings

Let's try and read the module with following command
Run:
```bash
strings /lib/modules/6.8.0-1016-aws/kernel/drivers/misc/spatch.ko
```
Scrolling up, around the top, we find:
```
6[CIPHER BACKDOOR] Here's the secret: 
```

![Desktop View](../assets/img/favicon/writeup/thm/evilgpt/3.png){: width="972" height="589" }

This hex string looks like the flag. so lets decode it with cyberchef

---

### Step 4: Decode the Flag

Decode the hex string with 'space' delimiter

![Desktop View](../assets/img/favicon/writeup/thm/evilgpt/4.png){: width="972" height="589" }

And we found the flag~~
---

### Conclusion

The Sneaky Patch room was a great exercise in spotting persistence through kernel-level tampering. By inspecting loaded modules and digging into the suspicious spatch.ko, we uncovered a hidden backdoor cleverly embedded as a kernel patch. This challenge highlights the importance of:

Baseline awareness: knowing what modules and binaries are normal for your system.

Integrity checking: validating system components against trusted sources.

Threat hunting mindset: persistence often hides in plain sight under the guise of legitimate processes.

In real-world environments, malicious kernel modules like this would allow attackers to maintain stealthy, long-term access. Detecting them requires vigilance, proper logging, and routine integrity monitoring.

Ultimately, the challenge was a reminder that even the smallest “sneaky patch” can compromise the entire system — and as defenders, it’s on us to notice the anomalies.

---


