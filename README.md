<div align="center">

# 🎯 MITRE-Sigma Lookup Tool

**One CLI to map MITRE ATT&CK techniques to mitigations, Sigma detection rules and Atomic Red Team tests**

<img src="https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.9+"/>
<img src="https://img.shields.io/badge/MITRE_ATT%26CK-C8102E?style=for-the-badge&logo=databricks&logoColor=white" alt="MITRE ATT&CK"/>
<img src="https://img.shields.io/badge/Sigma-1F6FEB?style=for-the-badge&logo=sigma&logoColor=white" alt="Sigma"/>
<img src="https://img.shields.io/badge/Atomic_Red_Team-D7263D?style=for-the-badge&logo=redhat&logoColor=white" alt="Atomic Red Team"/>
<img src="https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white" alt="Django"/>
<img src="https://img.shields.io/badge/Splunk-000000?style=for-the-badge&logo=splunk&logoColor=white" alt="Splunk"/>

</div>

---

## 📖 Overview

The MITRE-Sigma Lookup Tool lets analysts explore **defensive controls**, **detection content** and **attack emulation** for any ATT&CK technique from a single interface, in the terminal or the browser.

```text
  T1566  ──►  📝 Technique  ──►  🛡️ Mitigations  ──►  🔍 Sigma Rules  ──►  🧪 Atomic Tests
```

---

## 🧭 Table of Contents

- [🎯 Project Background](#-project-background)
- [⚙️ Specifications](#️-specifications)
- [✨ Features](#-features)
- [🚀 Setup](#-setup)
- [💡 Example Usage](#-example-usage)
- [🧩 Features Implemented](#-features-implemented)
- [🔮 Future Features](#-future-features)

---

## 🎯 Project Background

| | Framework | Why it matters |
|:---:|---|---|
| 🗺️ | **MITRE ATT&CK** | A globally accessible knowledge base of adversary tactics and techniques, used to understand attacker behavior, plan defenses and map observed activity |
| 🔍 | **Sigma** | An open, generic signature format for SIEMs. Rules are written once and converted to any SIEM, so linking them to ATT&CK turns threat intel into detection logic fast |
| 🧪 | **Atomic Red Team** | An open-source library of ATT&CK-mapped tests that safely emulate adversary behavior to validate detections, assess controls and practice threat hunting |

Together, they show how an attack is **performed, detected and defended against**.

---

## ⚙️ Specifications

<table>
<tr>
<th>📥 Input</th>
<th>📤 Output</th>
</tr>
<tr valign="top">
<td>

- Technique **ID**, e.g. `T1566`
- Technique **name**, e.g. `Phishing`
- Technique **description**, e.g. *"Adversaries may send victims emails containing malicious…"*

</td>
<td>

- Technique name and description
- Associated ATT&CK **mitigations**
- **Sigma rules** referencing the technique
- **Atomic Red Team** tests for the technique

</td>
</tr>
</table>

---

## ✨ Features

| | Feature |
|:---:|---|
| ⌨️ | Run the tool from anywhere in the terminal with a custom alias |
| 💾 | Export console output to files via Rich Console recording (`-o` / `--output`) |
| 📦 | Bulk-process techniques from comma-separated input or a text file, with per-technique reports and structured output folders |
| 🧪 | Atomic Red Team tests displayed in a formatted table |
| 🌐 | Django web interface with a search dashboard, routing and HTML templates |
| ⚡ | Inverted index + in-memory caching for fast `O(1)` Sigma lookups |
| 🔄 | One-flag `--update` refresh of ATT&CK and Sigma data using GitPython |
| 🔎 | Partial matching on technique names and descriptions, with paginated results |
| 📊 | Splunk integration: converts Sigma rules to SPL with PySigma and deploys them as active alerts via the Splunk SDK |

---

## 🚀 Setup

> [!NOTE]
> **Requirements:** Python 3.9+ and a Linux environment with internet access. Other systems work too, but these instructions are written for Linux.

**1. Go to the folder containing `mitre-attack-sigma.py`**

```bash
cd folder
```

**2. Create and activate a virtual environment**

```bash
python -m venv venv
source venv/bin/activate
```

**3. Install the libraries**

```bash
pip install mitreattack-python pyyaml rich python-dotenv pysigma GitPython pysigma-backend-splunk splunk-sdk
```

---

## 💡 Example Usage

### 🔢 Search by Technique ID

```bash
python mitre-sigma-lookup-tool T1566
```

The run for **`T1566` (Phishing)** prints a full description of the technique: adversaries sending malicious messages through social engineering, spearphishing, malicious attachments or links.

- **🛡️ Mitigations:** `Audit (M1047)`, `Network Intrusion Prevention (M1031)`, `Software Configuration (M1054)`, `Restrict Web-Based Content (M1021)`, `Antivirus/Antimalware (M1049)` and `User Training (M1017)`
- **🔍 Sigma rules:** local rule files referencing `T1566`, such as `proc_creation_win_office_outlook_execution_from_temp.yml` and `iso_phishing.yml`, including the parsed rule *Search-ms and WebDAV Suspicious Indicators in URL*

<details>
<summary><b>📸 View full output for T1566</b></summary>
<br/>
<img width="100%" alt="Lookup output for T1566" src="https://github.com/user-attachments/assets/e95e816f-8da4-4d50-b89d-1f8c3edd9075" />
</details>

### 🔤 Search by Technique Name

```bash
python mitre-sigma-lookup-tool Masquerading
```

Searching **Masquerading (`T1036`)** covers attackers disguising malicious files or activity as something harmless, like renaming malware to `svchost.exe` so it passes as a built-in Windows service.

- **🛡️ Mitigation example:** `Code Signing (M1045)` ensures files carry a valid digital signature, so standard system utilities can't easily be faked
- **🔍 Detection example:** `proc_creation_win_renamed_powershell.yml` and `proc_creation_win_renamed_psexec.yml` watch Windows event logs for processes renamed to hide their identity

<details>
<summary><b>📸 View full output for Masquerading</b></summary>
<br/>
<img width="100%" alt="Lookup output for Masquerading" src="https://github.com/user-attachments/assets/e3ebc8cc-aefc-4253-b852-c2c4aa461d4d" />
</details>

---

## 🧩 Features Implemented

| # | Feature | Writeup |
|:---:|---|:---:|
| 01 | ⌨️ Alias the program into a custom command available anywhere | [View](mitre-sigma-lookup-tool/post-project-implementation-01-alias) |
| 02 | 💾 Handle outputting to files | [View](mitre-sigma-lookup-tool/post-project-implementation-02-handle-file-output) |
| 03 | 📦 Handle bulk searches and outputs | [View](mitre-sigma-lookup-tool/post-project-implementation-03-bulk-search-output) |
| 04 | 🧪 Integrate Atomic Red Team, another ATT&CK-mapped library | [View](mitre-sigma-lookup-tool/post-project-implementation-04-atomic-attack-lib) |
| 05 | 🌐 Django web interface with search bar and hosting | [View](mitre-sigma-lookup-tool/post-project-implementation-05-django-web-interface) |
| 06 | ⚡ Faster Sigma searches | [View](mitre-sigma-lookup-tool/post-project-implementation-06-sigma-speed-searches) |
| 07 | 🔄 Auto-update local MITRE and Sigma data | [View](mitre-sigma-lookup-tool/post-project-implementation-07-auto-mitre-sigma-updates) |
| 08 | 🔎 Smarter search by name or description | [View](mitre-sigma-lookup-tool/post-project-implementation-08-smarter-search) |
| 09 | 📊 Splunk SIEM integration to deploy Sigma rules directly | [View](mitre-sigma-lookup-tool/post-project-implementation-09-splunk-integration) |

---

## 🔮 Future Features

- [ ] 🔥 **MITRE Caldera** integration, an attack platform for launching ATT&CK TTPs
- [ ] 💥 **Metasploit** integration, for emulating attacker TTPs

---

<div align="center">

⭐ **If this tool helped you, consider starring the repo!** ⭐

</div>
