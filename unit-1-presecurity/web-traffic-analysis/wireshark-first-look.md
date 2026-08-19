Wireshark Lab — Cleartext vs Encrypted Traffic

Network traffic analysis comparing an HTTP login (cleartext) against an HTTPS login (encrypted) using Wireshark.

Course: IT, Varia Vantaa Tool: Wireshark Captures: U1-03a_http_login.pcap, U1-03a_https_login.pcap

Table of Contents
Part A — HTTP Capture
Part B — HTTPS Capture
Part C — Making Sense of It
Reflection
Part A — HTTP Capture (U1-03a_http_login.pcap)
1. Credentials sent in the login submission

The username was anna.virtanen and the password was Summer2026!. The following line is visible in the reconstructed HTTP stream:

text
username=anna.virtanen&password=Summer2026!&remember=on
2. HTTP method used for the login form

The login form was submitted with the POST method. The request line immediately before the credentials is:

http
POST /login HTTP/1.1
3. SESSIONID cookie and its risk

The SESSIONID value was a3f9c2e7b81d4f60a5e2c9d10f4b7e88.

http
Set-Cookie: SESSIONID=a3f9c2e7b81d4f60a5e2c9d10f4b7e88; Path=/; HttpOnly

Risk: An attacker who obtains this session cookie may be able to impersonate the logged-in user by replaying it in a request — known as session hijacking. They could gain access to the account without ever needing the password.

4. Sensitive information exposed on the dashboard

Two sensitive details visible in the final HTTP response are the user's role, Finance Administrator, and email address, anna.virtanen@pohjola-logistics.local. The dashboard also discloses the user's full name and last-login IP address.

html
<h1>Welcome back, Anna Virtanen</h1>
<p>Role: Finance Administrator</p>
<p>Email: anna.virtanen@pohjola-logistics.local</p>
<p>Last login from 10.10.10.50</p>
Part B — HTTPS Capture (U1-03a_https_login.pcap)
5. Visibility of the username and password

No — the username and password could not be found in the HTTPS capture. After the TLS handshake, the HTTP login request and its contents are encrypted, so an eavesdropper who only captures the network traffic cannot read the credentials without the relevant TLS decryption keys.

6. Server name visible in the Client Hello

The Server Name Indication (SNI) hostname in the TLS Client Hello is lab-portal.local.

7. Information still visible to an eavesdropper

Even without decrypting the traffic, an eavesdropper can still observe metadata such as:

Visible metadata	Example value
Client IP address	10.10.10.50
Server IP address	10.10.10.10
Destination port	443
Packet timing	Observable
Packet sizes	Observable

For example, the capture shows that the client connected to the server over HTTPS, even though it does not reveal the contents of the login request.

Part C — Making Sense of It
8. Why the protocol choice matters for confidentiality

HTTP transmits credentials, cookies, and page contents in readable cleartext, whereas HTTPS encrypts this application data in transit and prevents a passive network observer from reading it.

9. Example of traffic on an untrusted network

When using public Wi-Fi in a café, I may send traffic over a network that other people could monitor. HTTPS protects the contents of my web sessions, such as passwords and messages, but some metadata can still be exposed, including the IP addresses involved, the timing and size of traffic, and often the site name through SNI or DNS.



What surprised me most was that the entire HTTP login — including the password, session cookie, and dashboard information — could be read directly from the stream. HTTPS did not make the connection invisible, because the IP addresses, port, timing, packet sizes, and server name were still observable, but it prevented the sensitive application contents from being read.
