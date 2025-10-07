---
title: Invite Only
description:  Try Hack me Invite Only challenge
author: Prabin Shrestha
date: 2025-10-07 11:33:00 +0800
categories: [Writeup, TryHackMe]
tags: [writeup]
pin: false
math: true
mermaid: true
image:
  path: ../assets/img/favicon/writeup/thm/invite_only/8.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## 🧠 Scenario Overview

You are an SOC Analyst at **TrySecureMe**, assisting an L3 analyst in an ongoing incident response.  
Earlier today, an L1 analyst escalated two suspicious findings for further investigation.

**Flagged Indicators:**
- **IP Address:** `101[.]99[.]76[.]120`
- **SHA256 Hash:** `5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f`

Your task was to use **TryDetectThis2.0** to gather, correlate, and document intelligence from these indicators.
---

## 🔍 Investigation Details

### **Q1: What is the name of the file identified with the flagged SHA256 hash?**

 - Lookup the hash we are given in the tool

 ![Desktop View](../assets/img/favicon/writeup/thm/invite_only/1.png){: width="972" height="589" }

---

### **Q2: What is the file type associated with the flagged SHA256 hash?**
Just below the file name we can see file type as well
---

### **Q3: What are the execution parents of the flagged hash?**  
*(List the names chronologically, using a comma as a separator.)*
 - Go to relation 

 ![Desktop View](../assets/img/favicon/writeup/thm/invite_only/2.png){: width="972" height="589" }
---

### **Q4: What is the name of the file being dropped?**

 ![Desktop View](../assets/img/favicon/writeup/thm/invite_only/3.png){: width="972" height="589" }
---

### **Q5: Research the second hash in Question 3 and list the four malicious dropped files in the order they appear (top to bottom).**

**Third Hash:**  
`fa102d4e3cfbe85f5189da70a52c1d266925f3efd122091cdc8fe0fc39033942`

 - Lookup this hash
 - Go to relation and you can see dropped files

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/4.png){: width="972" height="589" }

---

### **Q6: Analyse the files related to the flagged IP. What is the malware family that links these files?**

**Flagged IP:** `101.99.76.120`

 - Lookup the given IP: 101.99.76.120
 - Go to community

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/5.png){: width="972" height="589" }
---

### **Q7: What is the title of the original report where these flagged indicators are mentioned?**

- You can find the title on the community as well

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/5.png){: width="972" height="589" }
---

### **Q8: Which tool did the attackers use to steal cookies from the Google Chrome browser?**

 - Lookup the article with the title → you ll find many article
 - Use cmd/ctrl + f to lookup the keyword ‘tool’

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/6.png){: width="972" height="589" }
---

### **Q9: Which phishing technique did the attackers use?**
- Look for the keyword ‘technique’

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/7.png){: width="972" height="589" }
---

### **Q10: What is the name of the platform that was used to redirect a user to malicious servers?**

**Answer:**  
`Discord`

![Desktop View](../assets/img/favicon/writeup/thm/invite_only/9.png){: width="972" height="589" }
---

