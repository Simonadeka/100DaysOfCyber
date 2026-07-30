Day 37: SMTP Traffic Forensics in Kali VM

## 📚 **Challenge Overview**
- **Objective**: Analyze SMTP traffic in a pcap, extract email headers, and identify security risks.
- **Tools**: Kali Linux, Wireshark, tshark, grep.
- **Challenge**: Reconstruct SMTP conversation, verify auth methods, and check for plaintext creds.

## 🔍 **What I Did**
1. Verified pcap integrity with `sha256sum` and `capinfos`.
2. Filtered SMTP traffic using `tshark -Y "smtp"`.
3. Rebuilt TCP stream with `tshark -z follow,tcp,ascii,0`.
4. Extracted headers with `grep -Ei '^(From|To|Subject)'`.

## 📈 **Findings & Results**
- **SMTP Handshake**: `220 → EHLO → 250-AUTH LOGIN PLAIN → STARTTLS advertised`
- **Auth Method**: LOGIN (Base64 encoded)
- **Security Risk**: STARTTLS optional → creds sent in plaintext if sniffed.
- **Headers**: Extracted sender, recipient, subject, and client info.

## 🛠️ **Commands Used**
- `tshark -r smtp.pcap -Y "smtp"`
- `tshark -z follow,tcp,ascii,0`
- `grep -Ei '^(From|To|Subject)' smtp.txt`

## 📸 **Screenshots**
![SMTP Stream](images/day37-smtp-stream.png)
*Reconstructed SMTP Conversation*

## 💡 **Takeaway**
Most email leaks happen because SMTP sends creds in Base64 and `STARTTLS` isn’t enforced. If you can capture it, you can read it.

## 🔗 **Links**
- [LinkedIn Post](https://www.linkedin.com/your-post-url)
- [Report](reports/day37-smtp-report.md)

## 📝 **Next Up**
- Day 38: DNS Tunneling Analysis
- Want me to dissect DNS, HTTP creds, or SMB?

#CyberSecurity #DFIR #ThreatHunting #Wireshark #KaliLinux
