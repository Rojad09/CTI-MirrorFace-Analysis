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

MirrorFace is tracked under different names depending on which security vendor is writing about them. Upon further investigation, the aliases that I am able to find are as follows:

- **JPCERT/CC** → MirrorFace
- **Trend Micro** → Earth Kasha
- **Cybereason** → Cuckoo Spear
- **Microsoft** → Purple Typhoon (formerly Potassium)
- **Multiple vendors** → Considered a subgroup of APT10

> **Why the different names?** Researching this group highlighted a common challenge in Cyber Threat Intelligence that often complicates Advanced Persistent Threat investigations: threat actor convergence. Because security vendors frequently discover and name threats independently, a single group often accumulates multiple aliases before the industry reaches a consensus. Recognizing these overlaps is a critical analytical skill. In this specific case, it is essential to understand that MirrorFace, Earth Kasha, and Purple Typhoon all refer to the same underlying threat actor.

**Target history:**
- **Pre-2023:** Media organizations, political parties, think tanks, universities, diplomatic entities
- **2023 onward:** Manufacturers and research institutions (shift toward intellectual property theft)
- **Mid-2024:** First European target: a Central European diplomatic institute (using Expo 2025 Osaka as a lure)

**Primary goal:** Espionage. They are not financially motivated like ransomware groups. They want data, which refers to input such as government communications, defense research, political strategy documents.

---
