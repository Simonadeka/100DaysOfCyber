# Day 19: Fail2ban SSH Defense

## Goal
Implement automated defense against SSH brute-force attacks using fail2ban on Metaploitable 2

## Project Context & Inspiration
This project focuses on the automated prevention of credential attacks. For an excellent companion study on how these same attacks are monitored, visualized, and documented inside a Security Operations Center (SOC) using a SIEM, check out this repository: 🔗 [Splunk Brute Force Detection & Analysis by rutuja-late](https://github.com/rutuja-late/Splunk-Brute-Force-Detection)

## Objective
1. Configure fail2ban to monitor and ban IPs after 3 failed SSH login attempts
2. Simulate brute-force attack using Hydra from Kali Linux
3. Verify IP ban with iptables and fail2ban-client

## Tools & Environment
• **Target**: Metaploitable 2
• **Attacker**: Kali Linux
• **IPS**: fail2ban
• **Attack Tool**: Hydra

## Note
Kali needed custom SSH config to connect to Metasploitable2:
Host 192.168.56.3
    Ciphers +aes128-cbc,aes256-cbc
    MACs +hmac-md5,hmac-sha1
    KexAlgorithms +diffie-hellman-group1-sha1
    PubkeyAcceptedKeyTypes +ssh-rsa

## Commands Used
1. Installation & Configuration
```bash
# Install fail2ban
sudo apt update && sudo apt install fail2ban -y

# Configure jail.local
# Copy default config to create local override
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

2. Jail Settings
Add or modify the following rules inside `/etc/fail2ban/jail.local`:
```
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
findtime = 600
```
![Fail2ban Status Verification](../screenshots/Day19-fail2ban-status.png)

3. Service Management & Attack Simulation
```
bash
# Restart and enable service
sudo systemctl restart fail2ban
sudo systemctl enable fail2ban

# Execute attack from Kali Linux machine
hydra -l msfadmin -P rockyou.txt ssh://192.168.56.3
```
![Hydra Attack Simulation](../screenshots/Day19-hydra-attack.png)

4. Verification & Status Check
```
bash
# Check overall fail2ban status
sudo fail2ban-client status

# Check specific sshd jail status and see banned IPs
sudo fail2ban-client status sshd

# View active kernel firewall rules blocking attacker
sudo iptables -L -n -v
```
![Fail2ban Status Verification](../screenshots/Day19-fail2ban-status.png)

5. Defensive Admin Commands (Unbanning)
```
bash
# Unban a specific IP from sshd jail
sudo fail2ban-client set sshd unbanip 192.168.56.103
# Unban IP from all active jails
sudo fail2ban-client set sshd unbanip 192.168.56.103
```
![Netfilter/Iptables Ban Verification](../screenshots/Day19-iptables-rules.png)

##  MITRE ATT&CK Mapping
- Tactic: Credential Access (TA0006)
- Technique: Brute Force (T1110)
- Sub-technique: Password Guessing (T1110.001)
- Mitigation: Account Use Policies (M1036) via fail2ban connection limits.

## 📸 Lab Evidence & Screenshots
!/screenshots/Day19-hydra-attack.png
!/screenshots/Day19-fail2ban-status.png
!/screenshots/Day19-iptables-rules.png
```


  
```
