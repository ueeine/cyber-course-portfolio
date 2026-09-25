# NotPetya Case Study

1. **Timeline and spread:** NotPetya struck on June 27, 2017, starting inside Ukraine through a single tax software provider before spreading outward. Its initial infections were geographically concentrated in Ukrainian businesses, then it spread globally through networks with Ukrainian operations. Even though it was aimed at one country, it caused damage worldwide.

2. **Impact:** Maersk was among the hardest hit, 17 of its shipping terminals went down within minutes, and 45,000 workstations and 4,000 servers had to be rebuilt over roughly ten days. Merck, Mondelez, and FedEx's TNT division were also hit. Total global damage is estimated around $10 billion, making NotPetya the costliest cyberattack in history at the time.

3. **Intent:** Availability was the real target, but through permanent destruction rather than temporary lockout. NotPetya displayed a ransom note, but it was actually a wiper, the decryption key never worked, so paying achieved nothing. This matters because it shows the intent wasn't extortion; it was designed to cause maximum destruction.

4. **Root cause:** Attackers compromised the update servers of M.E.Doc, a tax software that businesses operating in Ukraine were essentially required to use. When users installed a routine, legitimate-looking update, they installed NotPetya alongside it.

5. **Propagation:** Once inside, NotPetya used EternalBlue to infect any still-unpatched machines. It also used a modified Mimikatz to harvest admin credentials straight from a computer's memory, then used built-in Windows tools to log into other machines with the stolen credentials.

6. **What could have helped:** Credential and network segmentation would have meaningfully limited the damage, isolating domain admin credentials and separating network segments so stolen credentials in one area couldn't be reused everywhere. Maersk's own post-mortem found its network was insufficiently segmented and that some patching was inadequate, which is what let the infection spread.

7. **Broader lesson:** NotPetya was later attributed to Russian military hackers targeting Ukraine, yet it caused massive damage in countries with no stake in that conflict. This shows how hard cyber operations are to contain once launched, malware spreads everywhere, and geopolitical attacks can harm the global economy.
