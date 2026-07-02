# Day 13: EOL Service Hardening - Samba / CVE-2007-2447 Lab

**Track**: 100 Days of Cyber (Blue Team)
**Focus**: Recon → Vulnerability Confirmation → Hardening → Validation
**Lab**: VirtualBox | Kali Linux 2026.1.5 [Attacker] + Metasploitable2 192.168.56.3 [Target]
**MITRE ATT&CK**: T1190 - Exploit Public-Facing Application

---

### 1. Objective
Identify and remediate an End-of-Life service running on a vulnerable VM.
Target: Samba 3.0.20 on Metasploitable2 (CVE-2007-2447).
No exploitation performed – all steps are defensive.

### 2. Reconnaissance
Identify open SMB ports and service version.

sudo nmap -sV -p 139,445 192.168.56.3

*Finding*:

139/tcp open netbios-ssn Samba smbd 3.X - 4.X
445/tcp open netbios-ssn Samba smbd 3.X - 4.X

![Day13-Nmap-Samba](Day13-Nmap-Samba.png)

3. Vulnerability Confirmation
Confirm EOL/risky software using NSE vuln scripts.

sudo nmap --script smb-vuln-cve2007-2447 192.168.56.3

*Finding*: Script failed (expected on Kali 2026.1.5). Risk confirmed via open 139/445 + EOL Samba 3.X.
![Day13-Nmap-Samba-Vuln](Day13-Nmap-Samba-Vuln.png)

4. Mitigation / Hardening [Blue Team]
Performed directly on Metasploitable2 to remove the attack surface.

sudo /etc/init.d/samba stop
sudo ufw enable
sudo ufw deny 139/tcp
sudo ufw deny 445/tcp

*Actions*:
1. Stopped Samba daemons `nmbd` and `smbd`.
2. Firewall blocked NetBIOS/SMB ports.
![Day13-Hardening-Actions](Day13-Hardening-Actions.png)

5. Validation
Re-scan from Kali to confirm the risk is closed.

sudo nmap -p 139,445 192.168.56.3

*Result*:

139/tcp filtered netbios-ssn
445/tcp filtered microsoft-ds

![Day13-Validation](Day13-Validation.png)

6. Key Takeaways
1. *EOL = Risk*: Samba 3.0.20 should never be internet-facing.
2. *Defense in Depth*: Stop service *and* firewall ports.
3. *Blue Team Value*: Prove risk reduction with Recon → Hardening → Validation workflow.
