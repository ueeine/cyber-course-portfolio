CIA Triad Case Studies
Scenario A: The Hospital
Primary CIA violation: Availability. Ransomware locked the file servers and took scheduling offline, forcing emergency patients to be diverted for 36 hours.

Secondary impacts:

Confidentiality: the attackers also exfiltrated a sample of patient files before encrypting the rest, a double extortion tactic.
Integrity: once the hospital restores from backup, they need to verify nothing was altered before trusting the records again.
Attack technique: Ransomware, likely delivered through phishing or an unpatched vulnerability, combined with data exfiltration before encryption.

Preventive controls:

Phishing training and MFA on email/remote access.
Network segmentation between clinical/scheduling systems and general IT.
Regular patching and EDR to catch exploitation early.
Damage-limitation controls:

Offline backups and a tested disaster recovery plan.
DLP monitoring to catch large outbound transfers before exfiltration finishes.
A pre-written incident response plan with downtime/diversion procedures.
Scenario B: The Leaked Database
Primary CIA violation: Confidentiality. Nothing was locked or changed, the data was simply copied and posted publicly. Pure disclosure.

Secondary impacts: Integrity wasn't touched (data wasn't altered) and availability wasn't affected (systems stayed up). This one stayed contained to a single pillar.

Attack technique: Data exfiltration, likely through an exploited vulnerability like SQL injection or through stolen credentials.

Preventive controls:

Input validation and parameterized queries to close off injection attacks.
Stronger password hashing (MD5 is outdated; bcrypt or similar would limit the damage).
Least-privilege access on the database so one compromised account can't pull the whole table.
Damage-limitation controls:

Monitoring for unusual bulk queries to catch exfiltration in progress.
Storing only partial/tokenized card numbers.
Fast breach notification and forced password resets to prevent account takeovers.
Scenario C: The Defaced Municipal Site
Primary CIA violation: Integrity. The homepage content was changed without authorization.

Secondary impacts: Availability took a hit too: the site was offline for 4 hours during restoration. Confidentiality wasn't involved since no personal data was touched.

Attack technique: Web defacement, likely through a vulnerable CMS: an outdated plugin, weak admin login, or unpatched server.

Preventive controls:

Keeping the CMS and plugins patched.
MFA on admin accounts.
A web application firewall to filter common exploit attempts.
Damage-limitation controls:

File integrity monitoring to flag unauthorized changes quickly.
Tested backups, which is what got the site back up in 4 hours.
A communications plan ready for public/press response.
Scenario D: The Manipulated Invoice
Primary CIA violation: Integrity. The invoice's bank details were changed in transit after the supplier's email account was compromised.

Secondary impacts: Confidentiality: the attack only worked because the supplier's email account was already compromised. Availability wasn't affected, nothing went down, which is partly why it took two weeks to notice.

Attack technique: Business email compromise. The attacker had access to the real inbox and modified an actual invoice thread rather than sending a fake email from a lookalike domain.

Preventive controls:

MFA on email accounts on both sides.
A rule requiring phone confirmation for any change to banking details.
SPF/DKIM/DMARC to make spoofed or manipulated messages harder to pass off as legitimate.
Damage-limitation controls:

Requiring a second person to sign off on large or newly-changed payments.
A known process for requesting a bank recall as soon as fraud is suspected.
Regular check-ins with suppliers on outstanding invoices so gaps don't sit for two weeks.
