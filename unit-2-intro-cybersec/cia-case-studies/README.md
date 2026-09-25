Scenario A- Hospital Ransomware
Primary: availability.
Secondary:Confidentiality data exfiltrated before encryption and minor integrity concerns.
Technique:Ransomware likely via phishing training, patching/MFA on remote access and
network segmentation.
Limit damage : immutable offline backups and manual fall back procedures.
Scenario B- Leaked database
Primary: Confidentiality
Secondary: minimal integrity and availability on impact data copied not altered or deleted
Technique: Data exfiltration, likely via SQL injection or exposed database.
Prevent: strong hashing regular pentesting least privilege and data minimization
Limit damage:Anomaly monitoring on bulk queries fast breach response and password
rests.
Scenario C- Defaced Municipial Site
Primary:Integritry
Secondary:Availability no confidentiality impact
Technique: WEb defacement via unpatched Cms or weak admin credentials
PRevent: Patching MFA on all admin accounts and WAF
Limit damage: tested backups and file integrity and monitoring.
Scenario D- Manipulated invoice
Primary: Integrity
Secondary: Confidentiality as the suppliers mailbox is compromised no availability impact
Technique:Business email compromised and invoice fraud
Prevent: out of brand verification bank changes dual approval for payments
Limit damage: fast bank fraud reporting cyber insurance and coordinated reset with
supplier.
