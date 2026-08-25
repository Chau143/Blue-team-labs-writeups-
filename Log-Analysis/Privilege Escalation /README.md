# BTLO Lab: Log Analysis - Privilege Escalation

## 🎯 Executive Summary
* **Scenario:** Investigation of a compromised Linux server hosting a PHP application where sensitive data restricted to the `root` account was leaked.
* **Log Source:** `.bash_history` (63 executed commands).
* **Attack Chain:** Web Application Filter Bypass (`.phtml` Web Shell) $\rightarrow$ Internal Reconnaissance $\rightarrow$ SUID Binary Exploitation (`Python`) $\rightarrow$ Root Privilege Escalation.

---

## 🔍 Incident Breakdown & Evidence

### 1. User & System Reconnaissance
Analyzed bash history logs to identify internal users and system footprinting actions. The attacker discovered a non-system user account (`daniel`) under the `/home` directory.

<img width="1474" height="623" alt="{D76B7054-C4A9-4353-89DD-FBA8DE8DF38C}" src="https://github.com/user-attachments/assets/aafa494b-0cbf-46ab-b24d-4e8d7492a96c" />

*Figure 1: Identification of user account `daniel` and initial directory enumeration.*

---

### 2. Malicious Tooling & Defense Evasion
The attacker utilized `wget` to pull external enumeration scripts (`linux-exploit-suggester.sh`) and attempted network sniffer deployment via `tcpdump`. To evade web application controls, the attacker bypassed the PHP file upload filter using a `.phtml` extension before cleanup.

<img width="1354" height="53" alt="{E873D5BC-909A-492E-A348-497EED4063C1}" src="https://github.com/user-attachments/assets/f983dd9e-2b71-4de2-9cf9-a832c9409548" />

<img width="148" height="31" alt="{E31C56A8-CF10-4B10-81C1-803491F87FED}" src="https://github.com/user-attachments/assets/446de918-eeeb-4fde-97d6-320cab840803" />


<img width="413" height="22" alt="{C455CFF9-B0AD-432B-BDB9-A36E2163A45B}" src="https://github.com/user-attachments/assets/3e040fbe-4790-462a-b129-ecaf5f9deaac" />

*Figure 2: Execution of `linux-exploit-suggester.sh` download and cleanup of the uploaded `.phtml` shell.*

---

### 3. Root Privilege Escalation (SUID Misconfiguration)
Identified the primary privilege escalation vector. After scanning for binaries with the SUID bit set (`find / -perm -4000`), the attacker exploited a misconfigured `/usr/bin/python` binary to spawn a root-privileged `/bin/sh` shell using the `-p` flag

<img width="816" height="53" alt="{FE19D103-97F7-45D8-B1C2-886AD5771EEA}" src="https://github.com/user-attachments/assets/9570dc5f-b501-402d-b5ec-967cfefeb02c" />

