Network Profile — Windows laptop (WLAN adapter, RZ616 Wi-Fi 6E)

Note: captured while connected to the Varia school network, not my home network — this explains the 10.18.x.x addressing and the DNA ISP infrastructure visible in the traceroute.

Part 1 — Identity

Q1. IPv4 address: 10.18.172.182 — MAC address: E8-65-38-21-7F-xx (masked last group for the repo).

Q2. A private IP address only works inside a local network and isn't routable on the public internet — every home network can reuse the same private ranges (192.168.x.x, 10.x.x.x, 172.16–31.x.x) without conflict. A public IP is globally unique and routable on the internet. My router uses private addresses internally so it can hand out addresses to all my devices without needing a public IP for each one — it uses NAT to translate all of them through its single public IP when they reach the internet.

Q3. My IP address identifies where I am on a network (Layer 3, the Network layer) and can change — it's assigned by DHCP and is different depending on which network I connect to. My MAC address identifies the physical network interface itself (Layer 2, the Data Link layer) and is burned into the hardware by the manufacturer, so it stays the same regardless of what network I'm on (though it can technically be spoofed in software).

Q4. My subnet mask is 255.255.255.0 = /24.

Total addresses in a /24: 2^8 = 256
Usable for devices: 256 − 2 (network + broadcast) = 254
My IP is 10.18.172.182/24 → network address: 10.18.172.0, broadcast address: 10.18.172.255
Part 2 — Gateway and reachability

Q5. Default gateway: 10.18.172.62. Yes, it's on the same subnet as my machine (10.18.172.182/24) — both addresses fall inside the 10.18.172.0–10.18.172.255 range defined by the /24 mask, so my machine can reach it directly without needing to be routed anywhere first.

Q6. Average round-trip time to my gateway (10.18.172.62) was 3ms, versus 29ms to 1.1.1.1. The gateway is one hop away on my own local network, so the packet barely has to travel. 1.1.1.1 has to leave my network, pass through my ISP, and cross the internet to reach Cloudflare's server, which adds distance and hops — hence the higher latency.

Q7. ping -n 4 example.com resolved to 104.20.23.154 and returned an average of 27ms, close to the 1.1.1.1 result since it's also reaching out to the internet. It worked using a name instead of an IP because of DNS (Domain Name System) — it translated example.com into an IP address before the ping was sent.

Part 3 — DNS

Q8. Configured DNS server: 10.18.172.62 (IPv4) and 2001:14bb:c4:4600::9f (IPv6). This is my gateway/router, not a public resolver — the router is likely forwarding these requests upstream to my ISP's or a public DNS server.

Q9. nslookup example.com returned four addresses: 104.20.23.154, 172.66.147.243, 2606:4700:10::6814:179a, and 2606:4700:10::ac42:93f3 (two IPv4, two IPv6 — example.com is behind Cloudflare). I also looked up two sites I actually use:

discord.com → five IPv4 addresses (162.159.138.232, 162.159.137.232, 162.159.128.233, 162.159.135.232, 162.159.136.232)
github.com → a single address (140.82.121.4)

Discord and example.com both return multiple IPs because they run behind a CDN/load balancer — spreading traffic across several servers so no single machine gets overwhelmed, and so the site stays up even if one server or data center goes down. GitHub returning just one address suggests a simpler setup, though it likely still has failover infrastructure behind that single IP.

Q10. Even with HTTPS encrypting the content of your traffic, DNS queries are typically sent in cleartext — so someone watching your network traffic could see every domain name you look up, even though they can't see what you actually did on those sites. That alone reveals a lot: which services you use, what you're researching, what time you're active, even sensitive things like health or legal sites you visited — just from the list of names being resolved.

Part 4 — Path to the internet

Q11. It took 8 hops to reach example.com (104.20.23.154). The first hop was 10.18.172.62 — my own default gateway, exactly as expected since every packet leaving my machine has to go through the router first.

Q12. In my trace, hop 5 showed Request timed out for all three probes, and hop 2 had one *. No, this doesn't necessarily mean the connection is broken. It usually means that router is configured to not respond to the ICMP/UDP probes traceroute sends (a common security/performance practice), while it still forwards the actual traffic just fine. I know the path itself wasn't broken because the trace kept going and successfully reached 104.20.23.154 at hop 8.

Part 5 — Listening ports

Q13–14. From Get-NetTCPConnection -State Listen:

Port	Interface	Localhost-only or network-facing	Common use
135	0.0.0.0 / ::	Network-facing	RPC Endpoint Mapper
139	10.18.172.182	Network-facing	NetBIOS Session Service (legacy file/printer sharing)
445	::	Network-facing	SMB — Windows file/printer sharing
5040	0.0.0.0	Network-facing	Connected Devices Platform (Windows)
49664–49670	0.0.0.0 / ::	Network-facing	Dynamic/ephemeral RPC ports
6463	127.0.0.1	Localhost-only	Discord IPC (rich presence)
49635	127.0.0.1	Localhost-only	Local app (ephemeral)
42050	::1	Localhost-only	Local app (ephemeral)

Ports 135/139/445 matter a lot here — this is the classic Windows SMB/RPC file-sharing trio, and it's what worms like WannaCry and NotPetya used to spread across networks via EternalBlue. Localhost-only ports (127.0.0.1/::1) can only be reached by processes on your own machine, so they're not a network attack surface at all. Ports bound to 0.0.0.0 or a specific network IP can be reached by anything else on your LAN (or the internet, if you're not behind NAT) — which is exactly what a port scanner run by someone else would find open.

Q15. My machine is exposing the standard Windows networking stack (135/139/445) plus a Windows service (5040) on all interfaces — nothing exotic, no random third-party servers listening on the network side. Since this is the school network rather than my own home network behind NAT, that's actually a bit more worth paying attention to — other devices on the same school network segment could potentially reach those ports, unlike on a home network where NAT keeps them isolated from the internet at large.

Part 6 — Network Profile
Identity
IPv4 address: 10.18.172.182
Subnet mask / CIDR: 255.255.255.0 (/24)
MAC address: E8-65-38-21-7F-xx
Network address: 10.18.172.0
Broadcast address: 10.18.172.255
Gateway and reachability
Default gateway: 10.18.172.62
Ping to gateway (avg): 3 ms
Ping to 1.1.1.1 (avg): 29 ms
DNS
Configured DNS server(s): 10.18.172.62, 2001:14bb:c4:4600::9f
example.com resolves to: 104.20.23.154, 172.66.147.243, 2606:4700:10::6814:179a, 2606:4700:10::ac42:93f3
Path to the internet
Hops to example.com: 8
First hop: 10.18.172.62
Listening ports
Port	Protocol	Interface (localhost / all)	Common use
135	TCP	all	RPC Endpoint Mapper
139	TCP	all (specific IP)	NetBIOS Session Service
445	TCP	all	SMB file sharing
5040	TCP	all	Connected Devices Platform
6463	TCP	localhost	Discord IPC
Reflection (150–200 words)

What surprised me was how much was already listening on my machine without me setting anything up — ports 135, 139, and 445 (the Windows SMB/RPC stack) were exposed on all interfaces by default, even though I never turned on file sharing. Those are the same ports worms like WannaCry exploited, so it's a reminder that default settings aren't the same as secure settings.

If I had to close or investigate one port, it'd be 445 — no reason it needs to be reachable beyond my own machine unless I'm actively sharing files.

I'll probably use ipconfig /all and Get-NetTCPConnection most often, since together they answer "what am I on this network" and "what's exposed" almost instantly.
