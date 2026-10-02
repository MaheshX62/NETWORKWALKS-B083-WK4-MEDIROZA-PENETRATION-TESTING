# NETWORKWALKS-B083-WK4-MEDIROZA-PENETRATION-TESTING

## Week 4: Mediroza General Hospital Penetration Testing

**Program:** Cybersecurity Program | NetworkWalks  
**Batch:** B083  
**Pentester:** Mahesh  
**Assessment:** Authorized black-box penetration testing  
**Target:** `https://medirozahospital.com`  
**Date:** 02 October 2026

> This project was performed in the authorized NetworkWalks educational lab. Testing was limited to the specified target. No social engineering or denial-of-service testing was performed.

## Project Objectives

- **M1: Initial Access** - web reconnaissance, patient authentication testing, authorized patient portal access, and retrieval of three assigned reports.
- **M2: Data Extraction** - password recovery and decryption of the three PDF reports.
- **M3: Critical Data Exposure** - PDF metadata analysis, legacy directory discovery, and analysis of the exposed SQL backup.
- **M4: Report** - documentation of methodology, evidence, findings, impact, and remediation.

## Tools Used

Kali Linux, Firefox, Burp Suite, cURL, John the Ripper, qpdf, ExifTool, grep, and sed.

## Attack Chain

```text
Reconnaissance
  -> Patient login discovery
  -> Authentication weakness / SQL injection
  -> Patient portal access
  -> Three protected PDF reports
  -> Password recovery with John the Ripper
  -> PDF decryption with qpdf
  -> PDF metadata analysis with ExifTool
  -> /old/ legacy directory discovery
  -> Exposed SQL database backup
  -> Staff salary and shareholder data exposure
```

## Key Findings

| ID | Finding | Risk |
|---|---|---|
| F-01 | SQL Injection Authentication Bypass | Critical |
| F-02 | Weak PDF Password Protection | High |
| F-03 | Sensitive PDF Metadata Disclosure | Medium |
| F-04 | Publicly Accessible Database Backup | Critical |
| F-05 | Employee Salary and Shareholder Data Exposure | Critical |

## Report

The complete Week 4 report is available in:

`report/NetworkWalks_WK4_Mediroza_Penetration_Testing_Report_FINAL.docx`

It contains the methodology, evidence, screenshots, findings, attack chain, impact analysis, recommendations, and evidence appendix.

## Repository Safety

For a **public GitHub repository**, upload only a sanitized/redacted version of the report. Do not upload patient PDFs, recovered passwords, raw SQL database backups, or unredacted sensitive evidence.

This repository package contains the report but does not contain patient PDFs, passwords, or the SQL database backup.

## Disclaimer

This material is for the authorized NetworkWalks educational lab only. The techniques described must only be used against systems for which explicit authorization has been provided.
