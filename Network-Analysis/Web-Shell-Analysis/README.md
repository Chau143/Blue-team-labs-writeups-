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

*(Chèn ảnh chụp màn hình danh sách gói tin HTTP vào đây: `![HTTP Traffic](screenshots/http_stream.png)`)*

### 2. Identifying the Web Shell Upload
By analyzing the HTTP POST requests:
- Observed a multipart/form-data request uploading a suspicious script.
- **Wireshark Filter:** `http.request.method == "POST"`
- **Payload Analysis:** Inspected the file parameters to find PHP/ASP system execution functions (e.g., `system()`, `exec()`, `passthru()`).

### 3. Reconstructing Attacker Commands (Follow HTTP Stream)
Right-clicked the session and selected **Follow -> HTTP Stream** to reconstruct the full conversation:
1. **Command 1:** `whoami` -> Executed as `www-data`
2. **Command 2:** `id` / `uname -a` -> System information gathering
3. **Command 3 (Data Exfiltration / Persistence):** `[Mô tả lệnh tiếp theo kẻ tấn công gõ]`

*(Chèn ảnh chụp màn hình Follow HTTP Stream: `![HTTP Stream](screenshots/payload_extracted.png)`)*

---

## 🛡️ Remediation & Mitigations

Based on the findings from this investigation, the following defense-in-depth measures are recommended:

1. **File Upload Hardening:** Implement strict file-type validation (whitelisting extension & MIME-type checks) on web applications and disable execution permissions in upload directories (e.g., `noexec` flag).
2. **Web Application Firewall (WAF):** Deploy WAF rules to inspect HTTP POST payloads for common web shell signatures and system command syntax.
3. **Endpoint Detection & Response (EDR):** Monitor process creation spawn events from web server processes (e.g., `apache2` or `w3wp.exe` spawning `cmd.exe` / `bash`).
