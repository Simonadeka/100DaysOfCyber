# Day35 Wireshark HTTP Traffic Capture Lab

## Objective
Capture and analyze HTTP traffic to understand plaintext data transmission risks.

## Tools Used
- Wireshark
- Apache2
- Kali Linux
- curl

## Lab Steps
### 1. Setup Apache Server
![Apache Service Running](IMAGE_LINK_1)
* Apache2 serving `basic.html` on `http://127.0.0.1`

### 2. Capture Traffic
![Wireshark Capture](IMAGE_LINK_2)
* Capturing loopback interface (`lo`)

### 3. Analyze Handshake + HTTP
![Handshake and HTTP](IMAGE_LINK_3)
* TCP 3-way handshake + HTTP GET/200 OK

### 4. Follow TCP Stream
![TCP Stream Evidence](IMAGE_LINK_4)
* Plaintext data: `Name: [REDACTED]` and `Reg No: [REDACTED]`

### 5. Webpage Served
![Webpage Served](IMAGE_LINK_5)
* `http://127.0.0.1/basic.html` in browser

### 6. Apache Status
![Apache Status](IMAGE_LINK_1) (same as 1)
* Server active during capture

### 7. Hashes
![SHA256 Hashes](IMAGE_LINK_6)
* `sha256sum evidence/basic.pcapng working/basic_working.pcapng`

## Key Takeaway
HTTP traffic is unencrypted. Use HTTPS/TLS for secure data transmission.

## Commands Used
```bash
# Start capture
tshark -i lo -w basic.pcapng

# Host webpage
echo "<html><body>Name: Simon Friday Adeka<br>Reg No: C11/26/DFIT/17300</body></html>" > /var/www/html/basic.html

# Generate traffic
curl http://127.0.0.1/basic.html

# Verify
tshark -r basic.pcapng -z follow,tcp,ascii,0
sha256sum evidence/basic.pcapng working/basic_working.pcapng
