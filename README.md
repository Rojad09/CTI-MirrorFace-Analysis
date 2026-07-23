#  Cyber Threat Intelligence Analysis: MirrorFace (APT)
# サイバー脅威インテリジェンス分析：MirrorFace（APT）

**Language Proficiency Demonstrated:** English | 日本語  

**Skills Demonstrated:** OSINT Collection, Threat Actor Profiling, MITRE ATT&CK Mapping, Sigma Detection Engineering  
**Sources:** JPCERT/CC (Japanese), ESET, Trend Micro, CISA, NPA Japan, NISC Japan

---

## What This Project Is / このプロジェクトについて

**[English]**

This is a Cyber Threat Intelligence (CTI) portfolio project analyzing MirrorFace, a China-aligned APT group that has been targeting Japanese organizations since 2019. I built this as a student practicing real-world CTI analyst skills: reading primary source threat reports (including Japanese-language JPCERT/CC publications), mapping adversary behavior to the MITRE ATT&CK framework, and writing detection rules a SOC team could actually use.

Everything in this repo is based on open-source intelligence. No classified data, no speculation, just publicly available reporting synthesized into analyst-style documentation.

**[日本語]**

本プロジェクトは、2019年から日本の組織を標的にしている、中国を背景とするAPTグループ『MirrorFace』を分析したCTIポートフォリオです。実際のCTIアナリストの業務を実践するために作成しました。JPCERT/CCをはじめとする一次情報（日本語レポート含む）の読解、MITRE ATT&CKフレームワークへのマッピング、そしてSOCチームが実務で活用できる検知ルールの作成を行っています。

このリポジトリ内のすべての情報は、オープンソースインテリジェンス（OSINT）に基づいています。機密情報や憶測は一切含まず、一般に公開されている脅威レポートを統合し、アナリスト形式のドキュメントとしてまとめたものです。

---

## Repository Structure / リポジトリ構成
 
```
/reports      → Full threat intelligence briefing (the main analysis document)
/detections   → Sigma detection rules based on MirrorFace TTPs
/references   → Links and notes from original source material including Japanese JPCERT/CC reports
```
---

## Threat Actor Profile / 脅威アクタープロファイル
 
| Field | Detail |
|-------|--------|
| **Name** | MirrorFace |
| **Also Tracked As** | Earth Kasha (Trend Micro), APT10 subgroup, Purple Typhoon, Bronze Riverside |
| **Origin** | China-aligned |
| **Active Since** | 2019 |
| **Primary Targets** | Japan — media, political orgs, think tanks, universities, manufacturers, research institutes |
| **Motivation** | Espionage and data exfiltration |
| **Signature Malware** | LODEINFO, NOOPDOOR (aka HiddenFace) |
| **Newer Tools** | AsyncRAT (customized), ANEL backdoor, LilimRAT, GOST proxy |
| **Notable Technique** | Abusing Windows Sandbox to hide malware from antivirus detection |

---

## Quick Links / クイックリンク
 
- 
- 
- 

---

## How to Use the Detections / 検知の使い方

The Sigma rules in `/detections` are written in the standard Sigma format and can be converted into SIEM-specific queries (Splunk, Microsoft Sentinel, Elastic) using [Uncoder.io](https://uncoder.io) or the Sigma CLI tool.

---

*Built by a trilingual cybersecurity student (EN / ES / 日本語) as part of a CTI portfolio.*  
*CTIポートフォリオの一環として、サイバーセキュリティを学ぶトライリンガル（英語 / スペイン語 / 日本語）の学生が作成しました。*
