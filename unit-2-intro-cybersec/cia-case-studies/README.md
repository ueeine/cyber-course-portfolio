CIA Triad Case Studies

Scenario A: The Hospital
Primary CIA violation: Availability. Ransomware lock up the file servers and take scheduling offline so emergency patients was got diverted for 36 hours.

Secondary impacts:

Confidentiality: the attackers also stealed some of patient files before they was encrypting the rest, this is call double extortion.
Integrity: when hospital restoring from backup them need to check nothing are altered before they trusting the records again.

Attack technique: Ransomware, probably it come in from phishing or unpatched vulnerability, and also data exfiltration is happen before encryption.

Preventive controls:

Phishing training and MFA for email and remote access.
Network segmentation between clinical and scheduling systems and the general IT stuff.
Regular patching and EDR so exploitation are catched early.

Damage-limitation controls:

Offline backups and disaster recovery plan what is actually tested.
DLP monitoring for catch big outbound transfers before exfiltration is finishing.
Incident response plan that is wrote before, with downtime and diversion procedures.

Scenario B: The Leaked Database
Primary CIA violation: Confidentiality. Nothing are locked or changing, data just get copied and posted on public. Pure disclosure.

Secondary impacts: Integrity it wasnt touch (data didnt got altered) and availability wasnt affect (systems is stay up). This one stayed in only one pillar.

Attack technique: Data exfiltration, likely by a exploited vulnerability like SQL injection or by stolen credentials is used.

Preventive controls:

Input validation and parameterized queries for stopping the injection attacks.
More better password hashing (MD5 are outdated, bcrypt or similar thing would limiting the damage).
Least-privilege access on database so one compromised account cant pull whole table.

Damage-limitation controls:

Monitoring for strange bulk queries for catching exfiltration when its happen.
Store only partial or tokenized card numbers.
Fast breach notification and forced password resets so account takeovers dont happening.

Scenario C: The Defaced Municipal Site
Primary CIA violation: Integrity. Homepage content are changed without no authorization.

Secondary impacts: Availability got hit also: site was offline 4 hours during it restoring. Confidentiality it wasnt involved because no personal data is touched.

Attack technique: Web defacement, likely because of vulnerable CMS: a outdated plugin, weak admin login, or server that unpatched.

Preventive controls:

Keeping CMS and plugins is patched.
MFA at admin accounts.
Web application firewall for filtering the common exploit attempts.

Damage-limitation controls:

File integrity monitoring for flag unauthorized changes more quick.
Tested backups, that is what make the site come back in 4 hours.
Communications plan be ready for public and press response.

Scenario D: The Manipulated Invoice
Primary CIA violation: Integrity. Invoice bank details was change in transit after supplier email account are got compromised.

Secondary impacts: Confidentiality: attack only work because supplier email account was compromised already. Availability wasnt affect, nothing go down, thats part of why it taking two weeks for noticing.

Attack technique: Business email compromise. Attacker have access to the real inbox and modify a actual invoice thread, instead of sending fake email from lookalike domain.

Preventive controls:

MFA on email accounts at both sides.
Rule that is requiring phone confirmation for any change of banking details.
SPF/DKIM/DMARC so spoofed or manipulated messages is more harder to pass like legit.

Damage-limitation controls:

Requiring second person for sign off large or newly-changed payments.
A known process for ask bank recall soon as fraud are suspected.
Regular check-ins with suppliers about outstanding invoices so gaps dont sitting for two weeks.
