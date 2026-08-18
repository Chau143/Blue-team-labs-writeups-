# 🔍 Investigation Report: Network Analysis - Web Shell

**Platform:** Blue Team Labs Online (BTLO)  
**Category:** Network Security / Traffic Analysis  
**Difficulty:** Easy / Medium  
**Tools Used:** Wireshark, TShark, CyberChef  

---

## 📌 Executive Summary
During a routine network security audit, anomalous HTTP traffic was detected communicating with an internal web server. A packet capture (`.pcap`) file was provided for forensic investigation. The objective of this analysis was to investigate the network traffic, identify the uploaded malicious web shell, extract the attacker's commands, and determine the scope of compromise.

---

## 🎯 Investigation Objectives & Findings

| Objective | Finding / Value |
| :--- | :--- |
| **Attacker IP Address** | 10.251.96.4 |
| **Target Web Server IP** | 10.251.96.5 |
| **Name of Uploaded Web Shell** | editprofile.php |
| **HTTP Request Method Used** | `POST` / `GET` |
| **Attacker's User-Agent** | Suspicious tools: gobuster 3.0.1 && sqlmap 1.4.7 |


---

## 🛠️ Step-by-Step Technical Analysis

### 1. Initial Traffic Triage & Protocol Hierarchy
First, inspect the network capture statistics and filter for web traffic:
- Applied Wireshark filter: `http` or `http.request`
- Identified a high volume of HTTP requests targeting the web directory `/uploads/` (or similar).

<img width="1919" height="1026" alt="{38D0FB0B-6449-4341-AC95-E97237E4F746}" src="https://github.com/user-attachments/assets/6090ac34-eaca-42e5-8be9-040b9a366e17" />


### 2. Identifying the Web Shell Upload
By analyzing the HTTP POST requests:
- Observed a multipart/form-data request uploading a suspicious script.
- **Wireshark Filter:** `http.request.method == "POST"`
- **Payload Analysis:** Inspected the file parameters to find PHP/ASP system execution functions (e.g., `system()`, `exec()`, `passthru()`).

<img width="1915" height="1017" alt="{EEA63358-7F84-4514-9954-F48E1BB35993}" src="https://github.com/user-attachments/assets/9aae6e38-b5d5-4626-abbb-ae6f33e0c91f" />

<img width="1259" height="1003" alt="{EFC97251-6197-44B0-89CB-43782FB72145}" src="https://github.com/user-attachments/assets/82760bd9-e711-4639-a2f3-ba87cbe75940" />

### 3. Reconstructing Attacker Commands (Follow HTTP Stream)
<img width="1914" height="1017" alt="{8C026DBD-5E34-4116-A3B8-C31762620424}" src="https://github.com/user-attachments/assets/f5fde882-c733-46e6-86dc-60901fdc7e17" />



Right-clicked the session and selected **Follow -> HTTP Stream** to reconstruct the full conversation:
1. **Command 1:** `id` -> System information gathering
<img width="891" height="535" alt="{BA7EB9EA-EBF0-4730-8679-16593548AF14}" src="https://github.com/user-attachments/assets/aa7e14e2-3ef0-4e9e-a99b-c1525206eb2e" />

2. **Command 2:** `whoami` ->  Executed as `www-data`
<img width="1044" height="559" alt="{216EA8C7-BD15-41FB-AB23-84B94F516501}" src="https://github.com/user-attachments/assets/8c8b9d64-00af-47b7-a07f-93f5ff8b0621" />

4. **Command 3 (Data Exfiltration / Persistence):** `The third command is Python script. This script tells is that is it a reverse shell to 210.251.96.4:4422 to utilize /bin/sh`
<img width="1261" height="725" alt="{FD79BC44-B1E3-4CE4-9519-1E67A29C533C}" src="https://github.com/user-attachments/assets/cb74864a-4d41-45a9-ab11-14bebfafe48d" />




---

## 🛡️ Remediation & Mitigations

Based on the findings from this investigation, the following defense-in-depth measures are recommended:

1. **File Upload Hardening:** Implement strict file-type validation (whitelisting extension & MIME-type checks) on web applications and disable execution permissions in upload directories (e.g., `noexec` flag).
2. **Web Application Firewall (WAF):** Deploy WAF rules to inspect HTTP POST payloads for common web shell signatures and system command syntax.
3. **Endpoint Detection & Response (EDR):** Monitor process creation spawn events from web server processes (e.g., `apache2` or `w3wp.exe` spawning `cmd.exe` / `bash`).
