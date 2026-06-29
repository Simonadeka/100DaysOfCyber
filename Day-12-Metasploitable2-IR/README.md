# Day 12: Metasploitable2 - Blue Team IR Lab

**Date:** Sept 4, 2025  
**Role Focus:** SOC Analyst / DFIR | Blue Team Mindset

### **Objective**
Compromised Metasploitable2 to map post-exploit artifacts, not just to exploit.

### **What I Found**
- **Timeline:** `last` → user login history for `msfadmin` 
- **Recon:** `history` → `find /home/msfadmin/vulnerable` used by attacker
- **Artifacts:** `/home/msfadmin/vulnerable/` folder with CTF flags

### **Blue Team Detections**
| TTP | SIEM Alert Logic |
| --- | --- |
| Priv Escalation | `uid=0` for non-root users |
| Discovery | `find /home -type f` command execution |
| Vuln Host | Hostname with `Kernel 2.6.24` |

### **Key Lesson**
Attack to defend. You need the attacker’s view to write good detection rules.

**#100DaysOfCyber #BlueTeam #SOC #DFIR #Metasploitable2**
