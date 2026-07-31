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
3. [How Their Attacks Have Evolved](#3-how-their-attacks-have-evolved)
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

> **Sourcing note:** JPCERT/CC's own July 2024 blog post confirms MirrorFace leveraging vulnerabilities in Array AG and FortiGate, and states Proself "may also be exploited," but doesn't cite specific CVE numbers. The exact CVE-to-product mapping above is confirmed directly in Trend Micro's November 19, 2024 primary research on Earth Kasha (with direct NVD links for each CVE), corroborated independently by CISA's Known Exploited Vulnerabilities catalog.

> **Why does this matter?** This shift is significant. Phishing requires a human to click something. Exploiting a vulnerable VPN appliance requires no user interaction. You just scan for unpatched devices and exploit them. It also tells us their targets weren't patching fast enough, which is a common real-world problem.

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

### Campaign D (2024–2025) — Expansion Beyond Japan, and a Return to Phishing
ESET named the mid-2024 expansion Operation AkaiRyū (赤龍 — Japanese for "Red Dragon"). Reviewing ESET's primary research directly reveals details substantially richer than what secondary sources conveyed:
 
**Two documented cases in 2024:** On June 20, 2024, MirrorFace targeted two employees of a Japanese research institute using a malicious password-protected Word document. Then on August 26, 2024, MirrorFace targeted a Central European diplomatic institute, which is the first confirmed European target. They used a two-stage spear-phishing email referencing a real prior interaction between the institute and a Japanese NGO. The lure referenced Expo 2025 in Osaka, and the actual malicious payload was only sent after the target replied to the initial benign email.
 
**New tools confirmed directly:** ANEL (also called UPPERCUT), a backdoor previously believed exclusive to APT10 and thought abandoned since 2018–2019, reappeared with updated version numbers (5.5.4), considered strong evidence that its development had restarted. MirrorFace also began using a heavily customized variant of the publicly available AsyncRAT, run inside Windows Sandbox via a complex execution chain, and started abusing Visual Studio Code's remote tunnels feature for stealthy C2 — a technique also used by other China-aligned groups (Tropic Trooper, Mustang Panda).
 
**Victim-specific targeting:** In the diplomatic institute case, ESET tracked two compromised machines over roughly a week. Machine A belonged to a project coordinator; Machine B belonged to an IT employee. MirrorFace deployed different tools and pursued different objectives on each: personal data theft on Machine A (including exporting Google Chrome's stored contact info, autofill data, and stored credit card information into a SQLite database), and deeper network access plus lateral-movement tooling (Rubeus, frp) on Machine B.
 
**A genuine attribution disagreement worth knowing:** ESET's observation of ANEL, combined with targeting overlap and code similarities, led them to firmly change their attribution stance — they now formally consider MirrorFace to be a subgroup under the APT10 umbrella. Trend Micro, conversely, has historically considered the relationship correlated but prefers to track Earth Kasha (MirrorFace) as its own distinct operation. This is a real, sourced disagreement between two credible vendors, highlighting the complexity of tracking converging threat actors.
 
**The campaign continued into 2025.** Concurrently, Trend Micro documented a separate campaign detected in March 2025 targeting government agencies and public institutions in Taiwan and Japan. This campaign utilized a highly specific delivery mechanism:
 
1. **Delivery:** A spear-phishing email with a OneDrive link, downloading a ZIP containing a malicious Excel file (a shift from the malicious Word files used in mid-2024)
2. **Dropper:** The malicious Excel file: a macro-enabled dropper Trend Micro named **ROAMINGMOUSE**. Requires the victim to click (changed from the mouse-move trigger used in the 2024 cases) before dropping its payload
3. **First-stage backdoor:** ROAMINGMOUSE drops a legitimate signed application alongside a malicious loader DLL (**ANELLDR**:Trend Micro's name for what ESET independently describes as the same ANEL-loading mechanism) for DLL side-loading, decrypting and running ANEL in memory
4. **Recon and triage:** operators take screenshots and run basic recon commands (`tasklist /v`, `net localgroup administrators`, `net user`) before deciding whether to escalate. Consistent with the victim-screening behavior ESET documented separately
5. **Second-stage backdoor:** for confirmed valuable targets, NOOPDOOR is downloaded and launched via `MSBuild.exe`, potentially using a tool called **SharpHide** to launch it persistently and hide the MSBuild window
6. **Stealthier C2:** the NOOPDOOR variant in this campaign added support for **DNS over HTTPS (DoH)**, which resulted in resolving its C2 domain through encrypted DNS providers like Google and Cloudflare, to hide suspicious domain lookups from network monitoring
7. **Cleanup:** working directories are deleted with `rd /s /q` after the operation
> **A nuance worth flagging:** It is tempting to describe NOOPDOOR (HiddenFace) simply as "a secondary backdoor," but that undersells how these tools work together. Intelligence indicates ANEL is used as the first-line backdoor immediately after compromise, while NOOPDOOR is only deployed in later stages once the operators decide the target is worth the deeper investment. Interestingly, no use of LODEINFO was observed in these 2024 attacks, a reminder that this actor's toolkit shifts over time, rather than staying fixed.

> **On the Windows Sandbox forensics specifically:** a separate JSAC2025 talk by researchers at ITOCHU Cyber & Intelligence ("Hack The Sandbox: Unveiling the Truth Behind Disappearing Artifacts") covered detection and forensic methodology for the Windows Sandbox abuse technique in more depth than either JPCERT's or ESET's write-ups — including which host-side processes and event logs to monitor, and how to recover sandbox-side artifacts via `vmmem` and YARA rules. This is a genuinely distinct source from the ESET material above, not a duplicate of it.
 
---

