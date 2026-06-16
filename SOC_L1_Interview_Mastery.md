# SOC L1 Interview — Mastery Guide

**How to use this:** Each domain has four parts — (1) the *60-second answer* you give if asked to "explain X", (2) *what you must know cold*, (3) *likely interview Q&A with model answers*, (4) the *SOC L1 detection angle* (event IDs, Sysmon, triage). At the end: a scenario bank, a rapid-fire round, an event-ID cheat sheet, and a final checklist.

Read it once for coverage, then drill the Q&A out loud. Interviewers care less about textbook definitions and more about whether you can reason through an alert. The scenario bank is where you win or lose the interview.

---

# 1. Active Directory

### 60-second answer
Active Directory is Microsoft's directory service for managing identities and resources in a Windows domain. It stores objects (users, computers, groups) in a hierarchical database on Domain Controllers, and handles authentication and authorization centrally. Logical structure top-down: **forest → domain(s) → OUs → objects**. Authentication is done with **Kerberos** (default) or **NTLM** (legacy/fallback). Policy is pushed via **Group Policy (GPOs)**.

### Know cold
- **Forest**: top-level security boundary. Shares a schema and global catalog.
- **Domain**: administrative/replication boundary. Has one or more **Domain Controllers (DCs)**.
- **OU (Organizational Unit)**: container for grouping objects and applying GPOs. Not a security boundary.
- **Global Catalog (GC)**: partial replica of all objects in the forest — used for cross-domain lookups and UPN logons.
- **Schema**: definition of every object class and attribute. Forest-wide.
- **FSMO roles** (5, single-master operations): Schema Master, Domain Naming Master (forest-wide); RID Master, PDC Emulator, Infrastructure Master (domain-wide). PDC Emulator is the one you mention most — time sync source, password change priority, account lockout processing.
- **SYSVOL**: shared folder on DCs holding GPOs and logon scripts, replicated between DCs.
- **LDAP**: the protocol used to query/modify AD (port 389, 636 for LDAPS).
- **Trusts**: allow authentication across domains/forests (one-way/two-way, transitive/non-transitive).
- **Tombstone / AD Recycle Bin**: deleted object handling.

### Likely Q&A
**Q: What's the difference between a forest and a domain?**
A domain is an admin and replication boundary with its own DCs and accounts; a forest is the *security* boundary containing one or more domains that trust each other and share a schema and global catalog. If you compromise a DC you own the domain; the forest is the true trust boundary.

**Q: What are FSMO roles and why do they matter to a SOC?**
Five specialized DC roles that can't be multi-master. For a SOC, the **PDC Emulator** matters most — it's the authoritative time source (Kerberos breaks if clocks skew >5 min) and processes account lockouts, so auth anomalies often trace back to it.

**Q: What's the Global Catalog used for?**
Forest-wide searches and UPN-based logons. It holds a partial attribute set for every object in the forest, so a user logging in with user@domain can be resolved without contacting every domain.

### SOC L1 angle
You don't administer AD, but you read its telemetry constantly. Know that **all authentication events live on the DCs** (4768/4769/4771 for Kerberos, 4776 for NTLM). LDAP reconnaissance (BloodHound, `Get-ADUser` sweeps) shows up as heavy 4662 / directory-service access or as ATT&CK T1087 (Account Discovery). SYSVOL is a common place attackers find passwords (Group Policy Preferences `cpassword`).

---

# 2. Active Directory Security (attack paths)

### 60-second answer
AD security is about protecting identities and the trust relationships that let one credential unlock the whole domain. The major attack classes are **credential theft and reuse** (Pass-the-Hash, Pass-the-Ticket), **Kerberos abuse** (Kerberoasting, AS-REP roasting, Golden/Silver tickets), **replication abuse** (DCSync), and **privilege escalation via misconfiguration** (delegation, ACLs, nested group membership). Tools like **BloodHound** map the shortest path from a low-priv user to Domain Admin.

### Know each attack cold (this is the highest-yield section for a SOC L1 interview)

**Kerberoasting** — Any authenticated user can request a service ticket (TGS) for any account with an SPN. The ticket is encrypted with the *service account's password hash*, so the attacker requests it, extracts it, and cracks it offline. Targets service accounts with weak passwords. *Detection:* spike in **4769** (TGS requests), especially with **RC4 encryption (0x17)** instead of AES, from a single user to many SPNs.

**AS-REP Roasting** — Targets accounts with "Do not require Kerberos preauthentication" set. The attacker requests an AS-REP and cracks it offline. *Detection:* **4768** with preauth not required.

**Pass-the-Hash (PtH)** — NTLM doesn't need the plaintext password, just the hash. Steal the NTLM hash (from LSASS/SAM) and authenticate as that user. *Detection:* **4624 Logon Type 3 + NTLM** from unexpected hosts; lateral patterns; LSASS access (Sysmon **EID 10** targeting lsass.exe).

**Pass-the-Ticket (PtT)** — Steal a Kerberos TGT/TGS from memory and reuse it. *Detection:* tickets used from a host that didn't request them; anomalous 4769/logons.

**Overpass-the-Hash (Pass-the-Key)** — Use a stolen NTLM hash to request a *Kerberos* TGT, blending PtH with Kerberos. *Detection:* RC4 TGT requests (4768) right after suspicious LSASS access.

**DCSync** — Attacker with replication rights (`Replicating Directory Changes`) asks a DC to replicate account data, pulling password hashes for *any* user including KRBTGT — without ever logging into the DC. *Detection:* **4662** with the DS-Replication-Get-Changes GUID from a non-DC account, or replication (`DRSUAPI`) requests from a host that isn't a DC.

**DCShadow** — Attacker registers a rogue DC and pushes malicious changes via replication. Harder to detect; watch for unexpected `nTDSDSA` object creation and new SPNs on a workstation.

**Golden Ticket** — Forged TGT signed with the stolen **KRBTGT** hash. Grants arbitrary access, long validity, survives password resets of the impersonated user. *Detection:* TGTs with anomalous lifetimes, logons with no preceding 4768, mismatch between account and privileges. Remediation requires **resetting KRBTGT twice**.

**Silver Ticket** — Forged *TGS* using a service account's hash. Scoped to one service, never touches the DC (stealthier than Golden). *Detection:* service access (4624/4634 on the member server) with no corresponding 4769 on the DC.

**Delegation abuse** — *Unconstrained* delegation lets a server impersonate any user who authenticates to it (TGT stored in memory — huge risk). *Constrained* and *resource-based constrained delegation (RBCD)* can be abused if an attacker controls the right object. *Detection:* changes to `msDS-AllowedToDelegateTo` / `msDS-AllowedToActOnBehalfOfOtherIdentity`.

**ACL abuse** — GenericAll/WriteDACL/WriteOwner on a privileged object lets an attacker grant themselves rights or reset passwords. This is what BloodHound paths often exploit. *Detection:* 4670 (permissions changed), 5136 (directory object modified), 4724/4738 (password reset/account change).

### Likely Q&A
**Q: Walk me through Kerberoasting.**
Any domain user can request a service ticket for any SPN-enabled account. That ticket is encrypted with the service account's password hash. The attacker requests it (legit Kerberos behavior, so no failed logons), exports it, and brute-forces the password offline — no further AD interaction needed. Mitigations: long random passwords / gMSA for service accounts, enforce AES, monitor 4769 for RC4 spikes.

**Q: Golden vs Silver ticket?**
Golden = forged **TGT** with the KRBTGT hash → access to anything, processed by the DC. Silver = forged **TGS** with a single service account's hash → access to one service, never contacts the DC, so stealthier. Golden requires the KRBTGT hash (full domain compromise already); Silver only needs one service account.

**Q: What is DCSync and why is it dangerous?**
It abuses the directory replication protocol. An account with replication rights asks a DC to "replicate" account data and receives password hashes — including KRBTGT, which enables Golden Tickets. It's dangerous because it never requires logging into a DC and mimics normal DC-to-DC traffic. Detect via 4662 with the replication GUID from a non-DC principal.

### SOC L1 angle
This is the domain interviewers probe hardest. For each attack be ready to say **(1) what it abuses, (2) the one detection signal, (3) one mitigation**. Memorize the 4769-RC4 → Kerberoasting, 4662-replication-GUID → DCSync, Sysmon-EID-10-lsass → credential dumping mappings. BloodHound = attack-path mapping (T1087 / LDAP recon on the wire).

---

# 3. Authentication & Identity Security

### 60-second answer
Authentication proves *who you are*; identity security is about protecting that proof and the privileges attached to it. In Windows: **Kerberos** (ticket-based, default in a domain) and **NTLM** (challenge-response, legacy). Modern identity adds **MFA**, **SSO**, and federation protocols (**SAML, OAuth 2.0, OIDC**). Defense centers on least privilege, tiered admin, and protecting credential material in memory.

### Kerberos flow (know this step by step)
1. **AS-REQ / AS-REP** — Client authenticates to the **KDC** (on the DC) using a timestamp encrypted with its password hash (pre-auth), receives a **TGT** (encrypted with KRBTGT hash) + session key.
2. **TGS-REQ / TGS-REP** — Client presents the TGT to request a service ticket (**TGS**) for a specific service (by SPN).
3. **AP-REQ / AP-REP** — Client presents the TGS to the target service, which validates it and grants access.
Key point: the service decrypts the TGS with *its own* hash, the DC isn't contacted again — which is exactly why Silver Tickets and Kerberoasting work.

### NTLM (challenge-response)
Server sends a challenge (nonce), client encrypts it with its NT hash, server (or DC via NETLOGON) validates. No timestamp, no mutual auth, vulnerable to relay and Pass-the-Hash. Logged as **4776** on the validating DC.

### Know cold
- **Logon types** (you read these daily): **2** interactive, **3** network (SMB, PtH), **4** batch, **5** service, **7** unlock, **8** network cleartext, **9** new-credentials (runas /netonly), **10** RemoteInteractive (RDP), **11** cached.
- **SSO / Federation**: **SAML** (XML, enterprise web SSO), **OAuth 2.0** (authorization/delegated access, not authN by itself), **OIDC** (authentication layer on top of OAuth 2.0, issues ID tokens).
- **MFA factors**: something you know / have / are. Push, TOTP, FIDO2/hardware keys. **MFA fatigue / push bombing** is a current attacker technique.
- **Protections**: **Credential Guard** (isolates LSASS secrets with virtualization), **Protected Users** group, **LAPS** (randomizes local admin passwords), **tiered admin model** (Tier 0 = DCs/identity, Tier 1 = servers, Tier 2 = workstations — no credential crossing tiers).

### Likely Q&A
**Q: Kerberos vs NTLM?**
Kerberos is ticket-based, uses mutual authentication and timestamps, scales well, and is default in AD. NTLM is an older challenge-response protocol with no mutual auth and no timestamp, kept for backward compatibility. NTLM is more exposed to relay and Pass-the-Hash, so a sudden rise in NTLM auth (4776) where Kerberos is expected is a red flag.

**Q: What's the difference between OAuth and OIDC?**
OAuth 2.0 is an *authorization* framework — it grants an app delegated access to resources. OIDC is an *authentication* layer built on OAuth that adds an ID token, so it actually tells you *who* the user is. Using raw OAuth as if it were authentication is a classic mistake.

**Q: A user authenticates with logon type 9 — what does that tell you?**
New-credentials logon — `runas /netonly` style. The process runs locally as the current user but uses different credentials for network connections. It's commonly seen with tools like Mimikatz/PsExec and lateral movement, so worth investigating if unexpected.

### SOC L1 angle
Logon types are your bread and butter. **Type 3 + NTLM from a workstation to many hosts** = lateral movement / PtH. **Type 10** = RDP — correlate source IPs. Impossible-travel and MFA-fatigue alerts come from your identity provider (Entra ID / Okta) logs. Know that a TGT request with **RC4** when the environment is AES-only is suspicious.

---

# 4. DHCP / DNS Security

### 60-second answer
DHCP hands out IP configuration; DNS resolves names to IPs. Both are high-value to attackers because they sit in the path of nearly all traffic. DHCP attacks: **starvation** (exhaust the pool) and **rogue DHCP** (hand out a malicious gateway/DNS for MITM). DNS attacks: **tunneling/exfiltration**, **cache poisoning**, **DGA-based C2**, and **fast flux**. DNS is the single most useful protocol for *detecting* compromise because malware almost always resolves a domain.

### Know cold
**DHCP**
- *Starvation*: attacker floods DISCOVER with spoofed MACs, exhausts the pool, denying legitimate clients.
- *Rogue DHCP*: attacker's server answers first, sets itself as gateway/DNS → man-in-the-middle.
- *Mitigations*: **DHCP snooping** (switch trusts only authorized DHCP ports), port security.

**DNS**
- *Tunneling / exfiltration*: encode data in subdomains/TXT records to a domain the attacker controls (e.g., `<base64data>.evil.com`). Used for C2 and data theft. *Signal:* abnormally long/high-entropy subdomains, high query volume to one domain, lots of TXT/NULL queries.
- *DGA (Domain Generation Algorithm)*: malware generates many pseudo-random domains to find a live C2. *Signal:* bursts of **NXDOMAIN** responses, high-entropy domain names.
- *Cache poisoning / spoofing*: injecting forged records so victims resolve to attacker IPs. **DNSSEC** mitigates by signing responses.
- *Fast flux*: rapidly rotating IPs behind one domain (low TTL) to keep C2 resilient.
- *DoH/DoT*: DNS over HTTPS/TLS encrypts queries — good for privacy, bad for visibility; can bypass your DNS logging.

### Likely Q&A
**Q: How would you detect DNS tunneling?**
Look for anomalies in query characteristics: unusually long subdomains, high entropy (random-looking), a high volume of queries to a single second-level domain, unusual record types (TXT, NULL), and consistent beaconing intervals. A workstation making thousands of DNS queries to one domain it's never resolved before is the classic pattern.

**Q: What's a rogue DHCP server and how do you stop it?**
An unauthorized DHCP server that answers client requests, typically to set a malicious default gateway or DNS for a man-in-the-middle. You stop it with **DHCP snooping**, which configures the switch to accept DHCP offers only from trusted (authorized) ports.

**Q: A host is generating hundreds of NXDOMAIN responses per minute. What's your hypothesis?**
Likely **DGA malware** cycling through generated domains to locate its live C2 server, or a misconfiguration. I'd pivot on the source host, check the domain entropy, look for a successful resolution after the NXDOMAIN burst (the live C2), and check process telemetry on the host.

### SOC L1 angle
**Sysmon Event ID 22 = DNS query** — your primary host-level DNS visibility. In Wazuh you'd alert on long/high-entropy domains, NXDOMAIN spikes, and queries to newly-registered or known-bad domains (threat intel/CDB list matching). DNS is often where you *first* see a compromise that endpoint AV missed.

---

# 5. Malware & PowerShell Threats

### 60-second answer
Malware is any code built to harm or gain unauthorized access — viruses, worms, trojans, ransomware, rootkits, spyware. Modern attackers favor **fileless** techniques: living off the land with built-in tools (**LOLBins**) like PowerShell, WMI, `rundll32`, `regsvr32`, `mshta`. PowerShell is the attacker's favorite because it's signed, native, and powerful — used for download cradles, in-memory execution, and AMSI bypass. Detection has shifted from file hashes to **behavior**: process trees, command-line arguments, and script-block logging.

### Know cold
**Malware types**: virus (needs a host file), worm (self-propagates), trojan (disguised), ransomware (encrypts for extortion), rootkit (hides at OS/kernel level), RAT (remote access), spyware/keylogger, dropper/loader (stages the real payload).

**Fileless / LOLBins**: execution that lives in memory or abuses trusted binaries so there's no malicious file on disk. Classic LOLBins: `powershell.exe`, `wmic.exe`, `rundll32.exe`, `regsvr32.exe` (**Squiblydoo** — fetching a remote scriptlet), `mshta.exe`, `certutil.exe` (download/decode), `bitsadmin.exe`.

**PowerShell attack patterns**:
- **Encoded command**: `powershell -enc <base64>` — hides intent. Decode it during triage.
- **Download cradle**: `IEX (New-Object Net.WebClient).DownloadString('http://...')` — pulls and runs code in memory.
- **Hidden / no-profile flags**: `-nop -w hidden -ep bypass`.
- **AMSI bypass**: disabling the Antimalware Scan Interface so malicious scripts aren't scanned.
- **Obfuscation**: string concatenation, format operators, base64, to evade signatures.
- Frameworks: **Cobalt Strike**, **Empire**, **Metasploit/Meterpreter**.

### Likely Q&A
**Q: What's fileless malware and why is it hard to detect?**
Malware that executes in memory or through trusted system tools without writing a malicious file to disk, so hash- and signature-based AV has nothing to scan. You detect it behaviorally — suspicious parent/child process relationships (e.g., Word spawning PowerShell), command-line arguments, and script-block logging — rather than by file.

**Q: You see `powershell.exe -nop -w hidden -enc SQBFAFgA...`. What do you do?**
That's PowerShell with no profile, a hidden window, and a base64-encoded command — strong indicators of malicious use. I'd decode the base64 to read the actual command, check the parent process (Office app spawning PowerShell is a big flag), look for outbound connections from that PID, pull script-block logs (4104), and check the host against threat intel. The `SQBFAFgA` decodes to "IEX" — an in-memory execution cradle.

**Q: What is Squiblydoo?**
Abuse of `regsvr32.exe` to download and execute a remote `.sct` scriptlet, bypassing application allowlisting because regsvr32 is a trusted signed Microsoft binary. Detect via regsvr32 making network connections or referencing scrobj.dll with a URL.

### SOC L1 angle
The key event IDs: **4688** (process creation with command line — enable command-line auditing), **4104** (PowerShell **script block logging** — your best PowerShell visibility), **4103** (module logging), and **Sysmon EID 1** (process create, rich detail incl. parent, hashes, command line). Triage mantra: **parent process + command line + network connections**. Decode every encoded command. Office → PowerShell/cmd/wscript is a top phishing-execution signal.

---

# 6. Cyber Kill Chain

### 60-second answer
The Lockheed Martin Cyber Kill Chain models an intrusion as seven sequential stages. The value for a defender is that **disrupting any one stage breaks the attack**, and it gives you a shared language to map where you have detection coverage. Modern SOCs pair it with **MITRE ATT&CK** for granular technique mapping and the **Diamond Model** for adversary/infrastructure/victim/capability analysis.

### The 7 stages (know each with a defensive action)
1. **Reconnaissance** — target research, OSINT, scanning. *Defend:* attack-surface reduction, monitor for scanning.
2. **Weaponization** — building the payload (e.g., malicious doc). *Defend:* threat intel on tooling.
3. **Delivery** — getting it to the target (phishing email, USB, web). *Defend:* email security, web filtering.
4. **Exploitation** — code executes / vuln triggered. *Defend:* patching, EDR, app allowlisting.
5. **Installation** — persistence established (registry run keys, scheduled task, service). *Defend:* EDR, autoruns monitoring.
6. **Command & Control (C2)** — beacon to attacker infrastructure. *Defend:* DNS/proxy monitoring, network detection.
7. **Actions on Objectives** — exfiltration, encryption, destruction. *Defend:* DLP, segmentation, anomaly detection.

### Likely Q&A
**Q: Why is the kill chain useful to a SOC analyst?**
It tells you *where in an intrusion* an alert sits, so you understand attacker progress and urgency. A delivery-stage alert (phishing) is earlier and less severe than a C2 or actions-on-objectives alert. Mapping your detections to stages also exposes coverage gaps. The principle is that breaking any single link stops the chain.

**Q: Kill Chain vs MITRE ATT&CK?**
The Kill Chain is a high-level linear *phase* model (7 stages). ATT&CK is a detailed matrix of **tactics** (the why — e.g., Persistence, Lateral Movement) and **techniques** (the how — e.g., T1053 Scheduled Task). Kill Chain gives you the narrative arc; ATT&CK gives you the granular, non-linear technique detail you actually map detections to.

**Q: Where would you place a Cobalt Strike beacon in the kill chain?**
Command & Control (stage 6) — the implant is beaconing to attacker infrastructure for instructions, typically after exploitation and installation.

### SOC L1 angle
When you triage, mentally place the alert on the chain — it drives priority and what to look for *next* (e.g., if you caught installation, hunt for prior delivery/exploitation and following C2). Interviewers love "map this scenario to the kill chain / ATT&CK."

---

# 7. Windows Security Fundamentals

### 60-second answer
Windows security rests on **identities (SIDs), access tokens, privileges, and the security event log**. As an L1 analyst, the single most important skill is reading **Windows Security Event IDs** to reconstruct what happened. Key controls: **UAC**, **Windows Defender / Defender for Endpoint**, application control (**AppLocker / WDAC**), and **Sysmon** for enriched telemetry.

### Event IDs you must know cold
| ID | Meaning | Why it matters |
|----|---------|----------------|
| **4624** | Successful logon | Logon type tells you how (3=network, 10=RDP) |
| **4625** | Failed logon | Brute force / password spray (watch counts + type) |
| **4634 / 4647** | Logoff / user-initiated logoff | Session correlation |
| **4648** | Logon with explicit credentials | runas, lateral movement |
| **4672** | Special privileges assigned (admin logon) | Privileged session started |
| **4688** | Process creation (+ command line) | Core execution visibility |
| **4720** | User account created | Persistence / unauthorized account |
| **4722/4725** | Account enabled / disabled | |
| **4724/4738** | Password reset / account changed | ACL abuse, takeover |
| **4728/4732/4756** | Member added to (global/local/universal) security group | Privilege escalation — esp. to Domain/Enterprise Admins |
| **4768** | Kerberos TGT requested (AS) | AS-REP roast, RC4 anomalies |
| **4769** | Kerberos service ticket requested (TGS) | **Kerberoasting** (RC4 spike) |
| **4771** | Kerberos pre-auth failed | Brute force against Kerberos |
| **4776** | NTLM authentication validated | NTLM use / Pass-the-Hash |
| **4662** | Operation on AD object | **DCSync** (replication GUID) |
| **4697 / 7045** | Service installed | Persistence (PsExec, malware services) |
| **1102** | Audit log cleared | Anti-forensics — always investigate |
| **4698** | Scheduled task created | Persistence |

### Sysmon (the SOC analyst's force multiplier)
- **EID 1** Process creation (parent, hashes, command line) — richer than 4688.
- **EID 3** Network connection.
- **EID 7** Image/DLL loaded — DLL hijacking/injection hunting.
- **EID 8** CreateRemoteThread — code injection.
- **EID 10** ProcessAccess — **LSASS access = credential dumping** (Mimikatz).
- **EID 11** File create.
- **EID 13** Registry value set — persistence (Run keys).
- **EID 22** DNS query.

### Know cold
- **SID**: unique identifier for a security principal. Well-known ones: RID 500 = built-in Administrator, RID 512 = Domain Admins, 519 = Enterprise Admins.
- **Access token**: holds the user's SID + group SIDs + privileges; checked against object DACLs.
- **UAC**: separates standard and admin tokens for the same user; elevation prompts.
- **AppLocker vs WDAC**: AppLocker is policy-based allow/deny by path/publisher/hash, easier but bypassable; **WDAC** (Windows Defender Application Control) is a stronger, kernel-enforced code-integrity policy, harder to bypass, harder to manage.

### Likely Q&A
**Q: You see 200 × 4625 then one 4624 from the same source. Interpretation?**
A successful brute-force or password-spray: many failed logons followed by a success from the same source IP/account. I'd check the logon type (3 = remote), the target account (privileged?), source IP reputation, and whether subsequent activity (4672, lateral movement) followed. Immediate action: flag, possibly isolate/disable, escalate.

**Q: Event 1102 appears. So what?**
The security audit log was cleared — a classic anti-forensics / defense-evasion move (ATT&CK T1070). It almost never has a benign cause in production, so I treat it as high priority, correlate what happened right before the clear, and check whether the account doing it should have that capability.

**Q: 4688 vs Sysmon EID 1?**
Both log process creation. 4688 is native (needs command-line auditing enabled); Sysmon EID 1 is richer — it includes the parent process, file hashes, image path, and GUIDs, which makes building process trees and pivoting far easier.

### SOC L1 angle
This *is* the L1 job. Interviewers will read you a few event IDs and ask "what happened." Practice narrating event sequences into a story: logon → privilege → process → network. Always note **logon type**, **source**, **account**, and **what followed**.

---

# 8. Email & Phishing Security

### 60-second answer
Phishing is the #1 initial-access vector, so a huge share of L1 work is triaging reported emails. The three authentication controls are **SPF, DKIM, DMARC**. Triage means analyzing **headers** (real sender, path, auth results), **URLs** (defanged, expanded, reputation), and **attachments** (hash, sandbox) — then deciding verdict and scope (was anyone else hit, did anyone click).

### Know cold
- **SPF (Sender Policy Framework)**: DNS TXT record listing IPs allowed to send for a domain. Checks the envelope/return-path. Fails on forwarding.
- **DKIM (DomainKeys Identified Mail)**: cryptographic signature in the header; the public key is in DNS. Proves the message wasn't altered and came from the domain.
- **DMARC**: policy (none/quarantine/reject) built on SPF + DKIM **alignment** (the visible From must align with the authenticated domain). Tells receivers what to do on failure and where to send reports.
- **Phishing types**: bulk phishing, **spear phishing** (targeted), **whaling** (executives), **BEC (Business Email Compromise)** — often no malicious link at all, pure social engineering for wire fraud/gift cards, **clone phishing**, **vishing/smishing** (voice/SMS).
- **Header fields**: `Received` (path, read bottom-up = origin), `Return-Path`, `Reply-To` (mismatch with From = flag), `Authentication-Results` (SPF/DKIM/DMARC verdicts), `Message-ID`, originating IP.
- **Indicators**: display-name spoofing, lookalike domains (rn vs m, paypa1), urgency/authority pressure, mismatched Reply-To, unexpected attachments (.html, .iso, .lnk, macro docs), link text ≠ link target.

### Triage workflow (say this — it shows process)
1. **Preserve** — work from the original `.eml`/headers, never click live links.
2. **Headers** — true sender IP, SPF/DKIM/DMARC results, From vs Reply-To/Return-Path alignment.
3. **URLs** — defang, expand shorteners, check reputation (VirusTotal/URLScan) in a safe environment.
4. **Attachments** — hash and check threat intel; detonate in a sandbox if needed.
5. **Scope** — search the mail gateway: who else received it, who clicked, who replied.
6. **Contain** — quarantine/purge across mailboxes, block sender/domain/URL, reset creds if anyone authenticated to a phishing page.
7. **Verdict + document** — malicious/spam/benign, with IOCs.

### Likely Q&A
**Q: Explain SPF, DKIM, DMARC together.**
SPF authorizes sending IPs for a domain; DKIM cryptographically signs the message so the receiver can verify integrity and origin; DMARC ties them together with alignment and tells the receiver what to do (none/quarantine/reject) when checks fail, plus sends reporting. You need all three — SPF and DKIM alone don't enforce a policy on the *visible* From address; DMARC does.

**Q: An email passes SPF but is still phishing. How?**
SPF only validates the envelope sender's IP, not the visible From. An attacker can send from a domain they legitimately control (which passes its own SPF) while spoofing the *display* name, or abuse a compromised account, or use a lookalike domain. That's why DMARC alignment and content/behavior analysis matter, not just a green SPF.

**Q: How do you analyze a suspicious link without getting compromised?**
Never click it directly. Defang it, expand any shortener, and check it against reputation services (VirusTotal, URLScan) or detonate in an isolated sandbox/VM. Inspect for lookalike domains and credential-harvesting pages.

### SOC L1 angle
Phishing triage is the most common L1 ticket. You may have a **SOAR** playbook (e.g., n8n: webhook → VirusTotal enrichment → scoring → ticket). Be ready to describe the *scoping* step — "did anyone else get it / click it" — because that's what separates closing one ticket from catching an incident.

---

# 9. Detection Engineering & SOC Operations

### 60-second answer
Detection engineering is building and tuning the logic that turns raw logs into actionable alerts; SOC operations is the people/process side — triage, escalation, response. The pipeline: **log sources → SIEM → detection rules → alerts → triage (L1) → escalation (L2/L3)**. Core artifacts: **detection rules** (often written in **Sigma**, a vendor-agnostic format), **use cases** mapped to **MITRE ATT&CK**, and **runbooks/playbooks**. Quality is measured by reducing false positives and lowering **MTTD/MTTR**.

### Know cold
- **SIEM**: aggregates and correlates logs, runs detection rules, raises alerts (e.g., Wazuh, Splunk, Sentinel, Elastic).
- **IOC vs IOA**: Indicator of *Compromise* = evidence something already happened (hash, IP, domain). Indicator of *Attack* = behavior/intent in progress (e.g., process injection pattern). IOAs survive attacker infrastructure changes.
- **True/False Positive, True/False Negative**: TP = real and alerted; FP = benign but alerted (tune it); FN = real but missed (worst); TN = benign and silent. **Tuning** = raising TP rate, cutting FP noise without creating FNs.
- **Sigma**: YAML-based, vendor-neutral detection rule format; converted to backend queries. Lets you share detections across SIEMs.
- **SOC tiers**: **L1** triage/initial investigation/escalation; **L2** deeper investigation/response; **L3** threat hunting/IR/advanced analysis.
- **SOAR**: automation/orchestration — playbooks that auto-enrich and respond (e.g., auto-pull VirusTotal data, isolate a host).
- **Threat intel**: feeds of IOCs/TTPs to enrich alerts; matched via lists (e.g., Wazuh **CDB lists**).
- **Metrics**: **MTTD** (mean time to detect), **MTTR** (mean time to respond), dwell time, alert volume, FP rate.
- **MITRE ATT&CK**: tactics/techniques framework — you map detections and triage findings to it.

### The L1 triage workflow (your core deliverable in the interview)
1. **Acknowledge & read the alert** — what fired, which rule, which asset/user.
2. **Validate** — TP or FP? Gather context (logs around the event, host/user history).
3. **Scope** — is it isolated or part of a pattern? Other hosts/users?
4. **Enrich** — threat intel on IPs/hashes/domains; check the asset's role/criticality.
5. **Decide** — close as FP (and suggest tuning), or **escalate** to L2 with a clear summary.
6. **Document** — timeline, IOCs, actions, recommendation.

### Likely Q&A
**Q: How do you handle a flood of the same alert?**
First confirm whether it's a true or false positive on a sample. If FP, identify the benign cause and recommend a tuning/exclusion to the rule so it stops generating noise (without introducing a blind spot). If TP, it may be an active incident — scope it and escalate. Never blanket-suppress without understanding the root cause.

**Q: IOC vs IOA?**
An IOC is forensic evidence of a known-bad artifact — a hash, IP, or domain — useful but brittle because attackers rotate infrastructure. An IOA describes adversary *behavior/intent* — like a process spawning PowerShell that injects into another process — which detects the technique regardless of the specific tooling, so it's more resilient.

**Q: When do you escalate vs close?**
I escalate when I've validated a true positive that's beyond L1 scope to remediate, when scope shows multiple affected assets, when there's evidence of privilege escalation/lateral movement/data access, or when I'm genuinely uncertain and the asset is critical. I close (with documentation and a tuning suggestion) when I've confirmed a false positive or benign activity. The rule of thumb: when in doubt on a critical asset, escalate.

**Q: What makes a good detection rule?**
High fidelity (catches the real behavior with low FPs), mapped to a known technique (ATT&CK), based on resilient signals (IOA over brittle IOC where possible), tested against benign baselines, and documented so the analyst knows what to do when it fires.

### SOC L1 angle
This domain ties everything together — they want to see *structured thinking*. Always answer scenario questions with the workflow: **validate → scope → enrich → decide → document**. Mention MITRE mapping and a tuning mindset; it signals maturity beyond button-pushing.

---

# 10. Cryptography Fundamentals

### 60-second answer
Crypto provides **confidentiality** (encryption), **integrity** (hashing/MACs), **authentication**, and **non-repudiation** (digital signatures). Two families: **symmetric** (one shared key, fast — AES) and **asymmetric** (public/private key pair — RSA, ECC). **Hashing** is one-way (SHA-256), not encryption. **PKI** binds public keys to identities via certificates, which underpins **TLS**.

### Know cold
- **Symmetric**: one key for encrypt+decrypt. Fast, good for bulk data. **AES** (128/256). Challenge = key distribution.
- **Asymmetric**: public key encrypts / private decrypts (confidentiality); private signs / public verifies (authenticity). **RSA**, **ECC** (smaller keys, same strength). Slow — used to exchange a symmetric key.
- **Hashing**: one-way, fixed-length digest. **SHA-256** good; **MD5/SHA-1** broken (collisions) — fine as file IDs, *not* for security. Properties: deterministic, irreversible, collision-resistant.
- **Salt**: random per-password value added before hashing to defeat rainbow tables / identical-password detection.
- **HMAC**: hash + secret key = integrity **and** authentication of a message.
- **Digital signature**: hash the message, encrypt the hash with the *private* key. Verifier decrypts with the public key and compares. Gives integrity, authentication, non-repudiation.
- **PKI / certificates**: a **CA** signs a certificate binding a public key to an identity. Chain of trust up to a root CA. X.509.
- **TLS handshake (conceptually)**: client and server agree on parameters, the server proves identity with its certificate, they use asymmetric crypto to establish a shared symmetric **session key**, then switch to fast symmetric encryption for the data.
- **Encoding ≠ encryption ≠ hashing**: Base64 is *encoding* (reversible, no key, no security) — attackers use it to obfuscate, not protect. Don't confuse these in an interview.

### Likely Q&A
**Q: Symmetric vs asymmetric — when do you use each?**
Symmetric (AES) is fast and used for bulk data encryption but needs a secure way to share the key. Asymmetric (RSA/ECC) solves key distribution and enables signatures, but is slow. In practice you combine them: asymmetric to securely exchange a symmetric session key (exactly what TLS does), then symmetric for the actual data.

**Q: Is hashing encryption?**
No. Encryption is reversible with a key; hashing is a one-way function with no key and no way back. Hashing verifies integrity and stores passwords (salted); encryption protects confidentiality. Treating Base64 or hashing as "encryption" is a common and revealing mistake.

**Q: How does a digital signature work?**
You hash the message, then encrypt that hash with your **private** key — that's the signature. The recipient decrypts it with your **public** key and independently hashes the message; if the hashes match, it proves integrity (unaltered), authenticity (only you have the private key), and non-repudiation.

**Q: Why salt passwords?**
A salt is a unique random value added to each password before hashing, so identical passwords produce different hashes and precomputed rainbow tables become useless. It forces an attacker to crack each hash individually.

### SOC L1 angle
You'll meet crypto in TLS-inspected traffic, certificate anomalies (self-signed/expired/mismatched on C2), ransomware (encrypts files), and **hashes as IOCs**. Know that attackers use Base64 *encoding* to obfuscate PowerShell — decoding it is a daily triage task. RC4 in Kerberos = weak/legacy and a Kerberoasting signal.

---

# 11. Real-Scenario-Based Attacks & Preventions (Scenario Bank)

Practice these *out loud*. For each: state the **hypothesis**, the **triage steps**, the **verdict/escalation**, and the **prevention**. Use the L1 workflow (validate → scope → enrich → decide → document).

---

### Scenario 1 — Password Spray
**Telemetry:** 400+ **4625** failures across many accounts, one failed password each, from a single external IP over 30 minutes, then a few **4624** successes.
**Hypothesis:** Password spray (one common password against many users to avoid lockout), with some successes.
**Triage:** Confirm logon type (3 = remote auth, often against a portal/VPN). Identify the source IP and check reputation. List which accounts succeeded and whether any are privileged. Check for follow-on activity (4672, MFA prompts, new sessions).
**Action:** Disable/force-reset compromised accounts, block the source IP, escalate. Check if MFA was bypassed.
**Prevention:** MFA everywhere, smart lockout, conditional access / geo-blocking, ban common passwords.

### Scenario 2 — Kerberoasting
**Telemetry:** Single user generates a burst of **4769** (TGS requests) for many distinct SPNs, encryption type **0x17 (RC4)**.
**Hypothesis:** Kerberoasting — harvesting service tickets to crack service-account passwords offline.
**Triage:** Confirm the requesting user is one principal hitting many SPNs in a short window with RC4. Identify the targeted service accounts and their privilege. No failed logons will appear (it's legitimate Kerberos). Check the host the requests came from.
**Action:** Escalate; treat targeted service accounts as at-risk; consider resetting their passwords.
**Prevention:** Long random passwords or **gMSA** for service accounts, enforce **AES**, monitor 4769 RC4 spikes.

### Scenario 3 — Phishing → Malicious Macro
**Telemetry:** User reports an invoice email; **Sysmon EID 1** shows `WINWORD.EXE` spawning `powershell.exe -nop -w hidden -enc ...`; **EID 3** outbound to an unknown IP.
**Hypothesis:** Macro-enabled document executed a download cradle for second-stage payload (kill-chain: delivery → exploitation → installation/C2).
**Triage:** Decode the base64 command. Confirm parent = Office app (high-fidelity malicious signal). Identify the C2 IP/domain, check intel. Pull 4104 script-block logs. Scope: who else received the email (mail gateway search), who else opened it.
**Action:** Isolate the host, block the C2 and sender, purge the email org-wide, reset the user's creds, escalate to L2 for IR.
**Prevention:** Disable Office macros from the internet, email sandboxing, EDR behavioral rules (Office → PowerShell), user awareness.

### Scenario 4 — Credential Dumping (Mimikatz)
**Telemetry:** **Sysmon EID 10** ProcessAccess targeting **lsass.exe** with a high-access mask from a non-system process; possibly EID 1 showing an unusual binary.
**Hypothesis:** LSASS memory access to dump credentials (Mimikatz / comsvcs.dll minidump).
**Triage:** Identify the accessing process and its parent; check the granted-access mask (0x1010/0x1410 are classic). Verify it isn't a legit security tool. Check what account is logged in (whose creds were exposed). Look for follow-on lateral movement (4624 type 3 / 4776 to other hosts).
**Action:** Isolate, escalate, reset exposed credentials (assume all interactive accounts on that host are compromised).
**Prevention:** **Credential Guard**, **Protected Users** group, LSASS protection (RunAsPPL), restrict local admin, EDR.

### Scenario 5 — DCSync
**Telemetry:** **4662** with the **DS-Replication-Get-Changes** GUID from a workstation/user that is **not a DC**.
**Hypothesis:** DCSync — replicating directory data to steal hashes (including KRBTGT) without logging into a DC.
**Triage:** Confirm the source isn't a legitimate DC and the account doesn't normally have replication rights. Determine how the account obtained `Replicating Directory Changes` (ACL abuse?). Assume KRBTGT may be exposed.
**Action:** High-severity escalation; treat as potential full domain compromise; plan **KRBTGT double reset**.
**Prevention:** Restrict replication rights to DCs only, monitor 4662 replication GUID, tier-0 protection, ACL audits.

### Scenario 6 — Lateral Movement via PsExec
**Telemetry:** **7045** service install named randomly (e.g., `PSEXESVC`) on a server, preceded by **4624 type 3** with admin creds, **4672** special privileges.
**Hypothesis:** Remote execution / lateral movement using PsExec-style admin service install.
**Triage:** Identify the source host of the network logon and the account used (compromised admin?). Map which hosts received the same service. Correlate with prior credential theft.
**Action:** Trace the chain back to patient zero, isolate affected hosts, escalate.
**Prevention:** Restrict admin lateral logons (tiering), LAPS (unique local admin passwords), network segmentation, monitor 7045.

### Scenario 7 — DNS Tunneling / Exfiltration
**Telemetry:** A host makes thousands of DNS queries to one second-level domain, subdomains are long and high-entropy, frequent TXT lookups.
**Hypothesis:** DNS tunneling for C2 or data exfiltration.
**Triage:** Confirm the volume and entropy via **Sysmon EID 22**. Identify the process making the queries. Check the domain's age/reputation. Estimate data volume.
**Action:** Block the domain, isolate the host, escalate; investigate what process/data is involved.
**Prevention:** DNS query monitoring/anomaly detection, block newly-registered domains, restrict outbound DNS to approved resolvers, threat-intel matching.

### Scenario 8 — Golden Ticket
**Telemetry:** Logons/service access with **no preceding 4768** (TGT request), anomalous ticket lifetimes, an account showing privileges it shouldn't, activity continuing after a password reset.
**Hypothesis:** Golden Ticket — forged TGT signed with a stolen KRBTGT hash.
**Triage:** Look for Kerberos service usage without a matching AS exchange, mismatched username/RID, abnormal ticket lifetime. Correlate with any earlier DCSync/DC compromise.
**Action:** Critical escalation; **reset KRBTGT twice** (with replication interval between); full domain IR.
**Prevention:** Protect KRBTGT/DCs (tier 0), least privilege, detect DCSync early, periodic KRBTGT rotation.

### Scenario 9 — Brute Force on Exposed RDP
**Telemetry:** Many **4625 type 10** from external IPs, then a **4624 type 10** success, then **4672**.
**Hypothesis:** RDP brute force leading to successful admin compromise.
**Triage:** Confirm RDP exposure, source IP reputation, which account succeeded and its privilege, follow-on actions (tools dropped, lateral movement).
**Action:** Disconnect/isolate, reset creds, block IPs, escalate; check for ransomware staging.
**Prevention:** Don't expose RDP to the internet (VPN/gateway), MFA, NLA, account lockout, geo-blocking.

### Scenario 10 — Log Clearing (Anti-Forensics)
**Telemetry:** **1102** (security log cleared), possibly preceded by suspicious admin activity.
**Hypothesis:** Defense evasion — attacker covering tracks (ATT&CK T1070).
**Triage:** Identify who cleared it and whether that's authorized. Reconstruct activity from *other* sources (Sysmon forwarded to SIEM, EDR, network logs) since the local log is gone — this is exactly why you forward logs centrally.
**Action:** Treat as high priority, escalate, pivot to retained telemetry.
**Prevention:** Forward logs to SIEM in real time (so clearing local logs doesn't destroy evidence), restrict who can clear logs, alert on 1102.

### Scenario 11 — Suspicious Scheduled Task Persistence
**Telemetry:** **4698** scheduled task created running a script from a temp/AppData path, or **Sysmon EID 13** registry Run-key write.
**Hypothesis:** Persistence mechanism established post-exploitation (kill-chain: installation).
**Triage:** Inspect the task action/command, the path, the creating account, and timing. Correlate with prior execution alerts.
**Action:** Hunt for the initial compromise (this is a *symptom*), remove persistence, escalate.
**Prevention:** Monitor task/Run-key creation, app allowlisting, least privilege.

### Scenario 12 — BEC (no malware at all)
**Telemetry:** Email from a lookalike domain or compromised vendor account asking to change banking details / urgent wire; clean attachments, passes SPF (attacker's own domain).
**Hypothesis:** Business Email Compromise — social engineering for fraud, no technical payload.
**Triage:** Check From vs Reply-To, domain similarity (lookalike), whether it's a real but compromised account. Verify the request out-of-band. Scope who received it.
**Action:** Block sender/domain, warn finance, advise out-of-band verification, escalate if a real account is compromised.
**Prevention:** DMARC enforcement, payment-change verification policy (call-back), user training, external-sender banners.

---

# Rapid-Fire Round (one-line answers — drill these)

- **Logon type 3?** Network (SMB, PtH).
- **Logon type 10?** RemoteInteractive (RDP).
- **4625?** Failed logon.
- **4769 + RC4 spike?** Kerberoasting.
- **4662 + replication GUID?** DCSync.
- **Sysmon EID 10 on lsass?** Credential dumping.
- **Sysmon EID 22?** DNS query.
- **1102?** Security log cleared (anti-forensics).
- **7045?** Service installed (often lateral movement/persistence).
- **Golden ticket forged with?** KRBTGT hash.
- **Silver ticket forged with?** Target service account hash.
- **SPF checks?** Authorized sending IPs (envelope).
- **DKIM provides?** Signature/integrity + origin.
- **DMARC adds?** Policy + alignment + reporting.
- **IOC vs IOA?** Artifact of past compromise vs behavior of attack in progress.
- **Symmetric example?** AES. **Asymmetric?** RSA/ECC.
- **Is hashing reversible?** No.
- **Base64 is?** Encoding, not encryption.
- **Kill chain C2 stage = number?** 6.
- **Tier 0 = ?** DCs / identity infrastructure.
- **First step on any alert?** Validate TP vs FP, gather context.
- **When in doubt on a critical asset?** Escalate.
- **MTTD / MTTR?** Mean time to detect / respond.
- **NXDOMAIN burst?** Possible DGA malware.
- **Office app spawning PowerShell?** Phishing execution — high fidelity.

---

# Event ID Cheat Sheet (memorize)

```
AUTH / LOGON
4624  Logon success        4625  Logon failure
4634  Logoff               4647  User-initiated logoff
4648  Explicit-cred logon  4672  Special privileges (admin)
4776  NTLM validated

KERBEROS
4768  TGT requested (AS-REP roast / RC4)
4769  Service ticket (TGS) requested (KERBEROASTING)
4771  Pre-auth failed (Kerberos brute force)

ACCOUNT / GROUP / AD
4720  Account created      4724/4738  Pwd reset / acct changed
4728/4732/4756  Added to security group (priv-esc)
4662  AD object op (DCSYNC replication GUID)

EXECUTION / PERSISTENCE / EVASION
4688  Process creation (+cmdline)
4697/7045  Service installed
4698  Scheduled task created
4104  PowerShell script block logging
1102  Security log cleared (ANTI-FORENSICS)

SYSMON
1 Process create | 3 Network | 7 Image load | 8 RemoteThread
10 ProcessAccess (LSASS=cred dump) | 11 File create
13 Registry set (persistence) | 22 DNS query
```

---

# Final Checklist (the night before)

- [ ] Can explain forest/domain/OU/FSMO/GC in plain words.
- [ ] Can describe each AD attack with: what it abuses + one detection signal + one mitigation.
- [ ] Can recite the Kerberos AS/TGS/AP flow and why Silver tickets + Kerberoasting work.
- [ ] Know all logon types by number.
- [ ] Can read an event-ID sequence and narrate the story.
- [ ] Can explain SPF/DKIM/DMARC and walk a phishing triage end to end.
- [ ] Can map a scenario to the kill chain *and* MITRE ATT&CK.
- [ ] Can define IOC vs IOA, TP/FP/FN, and a clean L1 triage workflow.
- [ ] Symmetric vs asymmetric vs hashing vs encoding — no confusion.
- [ ] Can run *any* of the 12 scenarios out loud: hypothesis → triage → action → prevention.

**Interview mindset:** They're hiring someone who can *triage calmly and reason structurally*, not someone who memorized definitions. For every scenario, lead with a hypothesis, show your workflow, and end with "I'd escalate to L2 because…". That's what gets you the offer.
