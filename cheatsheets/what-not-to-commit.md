# What NOT to Commit

This is a critical reference guide for keeping my portfolio and any real work safe.

## Never commit:

**Passwords, API keys, tokens, or other credentials** — even temporarily, even "just for testing". Once pushed, assume the secret is compromised. Rotate it immediately.

**Personal data of other people** (names, emails, photos with faces) — including classmates, colleagues, or anyone recognizable. This includes OSINT investigation data that could identify real individuals.

**Real-world targets** (IP addresses, hostnames, employee info) from environments you don't own. Only lab networks with RFC1918 private addresses are acceptable.

**Screenshots that contain credentials, session tokens, or personal information**. Crop or redact before uploading.

**Anything from OSINT investigations on fictional personas that could be used to identify a real person**. Keep the boundary clear.

## Be careful with:

**Lab VM IP addresses** — acceptable only if they're RFC1918 private addresses (10.x.x.x, 172.16–31.x.x, 192.168.x.x) on your own network.

**Tool outputs that include your username, hostname, or paths** revealing where you live or work. Redact these before committing.

**Wireshark captures (.pcap files)** — these contain far more data than people realize. Review carefully; consider removing sensitive traffic.

**Your full MAC address and full public IP** on devices you own — if the repo is public, mask the last group.

## If you accidentally commit a secret:

1. **Treat it as compromised immediately**. Rotate or revoke it right away.
2. **Deleting it from the next commit is not enough** — the secret stays in Git history.
3. **Tell the instructor** and work to remove the secret from history using `git filter-repo` or BFG Repo-Cleaner. The instructor can help.

This habit of thinking before committing is what separates professionals from people who leak data by accident.