# BTLO Lab: Log Analysis - Privilege Escalation

## 🎯 Executive Summary
* **Scenario:** Investigation of a compromised Linux server hosting a PHP application where sensitive data restricted to the `root` account was leaked.
* **Log Source:** `.bash_history` (63 executed commands).
* **Attack Chain:** Web Application Filter Bypass (`.phtml` Web Shell) $\rightarrow$ Internal Reconnaissance $\rightarrow$ SUID Binary Exploitation (`Python`) $\rightarrow$ Root Privilege Escalation.

---

## 🔍 Incident Breakdown & Evidence

### 1. User & System Reconnaissance
Analyzed bash history logs to identify internal users and system footprinting actions. The attacker discovered a non-system user account (`daniel`) under the `/home` directory.

![User Reconnaissance](./images/01-recon-and-user.png)
*Figure 1: Identification of user account `daniel` and initial directory enumeration.*

---

### 2. Malicious Tooling & Defense Evasion
The attacker utilized `wget` to pull external enumeration scripts (`linux-exploit-suggester.sh`) and attempted network sniffer deployment via `tcpdump`. To evade web application controls, the attacker bypassed the PHP file upload filter using a `.phtml` extension before cleanup.

![Tools and Bypass](./images/02-tools-and-bypass.png)
*Figure 2: Execution of `linux-exploit-suggester.sh` download and cleanup of the uploaded `.phtml` shell.*

---

### 3. Root Privilege Escalation (SUID Misconfiguration)
Identified the primary privilege escalation vector. After scanning for binaries with the SUID bit set (`find / -perm -4000`), the attacker exploited a misconfigured `/usr/bin/python` binary to spawn a root-privileged `/bin/sh` shell using the `-p` flag.

```bash
# Exploited SUID Python binary to retain root privileges
/usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
