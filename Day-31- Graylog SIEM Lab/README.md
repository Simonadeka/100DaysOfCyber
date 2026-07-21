# 🔍 Graylog SIEM Lab on Kali Linux

A complete Self-Hosted Security Information and Event Management (SIEM) lab built on Kali Linux using Docker. This project demonstrates centralized log collection, parsing, and analysis for security monitoring.

## 📌 Project Overview
This lab simulates a real-world SOC environment where system logs from Kali Linux are forwarded in real-time to a Graylog server running in Docker. It provides visibility into authentication events, sudo usage, and system activity for threat detection and forensics.

## 🛠️ Tech Stack
- **OS**: Kali Linux 2025
- **Containerization**: Docker & Docker Compose
- **SIEM**: Graylog 5.2
- **Database**: MongoDB 6.0, OpenSearch 2.11
- **Log Forwarder**: rsyslog
- **Network**: NAT / Host-Only / Bridged VirtualBox networking

## ⚡ Key Features
- [x] Centralized log collection from Kali to Graylog
- [x] Real-time Syslog TCP input on port 1514
- [x] Automatic forwarding of all system, auth, and sudo logs
- [x] Web dashboard for search, filtering, and analysis
- [x] Dockerized deployment for easy setup and portability
- [x] Supports multiple network modes: NAT, Bridged, Host-Only

## 🏗️ Architecture[Kali Linux] --rsyslog--> [Docker Network] --> [Graylog:9000]
|
-->
--> [OpenSearch][MongoDB]javascript
## 🚀 Setup Instructions
### 1. Clone and Start Graylog with Docker

git clone https://github.com/Simonadeka/graylog-siem-lab
cd graylog-siem-lab
docker compose up -d2. Configure Graylog InputGo to http://localhost:9000Login: admin / adminSystem > Inputs > Launch new input > Syslog TCPPort: 1514, Bind address: 0.0.0.03. Configure Kali to Forward Logsjavascriptbash
sudo apt install rsyslog -y
echo '*.* @@<GRAYLOG_IP>:1514;RSYSLOG_SyslogProtocol23Format' | sudo tee /etc/rsyslog.d/10-graylog.conf
sudo systemctl restart rsyslog4. Testjavascriptbash
logger -n <GRAYLOG_IP> -P 1514 -T "Test log from Kali"
sudo ls /rootCheck logs in Graylog > Search📸 Screenshots!screenshots/graylog-dashboard.png
Real-time capture of sudo authentication events from Kali📚 What I LearnedDeploying enterprise SIEM tools in a containerized environmentConfiguring rsyslog for remote log forwardingTroubleshooting Docker networking: NAT vs Bridged vs Host-OnlyLog parsing, stream routing, and basic SOC monitoring workflows🔮 Future ImprovementsAdd TLS encryption for syslog trafficCreate Graylog Dashboards and Alerts for failed SSH loginsIntegrate with Wazuh for IDS/IPS correlationForward logs from Windows and other Linux VMs👤 AuthorSimon Adeka
Cybersecurity Student | SOC Analyst in Training
https://github.com/Simonadeka | https://www.linkedin.com/in/simon-adeka/📄 LicenseThis project is licensed under the MIT License.javascript
And here's the `docker-compose.yml`:
``
version: '3'
services:
  mongo:
    image: mongo:6.0
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
    volumes:
      - mongo_data:/data/db

  opensearch:
    image: opensearchproject/opensearch:2.11.1
    environment:
      - discovery.type=single-node
      - "OPENSEARCH_JAVA_OPTS=-Xms1g -Xmx1g"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - opensearch_data:/usr/share/opensearch/data

  graylog:
    image: graylog/graylog:5.2
    environment:
      - GRAYLOG_PASSWORD_SECRET=somepasswordpepper
      - GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918 # admin
      - GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000/
    depends_on:
      - mongo
      - opensearch
    ports:
      - "9000:9000"
      - "1514:1514/tcp"
    volumes:
      - graylog_data:/usr/share/graylog/data

volumes:
  mongo_data:
  opensearch_data:
  graylog_data:
