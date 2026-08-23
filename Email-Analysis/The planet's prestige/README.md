## 📌 Key Focus & Objectives

This lab focuses on **Phishing & Malicious Document Analysis (DFIR/Blue Team)**:
* **Initial Access Triage:** Analyzing phishing attachments and malicious documents to identify initial entry vectors (MITRE ATT&CK T1566).
* **Static Malware Analysis:** Extracting embedded URLs, hidden macros/scripts, and metadata without executing the payload.
* **IoC Extraction & Threat Intel:** Uncovering Command & Control (C2) domains, malicious IP addresses, and obfuscated payloads for SOC enrichment.

## Decode base64 via CyberChef of the first boundary to identify the content of it
<img width="1914" height="950" alt="{7690CF6D-8634-4001-AD66-334EC886D886}" src="https://github.com/user-attachments/assets/abcbc576-a26c-4218-bd96-e7175ca410e5" />

## Decode base64 via CyberChef of the end of the boundary to identify the end of the content and then used garykessler to identify which file type is 
<img width="1913" height="726" alt="{DD0F3875-1633-492A-A9E6-F3CCA8236661}" src="https://github.com/user-attachments/assets/bfbb982f-127c-470b-80f8-c7ce305cf6fe" />
<img width="1782" height="857" alt="{84B058B2-B8E1-475A-AC2E-7ED565777533}" src="https://github.com/user-attachments/assets/bf9dbbb9-6a73-4cc5-85e5-b0cd295274bd" />
Note: The file was identified as 'Zip' file even the content type displaced as pdf in the file name 

## Used HxD tool to analyze three file extensions from the ZIP file of the end boundary above 
#### 1.DaughterCrown file 
<img width="1415" height="688" alt="{4A940630-3C4C-4275-AED3-6EBE58693A2B}" src="https://github.com/user-attachments/assets/663ef87f-4f5f-4e61-a247-237c8efaed5a" />

Note: Because of the first couples bites and then checked on garykessler -> identified as JPEG file 


<img width="50" height="30" alt="{B36ECB6A-A647-44BF-BBE1-06A6396D9529}" src="https://github.com/user-attachments/assets/f4e75dc0-90a3-4c5c-849f-2f1c8b825f0d" />

### 2.GoodJobMajor file 
<img width="1413" height="706" alt="{F6764F49-D78A-4235-8F24-4C010599FE21}" src="https://github.com/user-attachments/assets/6ee64c30-088d-491a-b05a-a878eacd52e7" />

Note: Because of the first couples bites and then checked on garykessler -> identified as PDF file 

<img width="1917" height="967" alt="{10894330-21E5-4705-A4C3-C7AF6D19DE55}" src="https://github.com/user-attachments/assets/09b2829e-4504-4c1a-a3b0-68b90c3630af" />

### 3.Money.xlsx file 
<img width="1401" height="709" alt="{9CD34CA9-771C-4AF2-AFB2-7EC24F382D5B}" src="https://github.com/user-attachments/assets/7ad9d872-b3d2-4710-9ea9-eee6d413ac46" />
Note: Because of the first couples bites and then checked on garykessler -> identified as Microsoft Office Open XML Format Document (Excel)
<img width="1911" height="603" alt="{24F1AA79-D667-49AB-9B09-9E61623D7F95}" src="https://github.com/user-attachments/assets/98466cc1-628a-4e48-b2b4-d9e0bf0ca1ad" />
-> The result of encode64 of sheet2 
