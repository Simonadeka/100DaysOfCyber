# Phishing Analysis: Malicious Google Forms URL - 

Incident Report & Analysis: Phishing Campaign Leveraging Google Forms and URL Shorteners
Date of Analysis: August 5, 2026  
Analyst: Cybersecurity Analyst | Abuja, Nigeria  
Classification: TLP: WHITE - For Defensive Purposes

1. Executive Summary
On August 5, 2026 at 13:13 UTC, a malicious phishing URL was identified using Google's own forms.gle URL shortener to redirect victims to a credential harvesting Google Form. The campaign was automatically detected and contained by Google within minutes, returning a 403 Forbidden "Terms of Service Violation" error.

Key Finding: No data exfiltration occurred. All network traffic was limited to legitimate Google infrastructure.  
Severity: Medium  
Status: CONTAINED

This report details the analysis methodology, technical findings, MITRE ATT&CK mapping, and recommendations.

2. Analysis Technique & Methodology
The investigation followed the standard SOC Tier 1 workflow: `Detection > Triage > Analysis > Containment > Reporting`.

Tools Used
- VirusTotal: For URL reputation, redirection chain, and community verdict
- Google Safe Browsing: For live URL status check
- Browser DevTools / VT Network Tab: For HTTP transaction analysis
- AbuseIPDB, http://urlscan.io: For IP and domain enrichment

Analysis Phases
1.  Initial Triage: The URL `https://forms.gle/kdjqyaKC9UG6tkYu6` was submitted to VirusTotal. Immediate flag: `1/92` vendors, specifically `SafeToOpen: Phishing
2.  Redirection Chain Analysis: Examined the 302 redirect to identify the final landing page and avoid interacting with it directly.
3.  HTTP & Network Analysis: Reviewed HTTP status codes and all outbound network requests to identify C2 communication or data exfiltration.
4.  HTML & Content Analysis: Inspected HTML metadata to determine the lure/pretext used by the attacker.
5.  Enrichment: Correlated IP `192.178.155.102` and Form ID against threat intel sources.

### 3. Technical Findings

#### 3.1 Indicators of Compromise - IOCs
URL Short: https://forms.gle/kdjqyaKC9UG6tkYu6
URL Final: https://docs.google.com/forms/d/e/1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ/viewform
IP:        192.178.155.102
Form ID:   1FAIpQLSd8ILZvEeC3LjkS909-jI2mDZAcccmNvkoXZTBP4xKZdYHQ
Favicon dhash: e89e931193338ee8
#### 3.2 Redirection & HTTP Analysis

**Step** | **URL** | **Status** | **Detections**
1 | `https://forms.gle/kdjqyaKC9UG6tkYu6` | 403 | 1/92 Phishing
2 | `https://docs.google.com/forms/d/e/...` | 403 | -

Finding: The attacker used a legitimate Google service to host the phishing page, increasing trust and bypassing basic email filters. Google responded with 403, indicating TOS enforcement and takedown.

## 3.3 Network Requests Analysis
All 7 HTTP requests resolved to legitimate Google domains:
- fonts.googleapis.com
- ssl.gstatic.com
- fonts.gstatic.com

Finding: No external POST requests, no data exfiltration endpoints, and no malicious JavaScript were observed. This indicates Google contained the form before it could be used at scale.

### 3.4 HTML & Content Analysis
- Page Title: Error
- Meta Description: Generic "Web word processing, presentations and spreadsheets"
- Trackers: None found

Finding: The original phishing content was removed by Google. The generic metadata suggests this was likely a credential harvesting form impersonating a job application, survey, or account verification.

#### 3.5 MITRE ATT&CK Mapping
Tactic | Technique ID | Technique Name | Description
Initial Access | T1566.002 | Phishing: Spearphishing via Service | Use of Google Forms to target victims
Defense Evasion | T1027 | Obfuscated Files or Information | Use of `forms.gle` shortener to hide final URL
##4. Timeline of Events
Timestamp UTC | Event
2026-08-05 13:13:10 | URL first submitted to VirusTotal
2026-08-05 13:13:10 | Google blocks URL with 403 TOS Violation
### 5. Impact Assessment
Impact: Low. No evidence of successful credential compromise from this specific IOC.  
Likelihood: High that similar forms are being used in other campaigns.  
Google's automated takedown was effective.

### 6. Recommendations

#### For Organizations / Defenders
1.  Detection: Implement SIEM rule to alert on access to `forms.gle/_` followed by `docs.google.com/forms/d/e/_ with 403 responses.
2.  Blocking: Add the IOCs above to web proxies, DNS filters, and email gateways.
3.  Hunting: Search email and proxy logs for any clicks to the IOC between `2026-08-01` and `2026-08-05 13:13 UTC`. Force password resets for affected users.
4.  Awareness Training: Educate users that attackers abuse legitimate services like Google Forms, Microsoft Forms, and DocuSign. Always verify the sender.

#### For Individuals
1.  Do not submit personal information, passwords, or OTPs on forms received via SMS, WhatsApp, or email.
2.  Verify the URL. A real Google Form should not be sent from unknown numbers.
3.  Report suspicious links to `reportphishing@google.com`

#### For Google
Continue aggressive takedown of Forms used for phishing. Consider adding warning banners for new Forms shared externally.

### 7. Conclusion
This case highlights a growing trend: attackers abusing trusted cloud services to host phishing. The use of `forms.gle` is particularly effective for evasion. However, Google's rapid automated response demonstrates the value of platform-level security controls.

The analysis confirms that proactive threat hunting and user education remain critical defenses against service-abuse phishing.

Author Note: This report was produced as part of a personal threat intelligence lab. All IOCs are provided for defensive use only.
