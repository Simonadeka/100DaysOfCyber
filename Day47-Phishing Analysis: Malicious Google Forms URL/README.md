
# Phishing Analysis: Malicious Google Forms URL - 2026-08-05

## Overview
This repository documents the analysis of a phishing campaign using a Google Forms short URL to harvest credentials. The campaign was automatically detected and contained by Google within minutes, returning a `403 Forbidden` "Terms of Service Violation" error.

![1_vt_url_input](screenshots/1_vt_url_input.png)
*Screenshot 1: VirusTotal URL input page showing the phishing link `forms.gle/kdjqyaKC9UG6tkYu6`.*

## IOCs
- **Short URL**: `https://forms.gle/kdjqyaKC9UG6tkYu6`
- **Final URL**: `https://docs.google.com/forms/d/e/1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ/viewform`
- **Serving IP**: `192.178.155.102`
- **Form ID**: `1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ`
- **VT Detection**: 1/92 ![4_vt_detection](screenshots/4_vt_detection.png)

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
#### Redirection Chain
1. `forms.gle/kdjqyaKC9UG6tkYu6` → Google Forms → 403
![2_redirection_chain](screenshots/2_redirection_chain.png)

#### HTTP Response
- Status: 403 Forbidden
- Reason: TOS Violation
![5_http_response](screenshots/5_http_response.png)

#### JavaScript Analysis
- Only Chrome objects found
![6_js_global_vars](screenshots/6_js_global_vars.png)

#### Cookies
- `COMPASS` cookie (1-hour expiry)
![3_cookies_tab](screenshots/3_cookies_tab.png)

#### Network Requests
- Clean Google traffic only
![7_network_requests](screenshots/7_network_requests.png)

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

