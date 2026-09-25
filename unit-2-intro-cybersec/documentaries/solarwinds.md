# SolarWinds Case Study

1. **Overview:** SolarWinds was a cyber espionage incident that occurred during the fallout of the election between Donald Trump and Joe Biden. Multiple Fortune 500 companies and government organizations were compromised, causing conflict between two major nations (the United States and Russia).

2. **Impact:** The espionage campaign used trojanized Orion updates that went out to roughly 18,000 organizations, though only a small number were actually deep-dive targets of the hackers. Confirmed victims included FireEye, whose breach exposed the whole campaign, as well as multiple US federal agencies. Consequences went beyond stolen data — companies had to assume any Orion-monitored host was compromised and rebuild trust in their supply chains from scratch, causing reputational and financial damage. Geopolitically, CISA issued Emergency Directive 21-01 ordering federal agencies to take immediate action, and the UK and US attributed the attacks to Russia's foreign intelligence service (SVR), leading to 10 Russian officials being expelled and sanctions being imposed.

3. **CIA Triad impact:** The leg of the CIA triad most directly attacked was **confidentiality**, since this was an espionage operation aimed at gaining access to read emails, documents, and internal communications over a long period of time. **Integrity** was also compromised at a foundational level, since the attackers altered a trusted, digitally signed build without authorization — a textbook integrity violation.

4. **Root cause:** The SolarWinds incident was a **supply chain attack**, where a trusted third-party vendor was attacked rather than the victim directly, and the compromised update was then distributed to every customer at once, like a normal update. This was far more dangerous due to its scale — it reached far more victims than targeting one specific victim would have — and due to its stealth, since devices didn't flag the software because it came from a trusted vendor.

5. **Detection:** It was not caught by SolarWinds or by any automated defense — it was only caught because FireEye noticed its own network had been breached and, while investigating, traced the intrusion back to the Orion update. This shows that many major attacks are discovered indirectly, through a downstream victim doing incident response on an unrelated alert, rather than through the vendor's own monitoring.

6. **What could have helped:** Code-signing integrity verification combined with build-system segmentation — closely monitoring the software pipeline itself. The attackers got in by compromising the build environment; if that environment had been treated as a high-value target with its own strict access controls, the attack could have been caught before signing and distribution.

7. **Broader lesson:** Trust itself is an attack surface. Before SolarWinds, many companies assumed that software from a reputable, properly signed vendor could be trusted. SolarWinds showed that vendors themselves are targets, and that the trust placed in them can be exploited.
   
8. **my personal takeaway:**
the only way to secure yourself by closing all doors leaving door that u think nobody will come from it is stupid way to get hacked even if u think nobody will find that door even if its hidden also i know that nobody can 100% secure himself but try atleast to avoid malicious things and think twice before u do something stupid
