#  Cyber Threat Intelligence Analysis: MirrorFace (APT)

**Language Proficiency:** English | Español | 日本語

**Skills Demonstrated:** OSINT Translation, Threat Modeling, MITRE ATT&CK Mapping, Detection Engineering (Sigma)

---

##  Executive Summary / エグゼクティブサマリー（事業計画概要）

**[English]**
This project translates and analyzes native Japanese threat intelligence regarding "MirrorFace," an Advanced Persistent Threat (APT) targeting Japanese critical infrastructure, media, and political entities. By synthesizing JPCERT/CC reports, this project maps the actor's Tactics, Techniques, and Procedures (TTPs) to the MITRE ATT&CK framework and provides actionable Sigma detection rules for global Security Operations Centers (SOCs). I found Mirrorface to be an interesting group, as they solely target Japan. I will be doing research, writing as I learn more.

**[日本語]**
本プロジェクトは、日本の重要インフラ、メディア、政治機関を標的とするAPT攻撃グループ「MirrorFace」に関するJPCERT/CCの脅威インテリジェンスを分析・翻訳したものです。攻撃者のTTPs（戦術、技術、手順）をMITRE ATT&CKフレームワークにマッピングし、グローバルなSOCチーム向けに実用的なSigma検知ルールを作成・提案します。

---

##  Threat Actor Profile

*   **Name:** MirrorFace (also tracked as Earth Kasha)
*   **Target Region:** Exclusively Japan (Media, Think Tanks, Diplomatic entities, Manufacturing)
*   **Primary Motivation:** Espionage and Data Exfiltration
*   **Signature Malware:** LODEINFO, NOOPDOOR

##  Methodology

1.  **OSINT Collection:** Gathered native Japanese incident response reports from JPCERT/CC.
2.  **Translation & Analysis:** Translated technical indicators of compromise (IOCs) and attack chains into English.
3.  **Framework Mapping:** Mapped adversary behaviors to MITRE ATT&CK v14.
4.  **Detection Engineering:** Engineered Proof-of-Concept (PoC) Sigma rules to detect lateral movement and persistence mechanisms.

---

##  MITRE ATT&CK TTP Mapping

| Tactic | Technique | ID | Actor Application |
| :--- | :--- | :--- | :--- |
| **Initial Access** | Phishing: Spearphishing Attachment | T1566.001 | Delivers malicious decoy documents via email. |
| **Execution** | Command and Scripting Interpreter | T1059 | Uses PowerShell to execute LODEINFO payloads. |
| **Persistence** | Scheduled Task/Job | T1053.005 | Creates scheduled tasks for persistence. |
| **Defense Evasion** | Obfuscated Files or Information | T1027 | Encrypts payload configurations to evade AV. |

---

##  Repository Structure

*   `/reports` - Contains the full translated Threat Intelligence Briefing (PDF/Markdown).
*   `/detections` - Contains custom Sigma rules (`.yml`) engineered for MirrorFace TTPs.
*   `/references` - Links and raw text of the original Japanese JPCERT/CC sources.

---

##  How to Use the Detections

The Sigma rules in this repository are designed to be converted into specific SIEM queries (Splunk, Sentinel, Elastic) using tools like Uncoder.io or the Sigma CLI. 

1. Navigate to the `/detections` folder.
2. Review the `sigma_scheduled_task.yml` rule.
3. Apply the logic to your specific EDR or Windows Event Log forwarding setup (Targeting Event ID 4698).
