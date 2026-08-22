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
head -n 1 access.log && tail -n 1 access.log -- The first event was recorded on 12/Jan/2021 at 15:52:41 and the last event was on 14/Jan/2021 at 07:46:52. Both events happened on the source_ip at 172.21.0.1

<img width="1462" height="140" alt="{3FB681A8-FAEE-4A64-AEB3-140B8076D10B}" src="https://github.com/user-attachments/assets/0e9b16f1-0e1a-4ddc-8ebb-7ff77dc6e826" />


# Extract and deduplicate unique Ip Address 
cat access.log | cut -d ' ' -f 1 | sort | uniq -c | sort -nr 
<img width="1041" height="773" alt="{B4776EB8-BED3-4F3E-9D0B-63A9C37AAF18}" src="https://github.com/user-attachments/assets/e992b332-3d7b-4a18-be20-16933e5b804e" />
