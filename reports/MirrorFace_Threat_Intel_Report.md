# MirrorFace Threat Intelligence Report 
# MirrorFace 脅威インテリジェンスレポート

**Author:** Cybersecurity Student (B.A.S. Cybersecurity & Ethical Hacking, CompTIA Security+ / CySA+)  
**Date:** 2025  
**Classification:** UNCLASSIFIED — Open Source Research Only  
**Primary Sources:** JPCERT/CC, ESET, CISA, NPA Japan, NISC Japan, Trend Micro

---

 **A note on my process:**  

> I read (and I am currently reviewing) the original JPCERT/CC reports in Japanese and cross-referenced them with English publications from ESET, CISA, and Trend Micro. Where the Japanese source emphasized something the English reports glossed over (or vice versa), I noted it. That comparison is part of what makes this report different from just summarizing a single English blog post.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Who Is MirrorFace?](#2-who-is-mirrorface)
* **(ADDING MORE AS I FINISH THE REPORT)**

---

## 1. Executive Summary

**[English]**
 
MirrorFace is a China-linked advanced persistent threat (APT) group that has been actively targeting Japanese organizations since at least 2019. They are primarily motivated by espionage, seeking to steal sensitive data, intellectual property, and political intelligence from Japan's government, media, defense industry, and research sectors.
 
What makes MirrorFace an interesting study subject is how clearly their tactics have adapted over time. They began with simple spear-phishing emails before shifting to exploiting unpatched vulnerabilities in network devices. Eventually, they developed a clever technique to hide their malware inside Windows Sandbox, which is a built-in Windows feature that most antivirus software cannot monitor. In 2024, they expanded their operations beyond Japan for the first time, targeting a European diplomatic institute by using the upcoming Expo 2025 in Osaka as a lure.
 
As a student, studying this group taught me what the "persistent" in APT truly means. JPCERT/CC has documented cases where MirrorFace maintained access to compromised networks for over two years without being detected.

**[日本語]**
 
MirrorFaceは、少なくとも2019年から日本の組織を標的にしている中国系APTグループです。主な目的はスパイ活動であり、日本の政府機関、メディア、防衛産業、研究機関から機密データや知的財産、政治情報を窃取しようとしています。

このグループを研究対象として選んだ理由の一つは、その適応能力の高さが明確に見て取れるからです。当初は単純なスピアフィッシングメールを使用していましたが、その後はネットワーク機器の脆弱性悪用に移行し、さらにはWindowsサンドボックスという多くのウイルス対策ソフトが検知できない機能を悪用してマルウェアを隠す手法まで開発しました。2024年には、大阪・関西万博をおとりとして悪用し、初めてヨーロッパの外交機関を標的にするなど、活動範囲を日本以外にも拡大しています。

一人の学生として、このグループの調査を通してAPTにおける「持続的（Persistent）」という言葉の真意を学びました。JPCERT/CCの報告によると、MirrorFaceが侵害したネットワークへのアクセスを、検知されることなく2年以上にわたり維持し続けた事例も確認されています。

---

## 2. Who Is MirrorFace?

MirrorFace is tracked under different names depending on which security vendor is analyzing their activity. Upon further investigation, the aliases break down as follows:

- **JPCERT/CC** → MirrorFace
- **Trend Micro** → Earth Kasha
- **Cybereason** → Cuckoo Spear (Campaign Name)
- **Multiple vendors** → Confirmed as a subgroup of the APT10 umbrella (which is tracked by Microsoft as Purple Typhoon, formerly Potassium).

> **Why the different names?** Researching this group highlights a common challenge in Cyber Threat Intelligence: threat actor convergence. Because security vendors frequently discover and name threats independently, a single group often accumulates multiple aliases before the industry reaches a consensus. Recognizing these overlaps is a critical analytical skill. In this case, it is essential to understand that MirrorFace, Earth Kasha, and the Cuckoo Spear campaign all refer to the same specific subgroup operating within the broader APT10 (Purple Typhoon) collective.

**Target history:**
- **Pre-2023:** Media organizations, political parties, think tanks, universities, and diplomatic entities (primarily in Japan).
- **2023 Onward:** Manufacturers and research institutions (indicating a strategic shift toward intellectual property theft).
- **Mid-2024:** First known European target. The group attacked a Central European diplomatic institute using the upcoming Expo 2025 in Osaka, Japan as a spear-phishing lure.

**Primary goal:** Espionage. Unlike financially motivated ransomware groups, this actor's objective is pure data exfiltration, specifically targeting high-value intelligence like government communications, defense research, and political strategy documents.

---

## 3. How Their Attacks Have Evolved

This is the section I found most useful when trying to understand MirrorFace, because it shows a real attacker thinking and adapting; not just running the same playbook forever.

### Campaign A (2019–2022) — Spear-Phishing Phase

MirrorFace's original method was straightforward: send a carefully crafted email with a malicious attachment to a specific target. The emails were written in Japanese and referenced real Japanese political events, which tells us the operators have Japanese language capability or local knowledge.

Opening the attachment delivered LODEINFO, their primary backdoor.

### Campaign B (April 2023 onward) — Vulnerability Exploitation Phase

They stopped relying entirely on phishing and started exploiting unpatched vulnerabilities in internet-facing network devices:

- **Array AG VPN** — CVE-2023-28461 (CVSS 9.8, unauthenticated RCE)
- **Fortinet FortiGate/FortiProxy** — CVE-2023-27997 (heap-based buffer overflow in SSL-VPN)
- **Proself** (a Japanese file-sharing product) — CVE-2023-45727

**Sourcing note:** JPCERT/CC's own July 2024 blog post confirms MirrorFace leveraging vulnerabilities in Array AG and FortiGate, and states Proself "may also be exploited," but doesn't cite specific CVE numbers. The exact CVE-to-product mapping above is confirmed directly in Trend Micro's November 19, 2024 primary research on Earth Kasha (with direct NVD links for each CVE), corroborated independently by CISA's Known Exploited Vulnerabilities catalog.

**Why does this matter?** This shift is significant. Phishing requires a human to click something. Exploiting a vulnerable VPN appliance requires no user interaction. You just scan for unpatched devices and exploit them. It also tells us their targets weren't patching fast enough, which is a common real-world problem.

### Campaign C (June 2023 onward) — Windows Sandbox Evasion Phase
This is where it gets technically interesting. MirrorFace started running their malware *inside* Windows Sandbox — a legitimate built-in Windows feature designed to let you run untrusted software safely.

Here is the attack chain step by step:
1. Attacker gains initial access (via phishing or vulnerability exploitation)
2. Attacker enables Windows Sandbox on the host machine (this requires admin rights and a system reboot).
3. Attacker drops the necessary files on the victim machine: a .wsb (Windows Sandbox configuration) file, an extraction script, and a password-protected archive containing the malware payload.
4. Attacker creates a Scheduled Task on the host machine. This task is configured to launch the Sandbox silently under a different user profile, ensuring the victim never sees the Sandbox window pop up.
5. When the Sandbox starts, the .wsb config automatically mounts the host folder and executes the script.
6. The script extracts the malware and establishes a connection to the attacker's C2 server inside the Sandbox, completely hiding the malicious execution and network traffic from the host's antivirus software.

> **Why this is clever:** Windows Sandbox is meant to *protect* you by isolating untrusted software. MirrorFace flipped this, as they used the isolation to hide *from* your security tools. As of January 2025, Japan's National Police Agency (NPA) and NISC issued a joint advisory specifically warning about this evasion technique.


