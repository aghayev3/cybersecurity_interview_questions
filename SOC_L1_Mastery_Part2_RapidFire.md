# SOC L1 Interview — Mastery Guide, Part 2 (Rapid-Fire 22)

**How to use this:** These are the short, sharp questions interviewers fire to test whether you *understand* a concept or just memorized a definition. Each one gets the **model answer** (what to say), **know cold** (the supporting detail), and where relevant the **SOC angle** (detection signal). Watch for the items tagged **⚠ GOTCHA** — those are deliberate traps where the obvious answer is wrong.

Study them in their clusters, not as 22 loose facts:
- **Crypto cluster:** 1, 2
- **Network / L2-L3 cluster:** 3, 6, 22
- **Malware / execution cluster:** 4, 7, 9
- **Auth attack cluster:** 5, 16, 17, 18, 19, 20, 21
- **DHCP / DNS / MITM cluster (learn as one story):** 10, 11, 12, 13, 14, 15
- **Linux:** 8

---

# Crypto cluster

## 1. Salting — what and why

**Model answer:** A salt is a unique random value added to a password before it's hashed. Its purpose is that identical passwords produce different hashes, so an attacker can't see who shares a password, and precomputed **rainbow tables** become useless — they'd have to crack each hash individually.

**Know cold:**
- A salt does **not** make a weak password strong — it defeats *bulk/precomputed* attacks, not targeted cracking.
- Salt is stored alongside the hash (it's not secret). A **pepper** is a *secret* salt stored separately (e.g., in app config, not the DB).
- Without salt: two users with password `Summer2024` get the same hash → attacker cracks once, owns both.

**SOC angle:** You meet this when reasoning about password-hash theft (SAM/NTDS.dit dumps, DCSync). Unsalted or weakly-hashed creds crack fast offline — relevant to how urgently you treat a credential-exposure incident.

---

## 2. Diffie-Hellman

**Model answer:** Diffie-Hellman is a key-exchange protocol that lets two parties agree on a shared secret over an insecure channel **without ever transmitting the secret itself**. Its security rests on the discrete-logarithm problem.

**Know cold:**
- It's **key exchange, not encryption**, and **not authentication** on its own — plain DH is vulnerable to **man-in-the-middle** unless combined with authentication (certificates/signatures).
- **DHE / ECDHE** (ephemeral variants) provide **forward secrecy** — a new key per session, so compromising the long-term private key doesn't expose past traffic.
- Used inside the **TLS handshake** to establish the symmetric session key.

**Trap to avoid:** Don't call it "an encryption algorithm." It establishes a key; AES (or similar) does the encrypting afterward.

---

# Network / L2-L3 cluster

## 3. ARP Poisoning

**Model answer:** ARP maps IP addresses to MAC addresses on a LAN and has **no authentication**, so an attacker sends forged ARP replies claiming their MAC owns the gateway's (or a victim's) IP. Victims then send traffic to the attacker, giving them a **man-in-the-middle** position.

**Know cold:**
- Works only on the local subnet (Layer 2).
- Often the foundation for traffic interception, session hijacking, or credential capture.
- *Detect:* duplicate IP-to-MAC mappings, a flood of unsolicited (gratuitous) ARP replies, one MAC suddenly claiming many IPs.
- *Mitigate:* **Dynamic ARP Inspection (DAI)**, **DHCP snooping**, static ARP entries for critical hosts.

---

## 6. Does ping use TCP or UDP? ⚠ GOTCHA

**Model answer:** Neither. Ping uses **ICMP** — Echo Request (type 8) and Echo Reply (type 0). ICMP runs directly on top of IP at Layer 3 and has **no port numbers** because it isn't a transport protocol.

**Know cold:** Saying "TCP" or "UDP" is the classic trap. ICMP is also used by `traceroute` and by attackers for **ICMP tunneling** (exfil/C2 hidden in ping payloads) and host discovery sweeps.

---

## 22. External Routing Protocols

**Model answer:** The external (exterior gateway) protocol that runs the internet is **BGP — Border Gateway Protocol**. It routes between **autonomous systems (AS)** and makes path decisions by policy and AS-path rather than simple metrics.

**Know cold:**
- **Exterior:** BGP (between organizations/ASes).
- **Interior (IGPs, within one network):** OSPF, EIGRP, RIP, IS-IS.
- *Security angle:* **BGP hijacking** — an AS announces routes it doesn't own to reroute or intercept traffic. Hard to prevent without **RPKI** route validation.

---

# Malware / execution cluster

## 4. Fileless Malware

**Model answer:** Malware that executes in memory or through trusted built-in tools (**LOLBins** — PowerShell, WMI, rundll32, regsvr32) without writing a malicious file to disk, so signature- and hash-based AV has nothing to scan.

**Know cold:**
- Detected **behaviorally**, not by file: suspicious parent/child process trees (Office → PowerShell), command-line arguments, in-memory injection.
- *SOC signals:* **4104** (PowerShell script-block logging), **4688** (process + command line), **Sysmon EID 1** (rich process create), **EID 8** (CreateRemoteThread = injection).

---

## 7. Encoded PowerShell

**Model answer:** `powershell -enc <base64>` (the `-EncodedCommand` flag) runs a Base64-encoded command — the string is UTF-16LE encoded — used to hide intent and evade simple signatures. It's usually paired with `-nop -w hidden -ep bypass`.

**Know cold:**
- *Triage step one is always:* **decode the Base64** to read the real command. Then check the **parent process** and any **outbound connections**.
- `SQBFAFgA` at the start decodes to `IEX` (Invoke-Expression) — an in-memory execution cradle.
- Base64 here is **encoding, not encryption** — reversible, no key.

**SOC angle:** Office app or `wscript`/`mshta` spawning encoded PowerShell is a high-fidelity phishing-execution signal.

---

## 9. How does persistence differ?

**Model answer:** Persistence is any mechanism that survives reboot or logoff to keep attacker access. The mechanisms differ by **where they live and what triggers them**.

**Know cold (the main types + signal):**
- **Registry Run keys** — trigger at logon (Sysmon EID 13).
- **Scheduled tasks** — time/event trigger (Event ID 4698).
- **Windows services** — boot trigger (Event ID 7045).
- **Startup folder** — logon trigger.
- **WMI event subscriptions** — stealthy, fileless, event-triggered.
- **DLL hijacking / search-order abuse.**
- **Golden Ticket** — identity-level persistence (survives password resets).

**Interview point:** Persistence is a **symptom**. When you find it, hunt *backward* for the initial compromise (delivery/exploitation) — don't just delete the task and close the ticket.

---

# Auth attack cluster

## 5. Brute Force vs Password Spraying

**Model answer:** Brute force throws **many passwords at one account** and trips account lockout quickly. Password spraying uses **one common password against many accounts**, staying under each account's lockout threshold, so it's much stealthier.

**SOC angle:** Both generate **4625** failures, but the *shape* differs:
- Brute force = many failures concentrated on **one account**.
- Spraying = **one failure each across many accounts**, often from a single source IP in a short window.
- Watch for the **4624 success** that follows, and whether the compromised account is privileged.

---

## 16. Does NTLM use encryption? ⚠ NUANCED

**Model answer:** Not in the way people expect. NTLM is a **challenge-response** protocol: the server sends a random nonce, the client encrypts it using a key derived from its **NT hash** (the MD4 hash of the password) and sends it back. So a cryptographic operation is involved, but NTLM provides **no session/channel encryption and no mutual authentication** on its own.

**Best phrasing:** "It uses the password hash in a challenge-response exchange, but it doesn't encrypt the session and has no mutual auth — which is exactly why it's exposed to relay and Pass-the-Hash."

---

## 17. Is NTLM vulnerable to replay?

**Model answer:** Yes — most importantly to **NTLM relay**: an attacker intercepts an authentication attempt and relays it to a *different* service to authenticate as the victim.

**Know cold:**
- **NTLMv1** is badly exposed. **NTLMv2** adds a client challenge + timestamp that limits straight replay, but **relay is still possible** because there's no mutual authentication or channel binding by default.
- *Mitigate:* **SMB signing**, channel binding / **EPA** (Extended Protection for Authentication), and disabling NTLM where possible in favor of Kerberos.

---

## 18. Does Kerberos use the password for each request? ⚠ GOTCHA

**Model answer:** No. The password (its hash) is used **once**, at the initial **AS exchange**, to obtain a **TGT**. After that the client uses **tickets and session keys** — the password is never sent again. That's the whole point of Kerberos: ticket-based **single sign-on**.

**Trap:** Answering "yes, every request" shows you don't understand the AS → TGS → AP flow. The TGT is what gets reused, not the password.

---

## 19. Is LDAP an authentication protocol? ⚠ NUANCED

**Model answer:** Primarily no — LDAP is a **directory access protocol** for querying and modifying a directory like Active Directory. It *can* perform authentication via the **bind** operation (simple bind = username/password, or SASL), but that's a feature, not its purpose.

**Know cold:**
- A **simple bind sends credentials in cleartext** unless wrapped in **LDAPS (636)** or StartTLS.
- *SOC angle:* heavy LDAP querying is classic **AD reconnaissance** (BloodHound, account/group enumeration — ATT&CK T1087).

**Best phrasing:** "LDAP is a directory protocol that supports authentication via bind — it's not an authentication protocol per se."

---

## 20. Pass-the-Hash (NTLM abuse)

**Model answer:** Because NTLM authenticates using the **hash rather than the plaintext password**, an attacker who steals an NT hash (from LSASS or the SAM) can authenticate as that user **without ever cracking it**.

**SOC angle:**
- *Detect:* **4624 Logon Type 3 + NTLM** from unexpected source hosts, LSASS access (**Sysmon EID 10** targeting lsass.exe), lateral-movement patterns.
- *Mitigate:* **Credential Guard**, **LAPS** (unique local admin passwords), restrict admin lateral logons (tiering), **Protected Users** group.

---

## 21. Kerberoasting / Golden / Silver Ticket

**Model answer:**
- **Kerberoasting:** any authenticated user can request a service ticket (TGS) for any account with an SPN; the ticket is encrypted with that **service account's password hash**, so the attacker cracks it offline. *Signal:* a spike in **4769** with **RC4 (0x17)** from one user to many SPNs.
- **Golden Ticket:** a forged **TGT** signed with the stolen **krbtgt** hash → access to anything, long-lived, **survives the impersonated user's password reset**, processed by the DC.
- **Silver Ticket:** a forged **TGS** signed with a single **service account's** hash → access to that one service only, and it **never contacts the DC**, making it stealthier.

**Key distinction:** Golden requires the **krbtgt** hash (you already own the domain) and is killed only by a **double krbtgt reset**; Silver needs just one service account's hash and is scoped to one service.

---

# DHCP / DNS / MITM cluster — *learn this as one connected story*

The attacker's progression: **starve the real DHCP server → stand up a rogue one → hand victims a malicious gateway and DNS → lock in the mapping with ARP spoofing → poison DNS answers.** Each question below is one link in that chain.

## 10. DHCP ports and protocol

**Model answer:** DHCP runs over **UDP** — server on port **67**, client on port **68**. (DHCPv6 uses 546/547.)

**Know cold:** The lease handshake is **DORA** — **D**iscover → **O**ffer → **R**equest → **A**cknowledge.

---

## 11. Can DHCP always assign the same IP to a device?

**Model answer:** Yes — through a **DHCP reservation**, a static MAC→IP binding on the server, so that device always receives the same address. Without a reservation the IP comes from the dynamic pool and *can* change at renewal, though it often stays the same while the lease is valid.

---

## 12. Rogue DHCP Server attack

**Model answer:** An unauthorized DHCP server that races to answer client DISCOVER requests and hands out a **malicious default gateway or DNS server**, putting the attacker in a man-in-the-middle position.

**Mitigate:** **DHCP snooping** — the switch only accepts DHCP offers from trusted (authorized) ports.

---

## 13. DHCP Starvation attack

**Model answer:** The attacker floods the DHCP server with **DISCOVER requests using spoofed MAC addresses**, exhausting the address pool so legitimate clients can't get an IP — a denial of service.

**Know cold:** It's often the **setup step before a rogue DHCP** — knock out the legitimate server, and the attacker's rogue server becomes the only one answering. *Mitigate:* **port security** (limit MACs per switch port) + DHCP snooping.

---

## 14. Combining DHCP + ARP spoofing

**Model answer:** A layered MITM. Starvation exhausts the legitimate server → the **rogue DHCP** makes the attacker the gateway/DNS for any client that joins → **ARP spoofing** then poisons the live IP↔MAC mapping so even already-connected or renewing clients route through the attacker.

**The logic to state:** DHCP controls the config a client gets *when it joins*; ARP poisons the *active* mapping. Together they give durable interception of both new and existing clients.

---

## 15. Combining DHCP + DNS poisoning

**Model answer:** The rogue DHCP hands clients an **attacker-controlled DNS server**. The attacker then answers name lookups with **malicious IPs**, silently redirecting victims (e.g., to credential-harvesting pages) while the domain in the browser still looks legitimate.

**The logic to state:** One attack controls *which resolver you trust*; the other controls *what that resolver tells you*. Combined, the victim resolves real domain names to attacker servers.

---

# Linux

## 8. File permission numbers (octal)

**Model answer:** Linux permissions are octal, summed per triad of **owner / group / other**, where **r = 4, w = 2, x = 1**.

**Know cold:**
- `755` → owner rwx (7), group r-x (5), other r-x (5) — typical for directories and executables.
- `644` → owner rw-, group r--, other r-- — typical for regular files.
- `777` → full access for everyone — a **red flag** in any security review.
- Quick math: a digit is just r+w+x added (e.g., rw- = 4+2 = 6).

---

# Rapid-Fire Recap (drill these one-liners)

- **Salt purpose?** Unique hashes for identical passwords; kills rainbow tables.
- **Diffie-Hellman is?** Key exchange (not encryption, not auth alone).
- **ARP poisoning enables?** Layer-2 MITM (no auth in ARP).
- **Brute force vs spray?** Many pwds/one acct vs one pwd/many accts.
- **Ping uses?** ICMP (not TCP/UDP, no ports). ⚠
- **First step on encoded PowerShell?** Decode the Base64.
- **`755` / `644` / `777`?** exec-dir / file / everyone-full (red flag).
- **Persistence is a?** Symptom — hunt backward for initial access.
- **DHCP ports?** UDP 67 (server) / 68 (client); DORA.
- **Same IP always?** Yes — DHCP reservation.
- **Rogue DHCP gives victim?** Malicious gateway/DNS → MITM.
- **DHCP starvation?** Spoofed-MAC DISCOVER flood → pool exhaustion (DoS).
- **DHCP + ARP?** Rogue config on join + poisoned live mapping = durable MITM.
- **DHCP + DNS?** Attacker resolver + malicious answers = silent redirect.
- **Does NTLM encrypt the session?** No — challenge-response only, no mutual auth.
- **NTLM replay?** Yes — relay; mitigate with SMB signing / EPA.
- **Kerberos password each request?** No — once at AS for the TGT. ⚠
- **Is LDAP an auth protocol?** No — directory protocol; auth via bind. ⚠
- **Pass-the-Hash signal?** 4624 type 3 + NTLM, Sysmon EID 10 on lsass.
- **Kerberoasting signal?** 4769 RC4 spike, one user → many SPNs.
- **Golden vs Silver?** Forged TGT (krbtgt) vs forged TGS (service acct).
- **External routing protocol?** BGP (between ASes); IGPs = OSPF/EIGRP/RIP/IS-IS.

---

# The three gotchas — don't get caught

1. **Ping → ICMP**, not TCP/UDP, no ports.
2. **Kerberos uses the password once** (at the AS to get the TGT), then tickets — not per request.
3. **LDAP is a directory protocol**, not an authentication protocol — it merely *supports* auth via bind.

If you can explain *why* each of those is the right answer (not just state it), you've shown the interviewer you understand the mechanics instead of reciting flashcards — which is exactly the difference between an L1 hire and a reject.
