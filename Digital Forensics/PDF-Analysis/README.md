# BTLO Lab: Suspicious USB Stick

## 🎯 Executive Summary
* **Scenario:** A client suffered an employee data breach. Physical evidence collected from the premises included a suspicious USB drive dropped on site.
* **Domain:** Digital Forensics & Malicious Document (Maldoc) Static Analysis.
* **Key Findings:** The USB drive configured `autorun.inf` to launch a weaponized PDF (`README.pdf`). The PDF contained malicious `/OpenAction` streams designed to execute `cmd.exe` on Windows targets upon opening.

---

## 🔍 Investigation Breakdown

### 1. Initial Triage & Autorun Configuration
Inspected the root directory of the USB drive. Parsing `autorun.inf` confirmed that inserting the USB drive triggers an automatic open action pointing to `README.pdf`. Validated the file signature (`%PDF-` / `25 50 44 46 2D`) to ensure file extension legitimacy.

![Autorun Inspection](./images/01-autorun-magic-bytes.png)
*Figure 1: Autorun file analysis and magic byte verification.*

---

### 2. Threat Intelligence Hash Correlation
Calculated the cryptographic hash of `README.pdf` and queried VirusTotal to cross-reference known threat indicators.

```bash
# Calculate SHA256 hash for Threat Intel lookup
sha256sum README.pdf
