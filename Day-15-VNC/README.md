# Day 15: VNC Exploit → Blue Team Fix
**Metasploitable2 | #100DaysOfCyber | Abuja, NG 🇳🇬**

### The Hack 😳
Got ROOT access with just 1 word: `password` 
No bruteforce. No exploits. 

**What I did:**
1. `nmap -sV -p 5900 192.168.56.3` 
   → Found `5900/tcp open vnc VNC protocol 3.3`
2. `vncviewer 192.168.56.3::a5900`
3. Password: `password` 
   → Full root GUI: `root's X desktop (metasploitable:0)`

### Blue Team Audit
**Red Team did this:** T1213 VNC Exploitation + T1110.003 Default Password

**Blue Team findings:**
1. `netstat -tulpn | grep 5900` 
   → `0.0.0.0:5900 LISTEN` = Exposed to entire LAN. Bad.
2. `find / -name passwd -path "*/.vnc/*"` 
   → `/root/.vnc/passwd` exists = T1552.001 Unsecured Credentials
3. `cat /var/log/auth.log | grep vnc` 
   → No output = T1070.004 SOC blind. No VNC logging.

### The Fix
1. **Never use defaults**: Change VNC password immediately
2. **Bind to localhost only**: `vncserver -localhost :0`
3. **Harden file perms**: `chmod 600 /root/.vnc/passwd` 
4. **Firewall it**: Don't expose 5900 to LAN. Use SSH tunnel/VPN instead.

### Evidence
![Day 15 VNC Root Access](../screenshots/Day17-VNC.png)
*Caption: Authenticated as root via VNC with password: password*

---
**Day 15 of #100DaysOfCyber** 
Documenting real labs → Blue Team vs Red Team. 
Follow along if you're breaking into cybersecurity too 👊
