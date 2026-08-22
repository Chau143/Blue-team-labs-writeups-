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
cat access.log | cut -d ' ' -f 1 | sort | uniq -c | sort -nr -- This showed the top hit of the ip address of access.log file was 172.21.0.1
<img width="1041" height="773" alt="{B4776EB8-BED3-4F3E-9D0B-63A9C37AAF18}" src="https://github.com/user-attachments/assets/e992b332-3d7b-4a18-be20-16933e5b804e" />

# Extract and deduplicate unique User-Agents 
cat access.log | cut -d '"' -f 6 | cut -d '[' -f 1 | sort | uniq -c | sort -nr
<img width="1481" height="778" alt="{E919B4E7-28A0-4CF9-86AC-B3A32B072EC3}" src="https://github.com/user-attachments/assets/4e73d948-6c0d-4a73-bc78-6e0fdbe22731" />
# Extract and find the 'POST' request and eliminate '403' -- It is the request that send data to the server 
grep 'POST' access.log | grep -v '403' | cut -d ' ' -f 1 | sort | uniq -c | sort -nr
<img width="1224" height="269" alt="{EFE5D850-3052-4015-BACE-5A154F5E7E86}" src="https://github.com/user-attachments/assets/8070cd97-821e-4eda-8a8b-e7c6537dc0f6" />
# Filter and check the IP of '103.69.55.212' from the second largest POST request 
<img width="1460" height="80" alt="{5E23C028-2AF9-4400-9745-FAB815AF5EF2}" src="https://github.com/user-attachments/assets/a6b23384-a74b-4471-a8c5-2cd14ff43410" />

# Identifying attack vector & Vulnerable plugin -- Filter 'contact-form-7' && 'simple-file-list' 
<img width="1445" height="78" alt="{9ED683E4-8A7D-412A-99F4-0E9E5A42095E}" src="https://github.com/user-attachments/assets/7fe4e413-333c-4971-9255-caae0e2a22b8" />

<img width="1457" height="80" alt="{0946F9EC-C999-4797-B4DA-F085E127C3D6}" src="https://github.com/user-attachments/assets/1a8a1b43-de4c-4f0a-b24a-e192b773d8e6" />
