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
