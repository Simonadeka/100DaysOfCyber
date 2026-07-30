# 100 Days Of Cybersecurity 🚀

My journey building real-world SOC/Blue Team skills through hands-on labs, tools, and write-ups.

## 📚 **Challenge Overview**
- **Goal**: Level up in DFIR, Threat Hunting, and Packet Analysis.
- **Tools**: Kali Linux, Wireshark, tshark, tcpdump.
- **Format**: 1 lab/day → Report + LinkedIn post + GitHub write-up.

## 🔍 **Labs & Write-Ups**

| Day | Topic | Tools Used | Screenshot | Write-Up | LinkedIn Post |
|-----|-------|------------|------------|----------|--------------|
| 1 | SMTP Traffic Forensics | Wireshark, tshark | ![SMTP Flow](images/smtp-flow.png) | [Report](reports/day1-smtp-report.md) | [Post](https://www.linkedin.com/your-post-url) |

## 🛠️ **Tools & Commands Cheat Sheet**
- `tshark -r file.pcap -Y "smtp"`: Filter SMTP traffic
- `tshark -z follow,tcp,ascii,0`: Reconstruct TCP stream
- `grep -Ei '^(From|To|Subject)'`: Extract email headers

## 📸 **Screenshots**
![SMTP Reconstruction](images/smtp-flow.png)
*Day 1: Reconstructed SMTP Conversation*

## 📈 **Progress Tracker**
- [x] Day 1: SMTP Forensics
- [ ] Day 2: DNS Tunneling
- [ ] Day 3: HTTP Creds in PCAP

## 💬 **Connect With Me**
- [LinkedIn](https://www.linkedin.com/in/yourprofile/)
- [Twitter](https://twitter.com/yourhandle)

## 🔖 **Resources I Use**
- [TCPDump & Wireshark Cookbook](https://www.wireshark.org/docs/)
- [Kali Linux Docs](https://www.kali.org/docs/)
- [SANS DFIR](https://www.sans.org/cfwi/)

## 📝 **License**
MIT License - Do whatever you want with these notes 😄
