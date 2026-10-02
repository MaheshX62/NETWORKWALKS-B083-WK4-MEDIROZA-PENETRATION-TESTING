# 🔐 NETWORKWALKS-B083-WK4-MEDIROZA-PENETRATION-TESTING

## 📌 Week 04 Cybersecurity Internship Report

This repository contains my **Week 04 practical work and final penetration-testing report** completed as part of the **Cybersecurity Program at NetworkWalks**.

The work covers the four milestones of the Mediroza General Hospital penetration-testing project:

- **M1:** Initial Access
- **M2:** Data Extraction
- **M3:** Critical Data Exposure
- **M4:** Penetration Testing Report

> **Scope:** All activities were performed within the authorized NetworkWalks educational lab scope against the assigned Mediroza General Hospital target. No social engineering or denial-of-service testing was performed.

---

## 🎯 Objectives

The main objectives of this week's practical work were to:

1. Conduct reconnaissance against the assigned web application.
2. Identify exposed application entry points.
3. Analyze the patient authentication mechanism.
4. Test input handling and demonstrate the authentication weakness.
5. Obtain authorized access to the restricted patient portal.
6. Retrieve the three assigned encrypted patient reports.
7. Recover the PDF passwords using John the Ripper.
8. Decrypt the three reports using qpdf.
9. Analyze PDF metadata using ExifTool.
10. Identify the exposed legacy `/old/` directory.
11. Retrieve and analyze the accessible SQL database backup.
12. Identify employee salary and shareholder information exposed in the backup.
13. Document the attack chain, findings, risks, evidence, and remediation recommendations.

---

## 🌐 Target

```text
https://medirozahospital.com
```

### Assessment Type

```text
Black-box penetration testing & vulnerability assessment
```

### Authorization

```text
Written authorization provided for the controlled educational lab
```

### Testing Scope

```text
Target domain only
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Kali Linux** | Controlled penetration-testing environment |
| **Burp Suite** | Web application request interception and testing |
| **Firefox** | Web application testing and evidence collection |
| **cURL** | Web reconnaissance and HTTP request testing |
| **John the Ripper** | PDF password recovery |
| **qpdf** | PDF decryption |
| **ExifTool** | PDF metadata analysis |
| **grep / sed** | Inspection and analysis of the exposed SQL backup |

---

# 🔎 M1: Initial Access

## Reconnaissance

The assessment began with web reconnaissance to identify exposed application entry points and understand the target's web application structure.

### Target

```text
https://medirozahospital.com
```

### Reconnaissance

```bash
curl https://medirozahospital.com/robots.txt
```

The reconnaissance identified application paths including:

```text
/patient/
/staff/
/old/
```

The `/patient/` path was used to locate the patient login interface.

---

## Patient Portal Authentication

The patient portal login was tested using Burp Suite.

The authentication mechanism was found to be vulnerable to unsafe handling of user-controlled input.

### Authentication Bypass

The authorized lab accepted the crafted SQL input:

```text
admin' --
```

This resulted in access to the restricted patient portal.

### M1 Result

The patient portal displayed **three encrypted pathology reports**, which were retrieved as the required M1 deliverable.

---

# 🔐 M2: Data Extraction

The three retrieved reports were password-protected PDF files.

### Password Recovery

John the Ripper was used with the Kali Linux wordlist to recover the passwords protecting the three reports.

### Decryption

The recovered passwords were supplied to `qpdf` to create decrypted copies.

### M2 Result

| File | Password Recovery | Decryption Result |
|---|---|---|
| `patient_report_1.pdf` | Recovered | Successful |
| `patient_report_2.pdf` | Recovered | Successful |
| `patient_report_3.pdf` | Recovered | Successful |

The final report contains the terminal evidence showing successful password recovery and PDF decryption.

> Plaintext passwords are intentionally not reproduced in this README.

---

# 🕵️ M3: Critical Data Exposure

## PDF Metadata Analysis

The third decrypted report was analyzed using ExifTool.

The metadata contained an internal operational comment referencing a legacy database backup:

```text
DB backup moved to /old before site migration, do not delete
```

This provided a direct lead to the exposed legacy directory.

---

## 📂 Exposed Legacy Directory

The `/old/` directory was accessible through the web server and exposed:

```text
mediroza_db_backup_2019.sql
```

The SQL backup was retrieved and analyzed as text without importing it into a database server.

---

## 💰 Employee Salary Exposure

The exposed SQL backup contained a `staff` table with employee information including a:

```text
monthly_salary_zar
```

field.

The analysis demonstrated that salary records were directly present in the publicly accessible database backup.

The final report includes the salary evidence with sensitive contact and identification fields redacted.

---

## 📊 Shareholder Data Exposure

The same SQL backup contained a `shareholders` table containing:

- Shareholder names
- Ownership percentages
- Shares held
- Share classes

The report documents the exposed shareholder records as part of the M3 findings.

---

# 📊 Findings & Observations

| ID | Finding | Risk |
|---|---|---|
| **F-01** | SQL Injection Authentication Bypass | **Critical** |
| **F-02** | Weak PDF Password Protection | **High** |
| **F-03** | Sensitive PDF Metadata Disclosure | **Medium** |
| **F-04** | Publicly Accessible Database Backup | **Critical** |
| **F-05** | Employee Salary and Shareholder Data Exposure | **Critical** |

---

# 🔗 Attack Chain

The Week 04 practical demonstrated the following attack chain:

```text
Reconnaissance
      ↓
Patient Login Discovery
      ↓
SQL Injection Authentication Bypass
      ↓
Patient Portal Access
      ↓
Three Encrypted Patient Reports
      ↓
Password Recovery
      ↓
PDF Decryption
      ↓
PDF Metadata Analysis
      ↓
/old/ Legacy Directory
      ↓
Exposed SQL Database Backup
      ↓
Employee Salary & Shareholder Data Exposure
```

---

# ⚠️ Security Recommendations

The assessment identified several remediation areas:

### 1. Prevent SQL Injection

Use parameterized queries and prepared statements for all database operations.

### 2. Strengthen Authentication

Implement robust server-side authentication and validate all authentication inputs.

### 3. Strengthen PDF Protection

Use strong, randomly generated passwords for encrypted documents and avoid predictable password patterns.

### 4. Remove Sensitive Metadata

Remove unnecessary internal metadata and operational comments from externally accessible documents.

### 5. Protect Database Backups

Remove database backups and legacy files from public web directories and store them outside the web root.

### 6. Secure Backup Storage

Apply strong access controls and encryption at rest to backup files.

### 7. Review Legacy Directories

Regularly review legacy directories and public file listings for accidental information exposure.

### 8. Apply Least Privilege

Restrict access to sensitive employee and corporate information according to business requirements.

---

# 📁 Repository Structure

```text
NETWORKWALKS-B083-WK4-MEDIROZA-PENETRATION-TESTING/
│
├── README.md
│
├── report/
│   └── NetworkWalks_WK4_Mediroza_Penetration_Testing_Report_FINAL.docx
│
└── evidence/
    └── README.md
```

---

# 📸 Evidence

The final report contains evidence collected throughout the Week 04 practical.

### M1 Evidence

- Reconnaissance evidence
- Patient portal authentication response
- SQL injection authentication-bypass evidence
- Successful patient portal access
- Three encrypted patient reports displayed in the portal

### M2 Evidence

- John the Ripper password-recovery evidence
- qpdf PDF-decryption evidence
- Confirmation of the three decrypted PDF files

### M3 Evidence

- ExifTool metadata showing the `/old/` database-backup clue
- `/old/` directory listing
- Exposed SQL database backup
- Employee salary evidence
- Shareholder evidence

---

# 📄 Final Report

The complete Week 04 penetration-testing report is available in:

```text
report/NetworkWalks_WK4_Mediroza_Penetration_Testing_Report_FINAL.docx
```

The report contains:

- Executive Summary
- Scope and Methodology
- Tools Used
- M1 Initial Access
- M2 Data Extraction
- M3 Critical Data Exposure
- Findings and Proof of Exploitation
- Risk Analysis
- Recommendations
- Conclusion
- Evidence Documentation
- Evidence Appendix

---

## 👤 Author

**Mahesh**

Cybersecurity Intern  
**NetworkWalks – Batch B083**

### Program

**Cybersecurity Program at NetworkWalks | Week 04**

---

## ⚖️ Disclaimer

This repository documents educational cybersecurity activities performed within an authorized lab environment.

Do not use the techniques, commands, or tools documented here against systems, applications, or networks without appropriate authorization.
