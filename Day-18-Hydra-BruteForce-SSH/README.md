# 🔒 Day 18: Brute-forced my own hardened SSH with Hydra... and failed

### Goal
Validate SSH hardening from Day 17 by testing it against a real brute-force attack with Hydra.

### Lab Environment
- **Attacker:** Kali Linux 
- **Target:** Metasploitable 2 - `192.168.56.3`
- **Tool:** THC-Hydra + `rockyou.txt`
- **Hardening Applied:** Key-only SSH auth, `PasswordAuthentication no`

### Steps Taken
1.  **Launch Attack:** 
    `hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.10 -t 4 -v`
2.  **Monitor Logs on Target:**
    `tail -f /var/log/auth.log`
3.  **Observe Result:** All login attempts were rejected immediately.

### Result
**Attack Failed** - Hydra could not get in.  
**Defense Worked** - Server rejected all password authentication attempts.

### Key Takeaways
- **Key-only authentication** removes the #1 attack vector for SSH.
- **Disabling `PasswordAuthentication`** in `sshd_config` is the fastest win for SSH hardening.
- **Hydra is great for testing**, but useless against properly hardened services.
- **Log monitoring** lets Blue Team detect and respond to brute-force attempts in real-time.

### Proof
![Hydra Failed Against Hardened SSH](./../screenshots/day18-hydra-failed.png)
*Screenshot: Hydra output showing all attempts failed against key-only SSH*

### TL;DR
Hardening works! Blocking password logins = blocking brute-force attacks.

### Next Step
Find another service to break or harden 💪

---
#100DaysOfCyber #Day18 #BlueTeam #SSH #Hardening #Hydra #CyberDefense
