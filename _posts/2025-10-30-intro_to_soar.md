---
title: Introduction To SOAR
description: Walkthrough and answers for the SOAR room (TryHackMe).
author: "Prabin Shrestha"
date: 2025-11-11 11:00:00 +00:00
layout: post
categories: [Writeup, TryHackMe, SOAR]
tags: [soar, playbook, soc, automation]
---

# SOAR — TryHackMe Writeup

**Room link:** https://tryhackme.com/room/soar

---

## Task 02: Traditional SOC and Challenges

**Question:** How would you describe the experience of an overload of security events being triggered within a SOC?  
**Answer:** Alert Fatigue

**Notes / Summary:**  
Traditional SOCs centralize monitoring and incident handling using people, processes, and tools (SIEM, EDR, firewalls, IAM, ticketing). Key SOC capabilities include Monitoring & Detection, Recovery & Remediation, Threat Intelligence, and Communication. Common challenges: alert fatigue, disconnected tools, manual processes, and talent shortage.

---

## Task 03: Overcoming SOC Challenges with SOAR

**Question 1:** The act of connecting and integrating security tools and systems into seamless workflows is known as?  
**Answer:** Orchestration

**Question 2:** What do we call a predefined list of actions to handle an incident?  
**Answer:** Playbook

**Notes / Summary:**  
SOAR = Security Orchestration, Automation, and Response. Core capabilities: Orchestration (connect tools + define playbooks), Automation (execute playbooks without manual clicks), and Response (take actions from a unified interface). SOAR reduces alert fatigue and manual repetitive tasks but does not replace SOC analysts — they remain necessary for judgment calls and playbook creation.

---

## Task 04: Building SOAR Playbooks

**Question 1:** Is manual analysis vital within a SOAR workflow? Yay or Nay?  
**Answer:** Yay

**Question 2:** From where is the CVE Patching playbook fetching the new CVEs?  
**Answer:** Advisory lists

**Question 3:** In the CVE Patching playbook, if the assets are found vulnerable even after the patch is deployed, what does the SOC develop next?  
**Answer:** Mitigation Plan

**Notes / Summary:**  
Example playbooks: Phishing (ingest email, create ticket, check URLs/attachments, TI lookups, sandboxing, remediate) and CVE Patching (ingest advisories, assess asset vulnerability, patch, test, create mitigation if needed). Playbooks often contain decision branches and require analyst involvement at critical points.

---

## Task 05: Threat Intel Workflow Practical

**Scenario summary / guidance:**  
Automate everything that does not require human judgement. Manual steps should remain where deletion or analyst review is required (e.g., deleting case tickets, discarding old alerts, manual analyst extraction, analyst approval of courses of action).

**Summary of recommended automation settings** (as described in the lab):
- **Case Ticket:** Automate everything *except* "Delete Case Ticket".
- **Threat Intelligence Feed:** Automate everything *except* "Discard Old Alerts".
- **Incident Data Extraction:** Automate domains, URLs, IP extraction; do **not** automate "Analyst Extraction".
- **Reputation Checks:** Automate "Reputation Results Output" only; keep validations and sandbox testing manual.
- **Course of Action (COA):** Automate blocking domains/IPs/URLs and updating case tickets; keep "Analyst Approval COA" manual.

**Question:** What is the flag received?  
**Answer:** `THM{AUT0M@T1N6_S3CUR1T¥}`

---

## Final Notes & Tips for Posting

- Save this file as `_posts/2025-11-11-soar-room.md` if you want it treated as a blog post.
- If your site uses a `baseurl`, update any asset/image paths accordingly (e.g., `{{ site.baseurl }}/assets/...`).
- If you get a Jekyll build error after uploading, open **Actions → Build and Deploy → Build site** in GitHub to view the exact error message and paste it here — I can troubleshoot further.

*End of writeup.*
