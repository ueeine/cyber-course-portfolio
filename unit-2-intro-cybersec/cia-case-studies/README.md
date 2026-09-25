# CIA Triad Case Studies

## Scenario A – Hospital Ransomware
- **Primary:** Availability
- **Secondary:** Confidentiality (data exfiltrated before encryption) and minor integrity concerns
- **Technique:** Ransomware, likely via phishing
- **Prevent:** Security awareness/phishing training, patching and MFA on remote access, network segmentation
- **Limit damage:** Immutable offline backups and manual fallback procedures

## Scenario B – Leaked Database
- **Primary:** Confidentiality
- **Secondary:** Minimal integrity and availability impact — data was copied, not altered or deleted
- **Technique:** Data exfiltration, likely via SQL injection or an exposed database
- **Prevent:** Strong hashing, regular pentesting, least privilege, data minimization
- **Limit damage:** Anomaly monitoring on bulk queries, fast breach response, password resets

## Scenario C – Defaced Municipal Site
- **Primary:** Integrity
- **Secondary:** Availability — no confidentiality impact
- **Technique:** Web defacement via unpatched CMS or weak admin credentials
- **Prevent:** Patching, MFA on all admin accounts, WAF (Web Application Firewall)
- **Limit damage:** Tested backups, file integrity monitoring

## Scenario D – Manipulated Invoice
- **Primary:** Integrity
- **Secondary:** Confidentiality — the supplier's mailbox was compromised; no availability impact
- **Technique:** Business email compromise (BEC) and invoice fraud
- **Prevent:** Out-of-band verification for bank changes, dual approval for payments
- **Limit damage:** Fast bank fraud reporting, cyber insurance, coordinated reset with supplier
