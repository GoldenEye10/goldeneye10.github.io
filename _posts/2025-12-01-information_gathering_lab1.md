---
title: Information Gathering
description: Ejpt lab 1 - Information Gathering
author: Prabin Shrestha
date: 2025-12-01 11:33:00 +0800
categories: [Writeup, ejpt]
tags: [writeup, redteam]
pin: false
math: true
mermaid: true
image:
  path: ../assets/img/favicon/writeup/thm/seven_minutes/1.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## Q1. This tells search engines what to and what not to avoid.

As we know, the `robots.txt` file tells search engines what to crawl and what to avoid.  
Checking the **robots.txt** file reveals our first flag.

**FLAG 1:**  
`f2ee2d09e076462eadce8807895a4461`

---

## Q2. What website is running on the target, and what is its version?

To determine the version of the website, we can use Nmap to identify the server and version.

Run:

```
nmap target.ine.local -sC -sV
```

This reveals the service/version information and the second flag.

**FLAG 2:**  
`3395843029c743ddb00c9adac8b2c7cc`

---

## Q3. Directory browsing might reveal where files are stored.

To brute-force directories, run:

```
dirb http://target.ine.local
```

After manually exploring the discovered directories, the flag is found in:

```
/wp-content/uploads
```

**FLAG 3:**  
`ccca869e58f54c02b0fa4f9b5a1ee84f`

---

## Q4. An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.

To find backup files, specify backup-related extensions using `-X`:

```
dirb http://target.ine.local -w /usr/share/dirb/wordlists/big.txt -X .bak,.tar.gz,.zip,.sql,.bak.zip
```

A backup file is discovered. Read it using:

```
curl http://target.ine.local/wp-config.bak
```

This reveals the fourth flag.

**FLAG 4:**  
`de9e6050a6de44daa74e91e87b3112f3`

---

## Q5. Certain files may reveal something interesting when mirrored.

To mirror the target website, use:

```
httrack http://target.ine.local -O target.html
```

After mirroring, navigate through the saved files.  
The final flag is located in:

```
xmlrpc0db0.php
```

**FLAG 5:**  
`e79d9c81cc384cdd91bbc563fd61b7ee`

---

Thank you for reading!  
See you all in the next CTF.

**Happy Hacking!**
