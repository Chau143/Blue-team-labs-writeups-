# 🛡️ SOC & Blue Team Investigation Portfolio

Welcome to my Cyber Defense & Incident Response portfolio. This repository contains my technical write-ups, PCAP analyses, memory forensics, and SIEM threat hunting cases solved on **Blue Team Labs Online (BTLO)**.

🔗 **BTLO Public Profile:** https://blueteamlabs.online/public/user/867f6ba62e4034fa3fd18a

✉️ **Contact / LinkedIn:** https://www.linkedin.com/in/chau-than-b43730300/

---

## 📂 Completed Investigations & Write-ups

### 🌐 Network & Traffic Analysis
* 🔍 **[Network Analysis - Web Shell](./Network-Analysis/Web-Shell-Analysis/)** 
  * *Focus:* Wireshark, HTTP Stream Analysis, Web Shell Command Extraction, Incident Timeline Construction.
* 🔍 **[Log Analysis - Compromised WordPress](./Log-Analysis/Compromised-WordPress)**
   * *Focus:* Web Threat Hunting & Log Triage, Identify Attack Vector & Root Cause, Trace Attacker Footprints(IOCs), Mastering CLI(grep, cut, sort, uniq, head,tail)
* 🔍 **[Email Analysis - The Planet's Prestige](<./Email-Analysis/The planet's prestige>)**
  *Focus:* Email & Document analysis (PDF, Word Macro, Zip file), Static Malware Triage(without running malware), Extracting C2&IoCs, Used tools(HxD, exiftool) to identify the metadata and file extensions
* 🔍 **[Malware Compromised Analysis - Malware Compromised](<./Network-Analysis/Malware Compromised>)**
  *Focus:* Identifying network-based IOCs of Dridex malware-- a banking Trojan, analyzed the establishment of connection between client and server via TLS Certificate issuer infomation
* 🔍 **[Log Analysis - Privilege Escalation](<./Log-Analysis/Privilege Escalation>)**
  *Focus:* Analyzing Linux command-line artifacts from compromised .bash_history log files, tracing attacker TTPs: Web application filter bypass (.phtml shell) and defense evasion, identifying local privilege escalation vectors via misconfigured SUID binaries (/usr/bin/python/)
* 🔍 **[Digital Forensics - PDF Analysis ](<./Digital Forensics/PDF->)**
  
---

*Note: Write-ups in this repository are structured for educational purposes and proof-of-work documentation.*
