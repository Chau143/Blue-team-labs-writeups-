# Blue Team Labs Online - Memory Analysis: Ransomware Writeup

## Summary
- **Platform:** Blue Team Labs Online (BTLO)
- **Category:** Digital Forensics / Memory Analysis
- **Tooling:** Volatility 2 / Volatility 3, Linux Terminal, VirusTotal


### Indentifying the suspicious process (psscan/pstree)
_ Open Linux shell was used to run the command: python3 vol.py -f ../infected.vmem windows.pstree > pstree.txt 

<img width="1913" height="1006" alt="image" src="https://github.com/user-attachments/assets/0ac5d0fb-8e0b-4711-94fa-cc7050c11e6e" />

<img width="905" height="144" alt="image" src="https://github.com/user-attachments/assets/11633b88-216e-4d60-8bbc-0ee600cfc5f1" />

### Investigating child process (grep on PID)
_ Command used to run: python3 volatility3/vol.py -f infected.vmem windows.psscan | grep 2732 

<img width="1882" height="222" alt="image" src="https://github.com/user-attachments/assets/8823b232-b272-437e-9e50-da7cebc6536d" />
-> Show the output revealing taskdl.exe (WannaCry's cleanup/ file deletion utility) spawning from PID 2732) -> indicate the ability to drill down into a suspicious PID to uncover auxiliary malicious activity 

### Execution Path Discovery (cmdline/ dllist) 
_ Command used to run: python3 volatility3/vol.py -f infected.vmem windows.cmdline 

<img width="1906" height="1017" alt="image" src="https://github.com/user-attachments/assets/8aa48b9d-8a5e-40f6-bade-9b452302f024" />
-> Capture the complete execution path to show the artifact tracing to determine where the initial payload was launched 

### Threat Intelligence /Malware identification (Dump the binary and hash it)
<img width="1907" height="367" alt="image" src="https://github.com/user-attachments/assets/cc4583c9-e753-4f96-9c6a-09022e6252b5" />

-> 42/69 security vendors flagged this file as malicious on VirusTotal
<img width="1912" height="1028" alt="image" src="https://github.com/user-attachments/assets/523bd546-5e83-4e5d-9a51-e162a6598128" />

### Artifact Identification (filescan / MFT Parser) 
_ Command used to run: python3 volatility3/vol.py -f infected.vmem windows.filescan | grep \.eky. The exact MFT listing for 00000000.eky (the file storing the public key) of WannaCry's cryptographic flow and artifact footprint in memory 

<img width="1916" height="152" alt="image" src="https://github.com/user-attachments/assets/1de4e424-e00d-457a-89df-8fdc6a02cf65" />



