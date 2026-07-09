# 100DaysOfCyber - Day 23: Command Injection Lab

## Overview
This is Day 23 of my #100DaysOfCyber journey, where I explore web application security and ethical hacking.

## Lab Environment
- **Attacker**: Kali Linux (Burp Suite + Firefox)
- **Target**: Ubuntu VM (DVWA) at `192.168.56.101`

## Objective
Exploit Command Injection vulnerability in DVWA to achieve Remote Code Execution (RCE).

## Tools Used
- Burp Suite Community Edition
- Firefox
- nmap
- DVWA (Damn Vulnerable Web Application)

## Steps
1. **Recon**
   - Scanned target with `nmap -sC -sV -oN dvwa_scan.txt 192.168.56.101`
   - Identified open ports: 22 (ssh), 80 (http), 443 (https)
     ![Recon-Scan Screenshot](Day-23-Recon- Scan.png)
     
2. **Exploit**
   - Intercepted request to `http://192.168.56.101/dvwa/vulnerabilities/command/`
   - Modified `ip` parameter to `127.0.0.1; whoami`
   - Forwarded request via Burp Suite
     ![Burp Intercept Screenshot](Day-23-Burp Intercept.png)
     
3. **Result**
   - Server responded with `www-data`, confirming RCE

## Screenshots
![Command-Injectiont Screenshot](Day-23-Command-Injection.png)

## Key Takeaway
- Command Injection occurs when apps pass unsanitized input to OS commands
- Use input validation, whitelisting, and least privilege to mitigate

## Next Steps
- Turn this RCE into a reverse shell
- Practice on other DVWA modules

## Commands Used
```bash
nmap -sC -sV -oN dvwa_scan.txt 192.168.56.101
# Burp Suite: Modify ?ip=127.0.0.1; whoami
