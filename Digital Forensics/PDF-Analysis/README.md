# BTLO Lab: Suspicious USB Stick

## 🎯 Executive Summary
* **Scenario:** A client suffered an employee data breach. Physical evidence collected from the premises included a suspicious USB drive dropped on site.
* **Domain:** Digital Forensics & Malicious Document (Maldoc) Static Analysis.
* **Key Findings:** The USB drive configured `autorun.inf` to launch a weaponized PDF (`README.pdf`). The PDF contained malicious `/OpenAction` streams designed to execute `cmd.exe` on Windows targets upon opening.

---

## 🔍 Investigation Breakdown

### 1. Initial Triage & Autorun Configuration
Inspected the root directory of the USB drive. Parsing `autorun.inf` confirmed that inserting the USB drive triggers an automatic open action pointing to `README.pdf`. Validated the file signature (`%PDF-` / `25 50 44 46 2D`) to ensure file extension legitimacy.

<img width="1915" height="994" alt="{151D5BE2-2F1A-46AC-B9B4-5D8ED9F21F2C}" src="https://github.com/user-attachments/assets/b5f6e4d9-fd12-4244-b204-b03a3052577a" />

<img width="1918" height="969" alt="{CAA5C270-891A-4FDA-96A8-0885F41D9C48}" src="https://github.com/user-attachments/assets/6277882c-f277-4b81-b2e8-72f1b3de47f8" />
-> Once the USB was inserted, it will automatically run and open this particular document called "README.pdf" file 

*Figure 1: Autorun file analysis and magic byte verification.*

### 2. PDF Object Inspection via peepdf 
Utilized `peepdf` to safely analyze the internal tree structure and object streams of `README.pdf` without executing the file. 

The tool flagged an automated execution vector (`/OpenAction`) tied to an embedded object stream, alongside references to Windows binaries (`cmd.exe`).

```bash
# Analyze PDF object structure and trigger actions
peepdf -i README.pdf

<img width="1919" height="980" alt="{002D6EE6-0EDD-45D7-984D-DC74E45288A1}" src="https://github.com/user-attachments/assets/ab159891-cde6-49e8-9989-1a5c5001dd9d" />


# Inspect specific suspicious objects flagged by peepdf (e.g., Object 1,27,28)
PP> object 1,27,28
<img width="1639" height="894" alt="{9251D70C-3B10-47E4-AD10-68C6B5A9AE49}" src="https://github.com/user-attachments/assets/37a6356b-27d7-482d-8d53-8bad399ab05a" />

### 3. Threat Intelligence Hash Correlation
Calculated the cryptographic hash of `README.pdf` and queried VirusTotal to cross-reference known threat indicators.

```bash
# Calculate SHA256 hash for Threat Intel lookup
sha256sum README.pdf
