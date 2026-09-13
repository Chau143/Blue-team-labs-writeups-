# Blue Team Labs Online - Memory Analysis: Ransomware Writeup

## Summary
- **Platform:** Blue Team Labs Online (BTLO)
- **Category:** Digital Forensics / Memory Analysis
- **Tooling:** Volatility 2 / Volatility 3, Linux Terminal, VirusTotal


### Indentifying the suspicious process (psscan/pstree)
_ Open Linux shell was used to run the command: python3 vol.py -f ../infected.vmem windows.pstree > pstree.txt 

<img width="1913" height="1006" alt="image" src="https://github.com/user-attachments/assets/0ac5d0fb-8e0b-4711-94fa-cc7050c11e6e" />

<img width="905" height="144" alt="image" src="https://github.com/user-attachments/assets/11633b88-216e-4d60-8bbc-0ee600cfc5f1" />

### Investigating child process 
