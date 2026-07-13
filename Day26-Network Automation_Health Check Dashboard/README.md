#  Day 26 Network Automation - Health Check Dashboard
Part of my 100 Days of Cybersecurity challenge

## 🎯 Objective
Build a bash script that automates daily health checks across all VMs in my cyber lab.  
The goal: Go from 15 minutes of manual checks to a 10-second automated HTML dashboard.

## 🖥️ Lab Environment
| Host | IP | Role |
| --- | --- | --- |
| Ubuntu Server | 192.168.56.101 | Target Server |
| Kali Linux | 192.168.56.103 | Attacker / Automation Host |
| Windows 10 | 192.168.56.104 | Target |
| Metasploitable 2 | 192.168.56.3 | Vulnerable Target |

## ⚙️ What the Script Does
1.  **Ping Check**: Verifies if each host is UP or DOWN
2.  **Port Check**: Checks if SSH port 22 is OPEN using `nc`
3.  **HTML Report**: Generates `health_report.html` with color-coded status

## 🚀 How to Run

chmod +x health_dashboard.sh
./health_dashboard.sh
firefox health_report.html

![ Network Automation](../screenshots/Day26_dashboard_screenshot)

# 1. Make script executable
chmod +x health_dashboard.sh

# 2. Run the health check
./health_dashboard.sh

# 3. View the dashboard
firefox health_report.html📸 Dashboard Preview!dashboard_screenshot.png
Green = UP / OPEN | Red = DOWN / CLOSED🧠 Key LearningBash scripting for real SOC automation tasksNetwork monitoring with ping and netcat ncGenerating dynamic HTML reports directly from shellThinking like an analyst: Automate repetitive work to save timeTime saved: 15 min manual work → 10 seconds automated📂 Fileshealth_dashboard.sh - The automation scripthealth_report.html - Generated dashboard⏭️ Next StepsAdd email alerts when a host goes DOWNAdd CPU/RAM/Disk checksSchedule with cron🛠️ Tech UsedBash, Linux, ping, netcat, HTML, cron#30DaysOfCybersecurity #SOC #Automationjavascript
### **2. `health_dashboard.sh` (no changes)**

#!/bin/bash
DATE=$(date "+%Y-%m-%d %H:%M:%S")
REPORT="health_report.html"

echo "<h1>Cyber Lab Health Dashboard - $DATE</h1>" > $REPORT
echo "<table border=1><tr><th>Host</th><th>IP</th><th>Status</th><th>SSH Port 22</th></tr>" >> $REPORT

check_host() {
  IP=$1
  NAME=$2
  if ping -c 1 -W 1 $IP > /dev/null 2>&1; then STATUS="UP 🟢"; else STATUS="DOWN 🔴"; fi
  if nc -zv -w1 $IP 22 > /dev/null 2>&1; then SSH="OPEN"; else SSH="CLOSED"; fi
  echo "<tr><td>$NAME</td><td>$IP</td><td>$STATUS</td><td>$SSH</td></tr>" >> $REPORT
}

check_host 192.168.56.101 "Ubuntu"
check_host 192.168.56.103 "Kali"
check_host 192.168.56.104 "Windows"
check_host 192.168.56.3 "Metasploitable"

echo "</table>" >> $REPORT
echo "Report generated: $REPORT"
