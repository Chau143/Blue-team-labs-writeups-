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

### Threat Intelligence /Malware identification 


