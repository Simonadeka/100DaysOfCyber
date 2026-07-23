# Kali Linux Blue Team SIEM: Graylog Threat Detection Lab

Most people use Kali Linux strictly for offensive testing. This repository flips the script. This project details how to upgrade a Kali Linux instance into a defensive posture, configuring it to generate rich telemetry and routing those logs into a centralized **Graylog SIEM** instance for real-time detection, parsing, and visualization.

## 🚀 Features

- ⚡ **Live Sudo & Auth Tracking:** Extracted privilege escalation events and target execution tracking.
- ⚡ **Real-Time SSH Monitoring:** Immediate visibility into authentication success, failures, and source vectors.
- ⚡ **Service State Auditing:** Timestamps and source mapping for critical system service alterations.
- ⚡ **Live Attack Heatmap:** Integrated MaxMind GeoIP processing to map brute-force origins visually.
- ⚡ **Actionable Dashboard UI:** Pre-built widgets transitioning raw syslog strings into structured metrics.

---

## 🛠️ Architecture Overview

```text
[ Kali Linux Endpoint ] 
       │ (rsyslog / Filebeat)
       ▼
[ Graylog SIEM Input ] ──► [ Pipeline Processing ] ──► [ MaxMind GeoIP Lookup ]
                                   │
                                   ▼
                    [ Actionable Threat Dashboard ]
```

---

## 📂 Repository Structure

* `/dashboards/` - JSON exports of the Graylog dashboard widgets and layout.
* `/pipelines/` - Graylog pipeline rules for advanced logic (e.g., failed login counters).
* `/extractors/` - Custom Grok patterns for parsing Kali `/var/log/auth.log` and syslog strings.
* `/config/` - Sample OS configuration files (`rsyslog.conf` or `filebeat.yml`).

---

## ⚙️ Quick Start Deployment

### 1. Configure the Kali Endpoint (Log Shipper)
Ensure your Kali instance is sending auth and system logs to your Graylog server. If using native `rsyslog`, append the following to `/etc/rsyslog.conf`:

```bash
*.* @[YOUR_GRAYLOG_IP]:5140
```
*Note: Replace with `@@` if using TCP.*

### 2. Import Grok Patterns & Extractors
Navigate to **Graylog System > Grok Patterns** and import the patterns provided in `/extractors/kali_auth_grok.txt`. These patterns explicitly isolate variables like `source_ip`, `auth_status`, and `requested_command`.

### 3. Apply the Processing Pipelines
Import the rules from `/pipelines/` into your Graylog pipeline stream to enrich incoming messages and enforce the GeoIP mapping logic.

### 4. Import the Dashboard
1. Go to **Dashboards** in your Graylog web console.
2. Click **Import Dashboard**.
3. Upload the JSON file located in `/dashboards/kali_defense_dashboard.json`.

---

## 📊 Dashboard Preview & Use Cases

- **Brute Force Detection:** Triggers distinct visual alerts when a single source IP exceeds 5 failed SSH authentication attempts within 120 seconds.
- **Geographic Mapping:** Generates a real-time coordinate heatmap identifying active scanning bots hitting exposed listener ports.

---

## 🤝 Contributing & Feedback

This is a personal home lab project built to bridge the gap between offensive tools and defensive analytics. 
- Found a bug in the Grok patterns? Submit an **Issue**.
- Optimized a pipeline rule? Open a **Pull Request**. 

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
