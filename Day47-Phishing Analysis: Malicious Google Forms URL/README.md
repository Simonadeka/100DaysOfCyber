Here's the full GitHub repo structure + files for your phishing analysis:

```
phishing-analysis-google-forms-2026-08-05/
├── README.md
├── reports/
│ └── incident_report_2026-08-05.md
├── iocs/
│ └── iocs.txt
├── screenshots/
│ ├── 1_vt_url_input.png
│ ├── 2_redirection_chain.png
│ ├── 3_cookies_tab.png
│ ├── 4_vt_detection.png
│ ├── 5_http_response.png
│ ├── 6_js_global_vars.png
│ └── 7_network_requests.png
└── detections/
    └── sigma_google_forms_phishing.yml
```

### 1. README.md
```
# Phishing Analysis: Malicious Google Forms URL - 2026-08-05

## Overview
This repository documents the analysis of a phishing campaign using a Google Forms short URL to harvest credentials. The campaign was automatically detected and contained by Google within minutes, returning a `403 Forbidden` "Terms of Service Violation" error.

## IOCs
- **Short URL**: `https://forms.gle/kdjqyaKC9UG6tkYu6`
- **Final URL**: `https://docs.google.com/forms/d/e/1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ/viewform`
- **Serving IP**: `192.178.155.102`
- **Form ID**: `1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ`

## Analysis Summary
- **Redirection**: `forms.gle` → Google Forms (now 403)
- **Network**: 7 requests, all to Google domains (clean)
- **Cookies**: 3 legit Google cookies, short-lived
- **Verdict**: Confirmed Phishing - Contained by Google

See `/reports/incident_report_2026-08-05.md` for full details.
```

### 2. reports/incident_report_2026-08-05.md
```
# Incident Report: Google Forms Phishing Campaign
## Date: 2026-08-05
## Analyst: Cybersecurity Analyst | Jos, Nigeria
### Executive Summary
A phishing URL `https://forms.gle/kdjqyaKC9UG6tkYu6` was submitted to VirusTotal. The link redirected to a fake Google Form designed to harvest credentials. Google detected the abuse and returned a **403 TOS violation**, wiping the form data.

### Technical Analysis
- **Redirection**: `forms.gle` → Google Forms (now 403)
- **HTTP Response**: 403 TOS Violation
- **JavaScript**: No malicious variables, form data wiped
- **Network**: Only Google domains contacted

### IOCs
See `/iocs/iocs.txt`

### Recommendations
- Block `forms.gle/kdjqyaKC9UG6tkYu6`
- Alert on `forms.gle` redirects with 403
- User awareness: Google Forms never ask for passwords via external links
```

### 3. iocs/iocs.txt
```
# IOCs - Google Forms Phishing 2026-08-05
URL: https://forms.gle/kdjqyaKC9UG6tkYu6
URL: https://docs.google.com/forms/d/e/1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ/viewform
IP: 192.178.155.102
Form ID: 1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ
```

### 4. detections/sigma_google_forms_phishing.yml
```
title: Suspicious Google Forms Phishing Short URL
status: experimental
description: Detects access to known malicious Google Forms short URLs
author: You
date: 2026/08/05
logsource:
    category: web
    product: proxy
detection:
    selection:
        c-uri|contains:
            - 'forms.gle/'
            - 'docs.google.com/forms/d/e/'
    condition: selection
fields:
    - c-uri
    - c-useragent
    - src_ip
falsepositives:
    - Legitimate Google Forms usage
level: medium
tags:
    - attack.phishing
    - attack.t1566.002
```

### 5. Screenshots Folder
Place your 7 screenshots in this folder with names matching the README:
- `1_vt_url_input.png`
- `2_redirection_chain.png`
- `3_cookies_tab.png`
- `4_vt_detection.png`
- `5_http_response.png`
- `6_js_global_vars.png`
- `7_network_requests.png`

This repo structure is ready for GitHub upload. Want me to:
A. *Generate a commit message* for you?
B. *Write a LinkedIn post* showcasing this analysis?
C. *Move to another lab* (e.g., `dns_tunnel.pcapng`)?
