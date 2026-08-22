# 🔍 BTLO Lab: Log Analysis - Compromised WordPress

## 📌 Incident Overview
* **Platform:** Blue Team Labs Online (BTLO)
* **Category:** Log Analysis / Incident Response / Web Security
* **Scenario:** A WordPress site was compromised via a suspected Remote Code Execution (RCE) vulnerability in an installed plugin.
* **Objective:** Analyze raw Apache/Nginx web server access logs using Linux CLI utilities (`grep`, `cut`, `sort`, `uniq`, `head`/`tail`) to trace the attack vector, identify malicious payloads, and determine the scope of the compromise.

---

## 🏗️ Investigation Workflow & Findings

### 1. Initial Triage & Scope Determination
Determined the log timeframe and identified anomalous activity, including automated scanner user-agents (`wpscan`, `sqlmap`, `python-requests`).


# Verify log time boundaries
head -n 1 access.log && tail -n 1 access.log

<img width="1462" height="140" alt="{3FB681A8-FAEE-4A64-AEB3-140B8076D10B}" src="https://github.com/user-attachments/assets/0e9b16f1-0e1a-4ddc-8ebb-7ff77dc6e826" />


# Extract and deduplicate unique User-Agents
cut -d '"' -f 6 access.log | cut -d '[' -f 1 | sort | uniq -c | sort -nr
