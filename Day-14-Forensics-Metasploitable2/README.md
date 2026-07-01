# Day 14: Blue Team Forensics - Metasploitable2

**Role**: Blue Team Forensics Analyst  
**Target**: Metasploitable2 `192.168.56.3`  
**Date**: 2026-06-29  
**Challenge**: #30DayCybersecurityChallenge Day 14

---

### **1. Objective**
Perform forensic analysis on a compromised Metasploitable2 VM. Identify initial access, enumerate the host, and spot anti-forensics techniques.

### **2. Lab Setup**
**Target VM**: Metasploitable2 `192.168.56.3`
**Analyst Tools**: Kali Linux, `grep`, `cat`, `ls -la`

### **3. Key Findings**
1.  **Initial Access**: `vsftpd v2.3.4` backdoor on TCP/21 -> root shell, `uid=0`
2.  **Host Enumeration**: EOL kernel `2.6.24-16-server` = no patching, easy privesc 
3.  **Anti-Forensics**: `T1070.003` - Attacker ran `ln -s /dev/null ~/.bash_history`. History = 0 bytes.

### **4. Evidence**
![vsftpd log](../screenshots/Day14-Forensics-left.png) *Left: /var/log/vsftpd.log showing 15x CONNECT*
![empty bash_history](../screenshots/Day14-Forensics-right.png) *Right: Empty .bash_history after anti-forensics*

### **5. Blue Team Takeaway**
Logs don't lie. `/var/log/vsftpd.log` showed `15x CONNECT` from attacker IP, but `.bash_history` was wiped. 

**Lesson**: If you only check bash history, you miss the intrusion. Correlate logs + artifacts.

### **6. MITRE ATT&CK Mapping**

| Tactic | Technique | ID |
| --- | --- | --- |
| Initial Access | Exploit Public-Facing Application | T1190 |
| Defense Evasion | Indicator Removal: Clear Command History | T1070.003 |

---
**Tags**: #BlueTeam #DFIR #SOCAnalyst #Metasploit #MITREATTACK #CyberSecurity
