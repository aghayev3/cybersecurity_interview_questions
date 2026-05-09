# Supplementary Interview Guide
### Covering Everything NOT in the 100-Question Guide
*Built specifically from your program's actual question bank*

---

> **How to use this:** Study this alongside the 100-question guide.
> Everything here fills gaps that were missing. Together they cover your full exam question bank.

---

## PART 1: ACTIVE DIRECTORY GAPS

---

**AD-S1. What is the difference between Forest, Tree, and Domain?**

- **Domain** — The basic unit of AD. A logical grouping of users, computers, and resources that share a common security boundary. Example: `company.com`
- **Tree** — One or more domains that share a contiguous DNS namespace. Example: `company.com` + `sales.company.com` form a tree.
- **Forest** — The top-level container. One or more trees that share a common AD schema and Global Catalog. The forest is the true security boundary in AD.

> *Key point:* Domains within a forest automatically have two-way transitive Kerberos trusts with each other. Forests do not trust each other by default.

---

**AD-S2. What does a Domain Controller do?**

A Domain Controller (DC) is a Windows server running Active Directory Domain Services (AD DS). Its core responsibilities:
- Authenticate users and computers (Kerberos/NTLM)
- Store and replicate the AD database (NTDS.dit)
- Enforce Group Policy
- Host DNS and SYSVOL
- Handle all AD queries and changes

> *Security importance:* Compromising a DC = compromising the entire domain. All domain accounts, passwords, and policies live here.

---

**AD-S3. What is SYSVOL and what does it store?**

SYSVOL is a shared folder on every Domain Controller that is replicated to all other DCs via DFS-R (or FRS on older systems). It stores:
- **Group Policy Objects (GPO templates)**
- **Logon/logoff scripts**
- **Other policy-related files**

> *Why it matters:* SYSVOL is replicated across all DCs so that GPO settings are consistent throughout the domain. It is accessible by all authenticated domain users at `\\domain\SYSVOL`. Attackers sometimes plant malicious scripts here if they have DC access.

---

**AD-S4. What is Delegation in Active Directory?**

Delegation allows an administrator to grant a user or group the ability to manage specific AD objects or attributes — without giving them full Domain Admin rights. This is called "Delegated Administration."

> *Example:* The help desk team is delegated the ability to reset passwords in the `OU=Employees` — but nothing else. They don't need Domain Admin for this one task.

> *Security risk:* Over-delegation is a privilege escalation path. BloodHound can find accounts with unexpected delegated rights.

---

**AD-S5. What is SID and RID?**

- **SID (Security Identifier)** — A unique identifier assigned to every security principal (user, group, computer) in Windows. Format: `S-1-5-21-[domain identifier]-[RID]`
- **RID (Relative Identifier)** — The last part of the SID. Some RIDs are fixed:
  - `500` — Built-in Administrator account
  - `501` — Built-in Guest account
  - `502` — KRBTGT
  - `512` — Domain Admins group
  - `1000+` — Regular user accounts start here

> *Attack relevance:* Golden Ticket attacks can forge a TGT for RID 500 (Administrator) even if that account is disabled. Knowing RIDs helps identify high-value targets.

---

**AD-S6. What are the Domain Trust types?**

| Trust Type | Direction | Transitive? | Description |
|-----------|-----------|-------------|-------------|
| **Parent-Child** | Two-way | Yes | Auto-created within a tree |
| **Tree-Root** | Two-way | Yes | Between tree roots in a forest |
| **External** | One or two-way | No | Between domains in different forests |
| **Forest** | One or two-way | Yes | Between entire forests |
| **Shortcut** | One or two-way | Yes | Manually created to speed up auth |
| **Realm** | One or two-way | Yes/No | Between AD and non-Windows Kerberos realms |

> *One-way trust:* If Domain A trusts Domain B, then Domain B's users can access resources in Domain A — not the other way around.

---

**AD-S7. Why are service accounts a security risk?**

Service accounts are non-human accounts used by applications and services to authenticate and access resources. They are risky because:
- Often have excessive privileges (admins take shortcuts)
- Passwords are rarely rotated — sometimes never
- Passwords are often weak and predictable
- The same service account is shared across multiple services
- They have SPNs registered, making them targets for Kerberoasting

> *Best practice:* Use **gMSA (Group Managed Service Accounts)** — AD auto-generates and rotates their 120-character passwords. No human ever knows the password.

---

**AD-S8. What is SPN (Service Principal Name)?**

An SPN is a unique identifier that associates a service with a specific account in AD. Format: `serviceclass/hostname:port`

> *Example:* `MSSQLSvc/dbserver.corp.com:1433`

Kerberos uses SPNs to know which account's hash to use when encrypting a Service Ticket. When a user wants to connect to the SQL server, they request a ticket for that SPN.

> *Attack:* Kerberoasting — any domain user can request a Service Ticket for any SPN. That ticket is encrypted with the service account's password hash — extractable and crackable offline.

---

**AD-S9. What is KRBTGT and what happens if it's compromised?**

KRBTGT is a special built-in domain account whose password hash is used by the KDC to encrypt and sign all Kerberos Ticket Granting Tickets (TGTs). It is the most sensitive account in AD.

**If compromised:** An attacker can forge valid TGTs for any account (Golden Ticket attack) — granting unlimited domain access that persists even after password resets.

> *Remediation:* After a compromise, the KRBTGT password must be rotated **twice** (because there's a previous password kept for compatibility). All existing tickets are then invalidated.

---

**AD-S10. What is Kerberos Pre-Authentication?**

A security feature that requires a user to prove knowledge of their password before the KDC issues a TGT.

**How it works:** The client sends an encrypted timestamp (encrypted with the user's password hash) as part of the AS-REQ. The KDC decrypts it — if valid, it issues a TGT.

**Without pre-auth (disabled):** The KDC returns an AS-REP without verifying identity. This AS-REP contains material encrypted with the user's password hash — which can be extracted and cracked offline. This is ASREPRoasting.

> *Why it exists:* Pre-authentication prevents an attacker from requesting encrypted material for any account and cracking it at leisure.

---

**AD-S11. What are FSMO Roles?**

Flexible Single Master Operations — 5 roles in AD that can only be held by one DC at a time (no multi-master for these specific operations):

| Role | Scope | Function |
|------|-------|---------|
| **Schema Master** | Forest-wide | Controls schema changes |
| **Domain Naming Master** | Forest-wide | Controls adding/removing domains |
| **PDC Emulator** | Per-domain | Time sync, password changes, legacy auth |
| **RID Master** | Per-domain | Issues RID pools to DCs for SID creation |
| **Infrastructure Master** | Per-domain | Maintains references between domains |

> *Key one to remember:* **PDC Emulator** is the most operationally critical — it's the time authority for the domain (Kerberos requires clocks within 5 minutes), handles account lockouts, and processes password changes.

---

**AD-S12. What is GPO Processing Order (LSDOU)?**

GPOs are applied in this order, with later ones overwriting earlier ones on conflicts:

1. **Local** — Local computer policy
2. **Site** — Policies linked to the AD site
3. **Domain** — Policies linked to the domain
4. **OU** — Policies linked to the OU (innermost OU last)

> *Rule:* Later wins on conflict. An OU-level GPO overrides a domain-level GPO.
> *Exception:* "Enforced" (No Override) reverses this — the enforced policy wins regardless of order.
> *Also:* "Block Inheritance" on an OU stops higher-level GPOs from applying (unless Enforced).

---

**AD-S13. What are SRV records and why are they needed in AD?**

SRV (Service) DNS records map a service to the server providing it. Format: `_service._protocol.domain TTL class SRV priority weight port target`

> *Example:* `_ldap._tcp.company.com SRV 0 0 389 dc01.company.com`

AD relies heavily on SRV records for clients to locate Domain Controllers, Global Catalogs, and Kerberos KDCs. When you join a computer to a domain, it uses DNS SRV records to find a DC.

> *If SRV records are missing or broken:* Domain joins fail, logins fail, Kerberos breaks. SRV records are critical AD infrastructure.

---

**AD-S14. What is BloodHound and how does Blue Team defend against it?**

BloodHound is an attack tool that maps Active Directory relationships — finding attack paths from low-privileged users to Domain Admin using graph theory.

It ingests AD data (via SharpHound collector) and visualises:
- Who has admin rights where
- Which accounts have delegation rights
- Shortest path to Domain Admin

**Blue Team defences:**
- Run BloodHound yourself regularly to find and fix attack paths before attackers do
- Audit and reduce privileged group memberships
- Remove unnecessary admin rights
- Monitor for SharpHound collection activity (unusual LDAP queries)
- Implement tiered administration model

---

**AD-S15. What is PAC (Privilege Attribute Certificate) in Kerberos?**

The PAC is a Microsoft extension to Kerberos. It is embedded in Kerberos tickets and contains:
- User's SID
- Group memberships
- User rights and privileges

When a service receives a Kerberos ticket, it reads the PAC to determine what the user is allowed to do — without needing to query AD itself.

> *Attack relevance:* MS14-068 was a famous vulnerability where an attacker could forge a PAC claiming Domain Admin membership in a regular user's TGT, gaining domain admin access. Fully patched systems are not vulnerable, but understanding PAC is important.

---

**AD-S16. What is the Global Catalog?**

The Global Catalog (GC) is a special DC role that stores a partial replica of all objects in all domains within the forest — not just its own domain.

**Why needed:** In a multi-domain forest, when a user in Domain A logs in, the system needs to know all their group memberships — including groups from Domain B and C. The GC has this cross-domain information.

**Port:** 3268 (LDAP to GC), 3269 (LDAPS to GC)

> *Without the GC:* Logins fail in multi-domain forests, Universal Group memberships can't be resolved, and some applications break.

---

**AD-S17. What is Password Policy and how is Default Domain Policy applied?**

Password Policy defines password requirements for domain accounts:
- Minimum length
- Complexity requirements
- Maximum age (expiry)
- Minimum age
- History (can't reuse last N passwords)
- Account lockout threshold/duration

**Default Domain Policy:** A built-in GPO linked to the domain root. Password policies must be set here to apply to domain accounts — password policies in OUs do NOT apply to domain accounts, only local accounts.

> *Fine-Grained Password Policies (FGPP):* Allow different password policies for different users/groups within the same domain, bypassing the one-policy limitation.

---

**AD-S18. Forward vs Reverse Lookup Zone in DNS**

- **Forward Lookup Zone** — Resolves hostname → IP address. Standard DNS. `company.com → 192.168.1.10`
- **Reverse Lookup Zone** — Resolves IP address → hostname. Uses PTR records. `192.168.1.10 → dc01.company.com`

> *Security use of reverse lookup:* Verifying that an IP's claimed hostname actually matches what it reverse-resolves to. Used in email spam filtering (FCrDNS check).

---

**AD-S19. What is Tombstone Lifetime in AD?**

When an AD object (user, computer, group) is deleted, it isn't immediately removed. It becomes a "tombstone" — a marker that the object was deleted — and is retained for the **Tombstone Lifetime** (default: 180 days).

**Why it matters:** During this window, AD replication can propagate the deletion to all DCs. After tombstone lifetime expires, the object is permanently purged.

> *Security relevance:* If a DC is offline for longer than tombstone lifetime and then reconnects, it cannot be safely reintroduced because it has objects that were deleted on other DCs. Attackers restoring old DCs could potentially resurface deleted accounts.

---

**AD-S20. What is Time Skew and how does it affect Kerberos?**

Kerberos tickets contain timestamps. To prevent replay attacks (reusing a captured ticket), Kerberos only accepts tickets where the timestamp is within **5 minutes** of the current time on the KDC.

If a client's clock is more than 5 minutes out of sync: Kerberos authentication fails entirely — users cannot log in.

> *Attack:* An attacker who can manipulate NTP on the network could cause widespread Kerberos failures — a denial of service. Also, attackers may try to extend ticket validity by manipulating time.

> *Why PDC Emulator matters:* It is the authoritative time source for the domain. All other DCs sync from it.

---

**AD-S21. What is Shadow Credentials?**

A technique that abuses the `msDS-KeyCredentialLink` attribute in AD — normally used for Windows Hello for Business (WHFB) passwordless authentication.

If an attacker has write access to a user's or computer's `msDS-KeyCredentialLink` attribute, they can add their own key to the target object. They can then authenticate as that object using their own certificate — without knowing the target's password.

> *Why it's dangerous:* It's very stealthy — no password change is made, and the account appears normal. The attacker just has a silent backdoor authentication path.

---

**AD-S22. What is Credential Guard?**

Windows Credential Guard uses virtualisation-based security (VBS) to isolate LSASS into a protected virtual environment called the **Isolated LSA**. Credentials stored in this protected space are inaccessible to the main OS — even to processes running as SYSTEM.

> *What it prevents:* Mimikatz-style `sekurlsa::logonpasswords` attacks — the credentials are in a separate virtual container that cannot be accessed from the normal OS. Pass-the-Hash and credential dumping from LSASS are significantly mitigated.

> *Requirements:* Needs hardware virtualisation, UEFI, Secure Boot. Windows 10+ Enterprise/Education.

---

**AD-S23. What is a Windows Access Token?**

When a user logs into Windows, the system creates an **Access Token** — an object containing:
- User's SID
- Group memberships
- Privileges
- Integrity level

Every process the user starts inherits this token. When a process tries to access a resource, Windows checks the access token against the resource's permissions.

> *Types:* **Primary token** (assigned to a process), **Impersonation token** (allows a process to act as another user).

---

**AD-S24. What is Token Impersonation?**

A technique where an attacker captures or duplicates another user's access token and uses it to execute code as that user — without needing their credentials.

> *Common tool:* Incognito (Metasploit), `getsystem` in Meterpreter. Works if the attacker's process has the `SeImpersonatePrivilege` or `SeAssignPrimaryTokenPrivilege`.

> *Example:* Attacker compromises an IIS web server (running as a service account with SeImpersonatePrivilege). They use a potato attack (Juicy Potato, Rogue Potato) to impersonate the SYSTEM token and escalate to SYSTEM.

---

**AD-S25. What is LSASS RunAsPPL?**

PPL (Protected Process Light) is a Windows feature that can protect the LSASS process by running it as a protected process. Other processes — even with SYSTEM privileges — cannot read or modify a PPL-protected LSASS.

> *How to enable:* Registry key: `HKLM\SYSTEM\CurrentControlSet\Control\Lsa` → `RunAsPPL = 1`

> *Effect on attackers:* Standard Mimikatz `sekurlsa::logonpasswords` fails because it cannot open a handle to PPL-protected LSASS. Attackers must use a kernel driver (e.g., mimidrv.sys) to remove the PPL protection first — which is much more detectable.

---

## PART 2: NETWORKING GAPS

---

**NET-S1. What is a subnet mask and why is it used?**

A subnet mask defines which part of an IP address is the **network** portion and which is the **host** portion.

> *Example:* IP `192.168.1.50` with mask `255.255.255.0` (/24)
> - Network: `192.168.1.0` (first 24 bits)
> - Host: `.50` (last 8 bits)
> - Valid hosts: `192.168.1.1` – `192.168.1.254`
> - Broadcast: `192.168.1.255`

**Purpose:** Determines whether a destination is on the local network (send directly) or remote (send to gateway). Essential for routing.

---

**NET-S2. What is CIDR and why is it used?**

CIDR (Classless Inter-Domain Routing) is a method for expressing IP address ranges using a prefix length instead of the old class-based system.

Format: `192.168.1.0/24` — the `/24` means 24 bits are the network portion.

| CIDR | Mask | Hosts |
|------|------|-------|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /30 | 255.255.255.252 | 2 |
| /16 | 255.255.0.0 | 65,534 |

> *Why:* CIDR replaced rigid Class A/B/C addressing, allowing efficient allocation of IP space and aggregating routes (route summarisation) to reduce routing table size.

---

**NET-S3. What is a Default Gateway?**

The default gateway is the router's IP address on the local network. When a device needs to send traffic to an IP address outside its own subnet, it sends the packet to the default gateway, which forwards it onward.

> *Example:* Your PC is `192.168.1.50/24`. Your router (gateway) is `192.168.1.1`. When you access `8.8.8.8` (Google), your PC sends the packet to `192.168.1.1` because `8.8.8.8` is not in your `/24` subnet.

---

**NET-S4. Static route vs dynamic routing?**

- **Static route** — Manually configured by an admin. Never changes unless manually updated. Simple, predictable, no overhead. Used for small networks or specific fixed paths.
- **Dynamic routing** — Routers automatically learn routes from each other using routing protocols (OSPF, BGP, EIGRP). Adapts to network changes automatically. Used for large or complex networks.

> *Security note:* Static routes cannot be poisoned by routing attacks. Dynamic routing protocols need authentication to prevent route injection.

---

**NET-S5. What is a Routing Loop?**

When packets circulate endlessly between routers because each router believes the next hop is "forward" — no router knows the correct path to the destination.

> *Example:* Router A sends to B, B sends to C, C sends back to A — packet loops forever.

> *Prevention:* TTL (Time to Live) — each router decrements TTL by 1. When TTL reaches 0, the packet is dropped. Routing protocols also have loop prevention mechanisms (split horizon, route poisoning).

---

**NET-S6. What is OSPF?**

OSPF (Open Shortest Path First) is a link-state interior routing protocol. Each router:
1. Discovers neighbors (Hello packets)
2. Shares its link-state information with all routers (LSA flooding)
3. Each router builds a complete map of the network
4. Runs Dijkstra's algorithm to calculate the shortest path to each destination

> *Used for:* Enterprise internal routing. Scales well, converges quickly.

---

**NET-S7. What is routing metric?**

A value used by routing protocols to determine the best path when multiple routes exist to the same destination. Lower metric = preferred path.

| Protocol | Metric Used |
|----------|------------|
| OSPF | Cost (based on bandwidth) |
| RIP | Hop count |
| EIGRP | Composite (bandwidth, delay, reliability, load) |
| BGP | AS path, local preference, MED |

---

**NET-S8. Access port vs Trunk port**

- **Access port** — Carries traffic for a single VLAN. End devices (PCs, printers) connect here. The device doesn't know about VLANs — the switch handles tagging.
- **Trunk port** — Carries traffic for multiple VLANs simultaneously using 802.1Q tagging. Used between switches, and between switches and routers.

> *Security note:* Access ports should be the default for end-user devices. Misconfigured trunk ports on end-user ports = VLAN hopping risk.

---

**NET-S9. What is Native VLAN?**

The VLAN for untagged traffic on a trunk port. Frames arriving on a trunk port without a VLAN tag are assigned to the native VLAN (default: VLAN 1).

> *Security risk:* VLAN 1 as native VLAN is dangerous. Double-tagging attacks use the native VLAN to hop to other VLANs. Best practice: change native VLAN to an unused VLAN ID and explicitly tag all traffic.

---

**NET-S10. What is STP (Spanning Tree Protocol)?**

STP prevents Layer 2 loops in switched networks with redundant links. Without it, a loop would cause broadcast storms (frames circulate infinitely, consuming all bandwidth).

**How it works:** STP elects a Root Bridge, then all other switches calculate the shortest path to the Root Bridge. Redundant ports are put into **Blocking** state.

**STP Port States:**
1. **Blocking** — Receives BPDUs only, no data forwarding
2. **Listening** — Processing BPDUs, participating in election
3. **Learning** — Building MAC address table, no forwarding yet
4. **Forwarding** — Normal operation — data passes through
5. **Disabled** — Administratively shut down

---

**NET-S11. How is the MAC address table built?**

When a frame arrives on a switch port, the switch reads the **source MAC address** and records it in the MAC table: "MAC address X is reachable via port Y."

When the switch needs to forward a frame:
- If it knows the destination MAC → forward to that specific port (unicast)
- If it doesn't know the destination MAC → flood to all ports except the incoming one

> *Security attack:* **MAC flooding** — attacker sends millions of frames with fake source MACs, filling the MAC table. The switch can no longer learn new entries and floods all traffic to all ports — the attacker can capture everything (like a hub).

---

**NET-S12. What is err-disabled?**

A switch port state where the port has been automatically disabled due to a security or error condition. The port shows `err-disabled` in `show interfaces`.

**Common causes:**
- Port Security violation (too many MACs or wrong MAC)
- BPDU Guard (STP BPDU received on an access port)
- Loop detected
- DHCP Snooping rate limit exceeded

> *Recovery:* `shutdown` then `no shutdown` on the port (or configure auto-recovery). Root cause must be fixed first.

---

**NET-S13. What do Ping, Traceroute, and Nslookup test?**

- **Ping** — Uses ICMP Echo Request/Reply. Tests basic Layer 3 reachability and round-trip time. "Can I get to this IP at all?"
- **Traceroute** — Uses increasing TTL values to identify each hop along the path to a destination. Shows where latency or loss is occurring. "What path does my traffic take?"
- **Nslookup** — Queries DNS to resolve hostnames to IPs and vice versa. Tests DNS functionality. "Is this domain resolving correctly?"

---

**NET-S14. DNS record types: A vs CNAME**

- **A record** — Maps a hostname directly to an IPv4 address. `www.example.com → 93.184.216.34`
- **AAAA record** — Maps a hostname to an IPv6 address.
- **CNAME record** — Alias — maps one hostname to another hostname. `ftp.example.com → www.example.com`. The final resolution follows the chain to the A record.

> *Key distinction:* You cannot point a CNAME to an IP. You cannot use a CNAME for the root domain (`example.com` itself) — only subdomains.

---

**NET-S15. 2.4 GHz vs 5 GHz Wi-Fi**

| | 2.4 GHz | 5 GHz |
|--|---------|-------|
| Range | Longer | Shorter |
| Speed | Slower | Faster |
| Penetration | Better through walls | Worse |
| Interference | More (microwaves, BT, other WiFi) | Less |
| Channels | 3 non-overlapping (1, 6, 11) | Many non-overlapping |

> *Security note:* Crowded 2.4 GHz environments are easier to disrupt and intercept. 5 GHz is generally more stable and less congested.

---

**NET-S16. WPA2 vs WPA3**

| Feature | WPA2 | WPA3 |
|---------|------|------|
| Auth protocol | PSK / 802.1X | SAE (Simultaneous Authentication of Equals) / 802.1X |
| Handshake | 4-way handshake | SAE (Dragonfly) |
| Offline dictionary attack | Vulnerable | Resistant |
| Forward Secrecy | No | Yes |
| Open network protection | No | OWE (Opportunistic Wireless Encryption) |

> *Key improvement:* WPA3 replaces the PSK 4-way handshake with SAE, which is resistant to offline dictionary attacks — even if an attacker captures the handshake, they can't brute-force it offline.

---

**NET-S17. What is LACP (Link Aggregation Control Protocol)?**

LACP (IEEE 802.3ad) is the standard protocol for automatically negotiating and managing Link Aggregation (bonding multiple physical links into one logical link).

**Benefits:**
- Increased bandwidth (combined throughput of all links)
- Redundancy (if one link fails, others carry the traffic)

> *Example:* Two 1 Gbps links aggregated = 2 Gbps logical link. If one fails, you lose half the bandwidth but not the connection.

---

**NET-S18. What is DHCP Snooping?**

A Layer 2 security feature on switches that prevents rogue DHCP servers. It:
1. Designates specific ports as "trusted" (where legitimate DHCP servers are connected)
2. Blocks DHCP Offer and ACK messages on "untrusted" ports

> *What it prevents:* Rogue DHCP server attacks where an attacker hands out malicious gateway/DNS settings to victims.

> *Secondary benefit:* Builds a binding table (IP-MAC-port mappings) used by Dynamic ARP Inspection (DAI) and IP Source Guard.

---

**NET-S19. What is DNS Spoofing?**

An attack where false DNS records are injected into a DNS resolver's cache — causing it to return malicious IP addresses for legitimate domain names.

> *Example:* Attacker poisons the DNS cache so `bank.com` resolves to the attacker's IP. Victim connects to the attacker's fake bank site. Combined with a valid-looking phishing page, credentials are captured.

> *Defence:* DNSSEC (cryptographically signs DNS responses), short TTLs on records, secure DNS resolvers.

---

**NET-S20. What is a Load Balancer and why is it used?**

A load balancer distributes incoming network traffic across multiple servers — preventing any single server from being overwhelmed.

**Benefits:** High availability, scalability, can route around failed servers.

**Types:**
- **Layer 4 (Transport)** — Distributes based on IP and TCP/UDP port
- **Layer 7 (Application)** — Can route based on URL path, HTTP headers, cookies — more intelligent

> *Security use:* Can offload TLS termination, apply WAF rules centrally, rate limit, and detect DDoS.

---

**NET-S21. What is MPLS?**

MPLS (Multiprotocol Label Switching) is a high-performance routing technique that directs traffic using short labels rather than long IP address lookups.

Instead of routing at each hop by examining the IP header, MPLS assigns a label at the entry point. Each router just reads the label and forwards — much faster.

> *Used by:* ISPs and large enterprise WAN networks. Provides QoS guarantees, traffic engineering, and VPN services over shared infrastructure.

---

**NET-S22. IPv4 vs IPv6**

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address size | 32-bit | 128-bit |
| Format | `192.168.1.1` | `2001:db8::1` |
| Address space | ~4.3 billion | 340 undecillion |
| Header | Variable, complex | Fixed 40-byte, simpler |
| NAT required? | Yes (address exhaustion) | No (enough addresses) |
| IPsec | Optional | Built-in |
| Broadcast | Yes | No (uses multicast) |
| Auto-config | DHCP | SLAAC or DHCPv6 |

---

**NET-S23. What is Anycast?**

One IP address is assigned to multiple servers in different locations. When a client sends traffic to that address, routing sends it to the **nearest** server (by routing metric).

> *Most famous use:* DNS root servers — there are only 13 root server IP addresses, but hundreds of physical machines globally all advertising those same IPs. Your query goes to the nearest one automatically. Also used by Cloudflare, Akamai for DDoS protection.

---

**NET-S24. Broadcast domain vs Collision domain**

- **Collision domain** — A network segment where frames can collide (only relevant in half-duplex, legacy shared media). Each switch port is its own collision domain. Hubs created one large collision domain.
- **Broadcast domain** — A network segment where a broadcast frame (destination MAC `FF:FF:FF:FF:FF:FF`) reaches all devices. Routers separate broadcast domains. VLANs also create separate broadcast domains on a switch.

> *Security relevance:* ARP operates within a broadcast domain. Keeping broadcast domains small limits the scope of ARP spoofing attacks.

---

**NET-S25. What is Loopback interface?**

A virtual network interface that always points back to the same device. The standard loopback IP is `127.0.0.1` (IPv4) or `::1` (IPv6) — also known as `localhost`.

**Uses:**
- Testing local network stack without physical interface
- On routers: a stable management IP that doesn't go down with a physical link failure
- Used as Router-ID in OSPF and BGP

---

**NET-S26. DHCP Options 3 and 6**

DHCP options are additional configuration parameters sent to clients alongside the IP address.

- **Option 3** — **Default Gateway** — tells the client which router to use
- **Option 6** — **DNS Servers** — tells the client which DNS servers to query

> *Security note:* A rogue DHCP server can provide malicious options — pointing clients to an attacker-controlled gateway (Option 3) or DNS server (Option 6) to intercept or redirect all their traffic.

---

**NET-S27. ICMP details**

ICMP (Internet Control Message Protocol) operates at **Layer 3** (Network), alongside IP.

**Purpose:** Error reporting and diagnostics — not for data transfer.

**Common messages:**
- Type 8 / Reply 0: Echo Request/Reply (Ping)
- Type 3: Destination Unreachable (with codes for host, port, network unreachable)
- Type 11: Time Exceeded (TTL expired — used by traceroute)
- Type 5: Redirect

**If ICMP is blocked:**
- Ping fails (no connectivity testing)
- Traceroute fails
- Path MTU discovery breaks (can cause silent connectivity issues)
- Some legitimate error reporting is lost

---

**NET-S28. "Request timed out" vs "Destination host unreachable"**

- **Request timed out** — ICMP Echo Request was sent, but no reply received within the timeout period. The packet may have been lost anywhere along the path, or ICMP is blocked at the destination. You don't know where it failed.

- **Destination host unreachable** — A router along the path explicitly replied with ICMP Type 3 (Destination Unreachable). A router knows the destination is unreachable and tells you. More informative — you know where it failed.

---

**NET-S29. VLAN 1 security risk**

VLAN 1 is the default VLAN on all Cisco switches. All ports start in VLAN 1 by default. It is also the default native VLAN on trunk ports.

**Security risks:**
- All switches communicate VLAN 1 management traffic by default (CDP, VTP, STP BPDUs)
- VLAN hopping double-tagging attacks require that the native VLAN matches the target — if native VLAN is VLAN 1 and you haven't changed it, attackers can hop into VLAN 1 from any access port

> *Best practice:* Move all user devices off VLAN 1, change native VLAN to an unused ID, explicitly allow VLANs on trunks.

---

**NET-S30. What is Duplex Mismatch?**

When two connected network devices are configured with different duplex settings — one at full-duplex, one at half-duplex.

**Result:** Significant performance degradation, high collision rates, intermittent connectivity. The full-duplex side continuously sends; the half-duplex side detects collisions and backs off.

> *Common cause:* Auto-negotiation failure. One side auto-negotiated, the other was manually set. Always manually set both sides to the same speed and duplex.

---

**NET-S31. DNS Round Robin**

A simple load distribution technique at the DNS level — returning multiple A records for the same hostname in a rotating order.

> *Example:*
> ```
> www.example.com → 192.168.1.10  (first query)
> www.example.com → 192.168.1.11  (second query)
> www.example.com → 192.168.1.12  (third query)
> ```

> *Limitation:* It's not true load balancing — it doesn't check if a server is healthy or how loaded it is. A proper load balancer is better for production.

---

**NET-S32. TCP Window Size and TTL**

**TCP Window Size:** How much data the receiver can accept before requiring an acknowledgment. A larger window = more data in flight = better throughput on high-latency links. Window scaling allows sizes beyond the original 65KB limit.

**TTL (Time to Live):** An IP header field decremented by 1 at each router hop. When TTL reaches 0, the router drops the packet and sends an ICMP "Time Exceeded" back to the sender. Default values: Windows=128, Linux=64, Cisco=255.

> *How traceroute uses TTL:* Sends packets with TTL=1, TTL=2, TTL=3... Each router that drops a packet sends back its IP. This reveals each hop in the path.

---

**NET-S33. Split Tunneling in VPN**

By default, a VPN routes all traffic through the VPN tunnel. **Split tunneling** sends only specific traffic (e.g., internal company resources) through the VPN tunnel, while other traffic (e.g., Netflix, general internet browsing) goes directly to the internet.

> *Security concern:* Split tunneling bypasses the corporate security stack for non-tunnelled traffic. A compromised endpoint could be a bridge from the internet to the internal network.

> *Benefit:* Reduces VPN bandwidth costs and improves performance for internet browsing.

---

**NET-S34. DHCP Relay**

When a DHCP server is not on the same subnet as the clients, DHCP Discover broadcasts can't reach it (routers don't forward broadcasts by default). A **DHCP Relay Agent** (usually a router) forwards DHCP broadcasts as unicast to the DHCP server.

> *How:* The relay agent receives the broadcast Discover, adds its own IP (the giaddr field), and forwards it unicast to the configured DHCP server. The server sees the giaddr and knows which scope to assign from.

---

**NET-S35. What is BGP?**

BGP (Border Gateway Protocol) is the routing protocol that runs the internet. It is an **Exterior Gateway Protocol** (EGP) used to exchange routing information between autonomous systems (ASes) — large networks operated by ISPs, companies, and organisations.

> *Every ISP, cloud provider, and large company has an AS number.* BGP makes the routing decisions about how packets travel across the global internet.

> *Security concern:* **BGP hijacking** — a malicious AS announces routes for IP prefixes it doesn't own, intercepting traffic. This has been used to intercept cryptocurrency transactions and government communications.

---

**NET-S36. ICMP Redirect**

An ICMP message (Type 5) sent by a router to a host saying "there's a better route to this destination — use this gateway instead of me."

> *Security risk:* ICMP Redirect messages can be spoofed to reroute traffic through an attacker's machine — a MITM setup. Most modern operating systems ignore ICMP Redirects by default for this reason.

---

**NET-S37. ARP Request vs ARP Reply**

- **ARP Request** — Broadcast to all devices on the subnet: "Who has IP 192.168.1.1? Tell 192.168.1.50." All devices receive it, only the owner responds.
- **ARP Reply** — Unicast from the owner: "I have 192.168.1.1 — my MAC is AA:BB:CC:DD:EE:FF." Sent directly back to the requester.

---

**NET-S38. What is QoS (Quality of Service)?**

QoS is a set of techniques for managing network traffic to prioritise certain types of traffic over others — ensuring time-sensitive applications (VoIP, video calls) get the bandwidth and low latency they need.

> *Example:* In a company network, QoS marks VoIP packets as high-priority. During congestion, routers forward VoIP packets first before bulk file transfers — preventing choppy calls.

> *Methods:* Traffic classification (DSCP marking), queuing (priority queues), traffic shaping, policing.

---

**NET-S39. What is MTU (Maximum Transmission Unit)?**

MTU is the largest packet size that can be transmitted over a network segment without fragmentation. Standard Ethernet MTU is 1500 bytes.

**Problems from MTU mismatch:**
- If a packet is larger than the path's MTU and "Don't Fragment" is set → packet dropped, communication silently fails
- VPN tunnels add headers, reducing effective MTU — can cause VPN connectivity issues

> *Diagnosis:* "Ping works but HTTPS doesn't" or "VPN connects but traffic doesn't pass" often indicates an MTU problem.

---

## PART 3: CLOUD GAPS

---

**CLOUD-S1. Scalability vs Elasticity**

- **Scalability** — The ability of a system to handle increased load by adding resources. Can be vertical (bigger machine) or horizontal (more machines). Often a manual or planned process.
- **Elasticity** — The ability to automatically scale resources up and down in real time based on current demand — and then release those resources when no longer needed.

> *Example:* An e-commerce site with elasticity automatically adds 20 servers on Black Friday afternoon and removes them at midnight. Scalability is the capability; elasticity is the automatic, dynamic expression of it.

---

**CLOUD-S2. Why is virtualisation important for cloud?**

Virtualisation allows a single physical server to run multiple isolated virtual machines simultaneously. This is the foundational technology for cloud because it enables:
- **Multi-tenancy** — Multiple customers on the same hardware, fully isolated
- **Resource efficiency** — Physical hardware is shared, not idle
- **Rapid provisioning** — New VMs start in seconds
- **Portability** — VMs can be moved between physical hosts

---

**CLOUD-S3. Region vs Availability Zone**

- **Region** — A geographic area containing cloud infrastructure. Example: `eu-west-1` (Ireland), `us-east-1` (Virginia). Regions are completely independent — a failure in one doesn't affect another.
- **Availability Zone (AZ)** — Isolated data centres within a region. Connected by high-speed links but physically separate (different power, cooling, networking). Multiple AZs provide high availability within a region.

> *Best practice:* Deploy across at least 2 AZs for high availability. Deploy across regions for disaster recovery.

---

**CLOUD-S4. Multi-tenant architecture**

Multiple customers (tenants) share the same physical infrastructure (compute, storage, network) but are logically isolated from each other. They cannot see or access each other's data.

> *Security concern:* Tenant isolation failure (VM escape, side-channel attacks) could expose one customer's data to another. Cloud providers invest heavily in hypervisor security and hardware isolation.

---

**CLOUD-S5. Cloud storage tiers: Hot, Cool, Archive**

| Tier | Access frequency | Cost (storage) | Cost (retrieval) | Use case |
|------|-----------------|----------------|-----------------|---------|
| **Hot** | Frequent | High | Low | Active data, production |
| **Cool** | Infrequent | Medium | Medium | Backups, rarely accessed |
| **Archive** | Very rare | Very low | High + delayed | Long-term retention, compliance |

> *Example:* Current year's customer data = Hot. Last year's = Cool. 7-year compliance archive = Archive tier.

---

**CLOUD-S6. Serverless computing**

A cloud execution model where the cloud provider manages all server infrastructure. You deploy code (functions), and the provider:
- Allocates resources automatically when triggered
- Scales to zero when not in use
- Bills only for actual execution time

> *Examples:* AWS Lambda, Azure Functions, Google Cloud Functions.

> *Security considerations:* Still runs on servers — the name is from the developer's perspective. Risks include function injection, over-permissive IAM roles, insecure dependencies in function packages.

---

## PART 4: GENERAL SECTION GAPS

---

**GEN-S1. What is a CAM table?**

CAM (Content Addressable Memory) table is what vendors more generically call the MAC address table. It stores the mapping of MAC addresses to switch ports. "CAM table" is the Cisco-specific term for the hardware that stores these mappings — accessed very quickly by dedicated hardware rather than software lookup.

---

**GEN-S2. What is MAC flooding?**

An attacker sends enormous volumes of Ethernet frames with random fake source MAC addresses. The switch's MAC table (CAM table) fills up with fake entries. Once full, the switch can no longer learn new MAC addresses and falls back to flooding all traffic on all ports — behaving like a hub.

> *Result:* Attacker can now capture all traffic on the network segment using a packet analyser.

> *Defence:* Port Security — limits the number of MAC addresses allowed per switch port.

---

**GEN-S3. OSI Layer Attacks — one example per layer**

| Layer | Name | Attack Example |
|-------|------|---------------|
| L1 Physical | Physical | Cable cutting, hardware keylogger, RF jamming |
| L2 Data Link | Data Link | ARP Spoofing, MAC Flooding, VLAN Hopping |
| L3 Network | Network | IP Spoofing, ICMP Redirect, routing attacks |
| L4 Transport | Transport | SYN Flood, port scanning, TCP session hijacking |
| L5 Session | Session | Session hijacking, replay attacks |
| L6 Presentation | Presentation | SSL stripping, malformed data attacks |
| L7 Application | Application | SQL Injection, XSS, DNS spoofing, phishing |

---

**GEN-S4. What is IP Spoofing?**

Sending network packets with a forged source IP address — impersonating another host or hiding the real origin.

> *Uses:* DDoS amplification attacks (send request with victim's IP as source → amplified response floods the victim), bypassing IP-based access controls, blind TCP attacks.

> *Defence:* Ingress filtering (BCP38) — ISPs and network edges should drop packets with source IPs that couldn't legitimately come from that network.

---

**GEN-S5. What is DHCP Spoofing?**

An attacker sets up a rogue DHCP server on the network that responds to DHCP requests faster than the legitimate server. Clients receive IP configuration from the attacker, who can:
- Set a malicious default gateway (DHCP Option 3) → MITM all traffic
- Set a malicious DNS server (DHCP Option 6) → redirect all DNS queries

> *Defence:* DHCP Snooping on switches — only trust DHCP Offer/ACK from designated trusted ports.

---

**GEN-S6. What is Rate Limiting?**

Controlling how many requests a user or IP address can make within a time window — preventing abuse, brute force, and DoS.

> *Examples:*
> - Login endpoint: max 5 attempts per minute per IP
> - API: max 1,000 requests per hour per API key
> - Email sending: max 100 emails per hour per account

> *Security use:* Rate limiting is a primary defence against brute force attacks and API abuse.

---

**GEN-S7. L3 Switch vs Router — what's the difference?**

Both operate at Layer 3 and can route traffic between subnets, but:

| | L3 Switch | Router |
|--|-----------|--------|
| **Primary role** | High-speed inter-VLAN routing within LAN | WAN connectivity and complex routing |
| **Speed** | Hardware-based (ASIC) — very fast | Software-based — slower |
| **Features** | Limited WAN protocols | Full WAN support (PPPoE, MPLS, etc.) |
| **Use case** | Campus/enterprise core switching | Internet edge, WAN connectivity |

> *Simple answer:* An L3 switch is a switch that can also route. A router is primarily for routing, especially at the WAN edge.

---

**GEN-S8. SSL Stripping vs SSL Downgrade**

- **SSL Stripping** — A MITM attack where the attacker intercepts an HTTPS connection and forwards the request to the server over HTTPS, but serves the victim over HTTP. The victim gets an unencrypted connection without realising it — they might not notice the missing padlock.

- **SSL/TLS Downgrade Attack** — The attacker manipulates the TLS handshake to force both parties to negotiate a weaker cipher suite or older protocol version (e.g., SSLv3, TLS 1.0) that has known vulnerabilities. POODLE and BEAST are examples.

> *Defence:* HSTS (HTTP Strict Transport Security) prevents stripping — browser refuses HTTP. Disable old TLS versions (1.0, 1.1) and weak cipher suites to prevent downgrade.

---

**GEN-S9. DLL Hijacking vs DLL Injection**

- **DLL Hijacking** — Exploiting Windows DLL search order. Attacker places a malicious DLL in a location the application searches before the legitimate path. Application loads the malicious DLL instead.

- **DLL Injection** — Forcefully inserting a malicious DLL into a running process's memory space — using Windows APIs like `CreateRemoteThread` + `LoadLibrary`. Requires a handle to the target process.

> *Key difference:* Hijacking is passive — you wait for the app to load. Injection is active — you force a running process to load your code.

---

**GEN-S10. What is the equivalent of DLL in Linux?**

**Shared Object files (.so)** — the Linux equivalent of Windows DLLs. They use the `.so` extension (e.g., `libc.so.6`).

The dynamic linker (`ld.so`) loads them at runtime. The search path is controlled by `LD_LIBRARY_PATH` and `/etc/ld.so.conf`.

> *Attack:* **LD_PRELOAD hijacking** — setting the `LD_PRELOAD` environment variable to a malicious `.so` file causes it to be loaded before all other libraries, allowing you to override any function. A classic privilege escalation technique on Linux.

---

**GEN-S11. What is a Vulnerability Vector (Attack Vector)?**

The attack vector (in CVSS scoring) describes the context in which exploitation occurs:

- **Network (N)** — Exploitable remotely over the internet. Highest severity impact.
- **Adjacent (A)** — Requires access to the same network (LAN, Bluetooth, Wi-Fi).
- **Local (L)** — Requires local OS access or physical access.
- **Physical (P)** — Requires physical contact with the device.

> *Example:* A buffer overflow in a web-exposed service = Network vector (most dangerous). A privilege escalation requiring local login = Local vector.

---

**GEN-S12. Digital Forensics Steps**

Standard digital forensics methodology:

1. **Identification** — Identify potential evidence sources (devices, logs, cloud)
2. **Preservation** — Secure the scene, prevent evidence tampering (legal hold, image before touching)
3. **Collection** — Acquire evidence following order of volatility (RAM first, then disk)
4. **Examination** — Process and filter collected data
5. **Analysis** — Draw conclusions from evidence — timeline reconstruction, attribution
6. **Reporting** — Document findings in a clear, court-admissible format
7. **Presentation** — Present findings to relevant parties

> *Critical throughout:* Maintain chain of custody at every step.

---

**GEN-S13. What is the Pyramid of Pain?**

A threat intelligence framework by David Bianco describing the relative difficulty for attackers when defenders detect and block different types of indicators:

| Level | Indicator Type | Pain to Attacker |
|-------|---------------|-----------------|
| Top | **TTPs** (Tactics, Techniques, Procedures) | Very High — must change how they operate |
| ↑ | **Tools** | High — must find new tools |
| ↑ | **Network/Host Artefacts** | Annoying — must recompile/reconfigure |
| ↑ | **Domain Names** | Simple — register new domain |
| ↑ | **IP Addresses** | Easy — change IP |
| Bottom | **Hash values** | Trivial — change one byte |

> *Key insight:* Blocking file hashes (bottom) is the lowest-value defence. Detecting and disrupting attacker TTPs (top) forces them to fundamentally retool — the most effective defence.

---

**GEN-S14. What are TTPs (and what is meant by "PPT")?**

**TTPs** = Tactics, Techniques, and Procedures:
- **Tactics** — The adversary's goal at a given stage. (e.g., "Lateral Movement")
- **Techniques** — How they achieve that goal. (e.g., "Pass the Hash")
- **Procedures** — The specific implementation detail. (e.g., using Mimikatz's specific command)

> *Note on "PPT":* This is likely a typo/mistranslation of "TTP" or "IoC" in your question bank. TTPs are what MITRE ATT&CK catalogues.

---

**GEN-S15. What are DNS attack types?**

| Attack | Description |
|--------|-------------|
| **DNS Spoofing/Cache Poisoning** | Injecting false records into a resolver cache |
| **DNS Tunneling** | Encoding data in DNS queries for covert C2 or exfil |
| **DNS Amplification** | DDoS — small query to open resolver returns large response to spoofed victim IP |
| **DNS Hijacking** | Redirecting DNS queries to malicious servers (via router compromise, malware, ISP) |
| **NXDOMAIN Attack** | Flooding a DNS server with queries for non-existent domains |
| **Zone Transfer Abuse** | Requesting AXFR to enumerate all DNS records in a zone |
| **Subdomain Takeover** | Claiming an abandoned subdomain's cloud resource (dangling CNAME) |

---

**GEN-S16. TCP Session Hijacking**

An attacker takes over an established TCP session between two parties by:
1. Monitoring a session to learn sequence numbers
2. Injecting packets with the correct sequence numbers, impersonating one of the parties
3. The legitimate party is desynchronised and kicked out

> *Modern protection:* TLS encrypts and authenticates the session — even if TCP sequence numbers are guessed, the TLS encryption prevents content injection. Session hijacking is largely a concern in cleartext protocols.

---

**GEN-S17. What is a DHCP Starvation Attack?**

An attacker sends thousands of DHCP Discover messages with spoofed MAC addresses. The DHCP server allocates an IP for each one — exhausting its entire IP address pool (scope). Legitimate clients can no longer obtain IP addresses and cannot connect to the network.

> *Defence:* DHCP Snooping with rate limiting — limits the number of DHCP messages per second per port.

---

**GEN-S18. SSL/TLS Handshake steps**

Simplified TLS 1.3 handshake:

1. **ClientHello** — Client sends supported TLS versions, cipher suites, and a random value
2. **ServerHello** — Server selects cipher suite, sends certificate and its random value
3. **Certificate Verification** — Client validates the server's certificate against trusted CAs
4. **Key Exchange** — Both sides use Diffie-Hellman to derive the same session key
5. **Finished** — Both sides confirm the handshake with a MAC — encryption starts
6. **Application Data** — All further communication is encrypted with symmetric encryption

---

**GEN-S19. TCP 4-Way Termination Handshake**

How a TCP connection is gracefully closed:

1. **FIN** — Initiator says "I'm done sending"
2. **ACK** — Receiver acknowledges
3. **FIN** — Receiver says "I'm done sending too"
4. **ACK** — Initiator acknowledges

> *Why 4 steps:* TCP is full-duplex — each direction is closed independently. After step 2, the receiver can still send data before closing its side (step 3).

---

**GEN-S20. WiFi WPA 4-Way Handshake**

A different 4-way handshake — for WPA/WPA2 wireless authentication between a client and access point, deriving the Pairwise Transient Key (PTK) for encrypting traffic:

1. **ANonce** — AP sends a random nonce (ANonce) to the client
2. **SNonce + MIC** — Client sends its own nonce (SNonce) and a Message Integrity Code computed with the PMK (from the WiFi password)
3. **GTK + MIC** — AP sends the Group Transient Key (for broadcast traffic) and confirms
4. **ACK** — Client confirms installation of the keys

> *Attack:* The 4-way handshake can be captured when a client connects. The captured handshake can then be brute-forced offline (PMKID attack or traditional capture-and-crack with tools like hashcat + aircrack). WPA3 replaces this with SAE which is resistant to offline attacks.

---

**GEN-S21. What is CASB (Cloud Access Security Broker)?**

A CASB is a security policy enforcement point between users and cloud services — providing visibility and control over cloud application usage.

**Four pillars:**
- **Visibility** — Discover what cloud apps are being used (Shadow IT)
- **Compliance** — Enforce data compliance policies in cloud
- **Data Security** — Apply DLP policies to cloud data
- **Threat Protection** — Detect anomalous behaviour in cloud apps

> *Examples:* Microsoft Defender for Cloud Apps, Netskope, Zscaler.

---

**GEN-S22. DLP + CASB Integration and First Steps**

**How they work together:** DLP focuses on data in motion (network) and at rest (endpoint). CASB extends DLP controls into cloud applications — so when a user uploads a sensitive file to Dropbox or Google Drive, CASB enforces the DLP policy.

**When setting up DLP — first steps:**
1. **Discovery** — Identify and classify sensitive data (where is it? what is it?)
2. **Policy definition** — Define what "sensitive" means for your organisation (PII, financial data, IP)
3. **Monitor mode first** — Deploy in monitor-only mode to understand traffic before blocking
4. **Tune** — Reduce false positives before switching to block mode
5. **Enforce** — Gradually enforce policies, starting with highest-risk channels

---

**GEN-S23. What is SMB and Samba?**

- **SMB (Server Message Block)** — A Microsoft network protocol for file sharing, printer sharing, and inter-process communication. Runs on port 445 (modern) and 139 (legacy NetBIOS). Used across all Windows environments.

- **Samba** — A free, open-source implementation of SMB for Linux/Unix systems. Allows Linux servers to appear as Windows file servers — sharing files and printers with Windows clients and joining Windows domains.

> *Security history:* SMBv1 was exploited by EternalBlue (WannaCry, NotPetya). Always disable SMBv1. Enable SMB Signing to prevent relay attacks.

---

**GEN-S24. Microsoft System Hardening Steps**

A structured approach to securing a Windows system:

**Account security:** Disable/rename built-in Administrator, disable Guest account, enforce strong password policy, implement MFA, use least privilege

**Services:** Disable unnecessary services (Telnet, FTP, SNMP), disable SMBv1, disable LLMNR and NetBIOS over TCP/IP

**Updates:** Enable automatic updates, patch regularly, use WSUS for enterprise

**Firewall:** Enable Windows Defender Firewall, restrict inbound/outbound rules

**Auditing:** Enable detailed audit policies (logon, object access, process creation), configure Security log size, forward logs to SIEM

**Endpoint protection:** Deploy EDR, enable Credential Guard, enable LSASS RunAsPPL, configure AppLocker/WDAC for application control

**Network:** Disable RDP if not needed, enable NLA for RDP, restrict RDP to specific IPs, enable SMB Signing

---

**GEN-S25. What is the Unified Kill Chain?**

An expanded attack model combining the Cyber Kill Chain and MITRE ATT&CK into 18 phases — organised in three cycles:

**Cycle 1 — Initial Foothold:** Reconnaissance → Weaponisation → Social Engineering → Exploitation → Persistence → Defence Evasion → C2

**Cycle 2 — Network Propagation:** Pivoting → Discovery → Privilege Escalation → Execution → Credential Access → Lateral Movement

**Cycle 3 — Action on Objectives:** Collection → Exfiltration → Impact → Objectives

> *Value over Cyber Kill Chain:* More granular, reflects real-world attack complexity, maps directly to MITRE ATT&CK. Better for both red and blue team planning.

---

**GEN-S26. What is a Watering Hole Attack?**

An attacker compromises a website frequently visited by their target audience. When the targets visit the trusted website, they are infected.

> *Example:* An APT group wants to target financial analysts. Instead of spear-phishing them directly, they compromise a financial industry news site. Analysts visit it daily — they're infected by the compromised site.

> *Why effective:* The target visits a site they already trust. No suspicious email needed. The "watering hole" metaphor: predators wait at the watering hole where prey must come to drink.

---

**GEN-S27. Phishing Analysis — Step by Step**

When you receive a suspicious email for analysis:

1. **Don't click anything** — open in a safe analysis environment (VM, sandboxed email client)
2. **Check email headers** — `Received` headers show true path, check `Reply-To` vs `From`, check `X-Originating-IP`
3. **SPF/DKIM/DMARC check** — did authentication pass? (Look for `Authentication-Results` header)
4. **Analyse sender** — does the From domain match the claimed organisation? Lookalike domains?
5. **Extract URLs** (without clicking) — use CyberChef, defang them (`http[s]://evil[.]com`). Check with VirusTotal, URLScan.io
6. **Analyse attachments** — hash them, check VirusTotal. Use sandbox (Any.run, Joe Sandbox) for dynamic analysis
7. **Check for credential harvesting** — does the linked page mimic a login form?
8. **Assess scope** — who received this? Did anyone click? Check email gateway logs
9. **Remediation** — block sender domain, pull the email from all mailboxes, reset credentials if any victims clicked
10. **Report** — document findings, share IoCs with threat intel feeds

---

**GEN-S28. Port Forwarding Types**

- **Local Port Forwarding** — Forwards a port on your local machine to a remote destination through an SSH tunnel. `ssh -L 8080:internalserver:80 jump-host` — browse `localhost:8080` to reach the internal server.
- **Remote Port Forwarding** — Exposes a local port on a remote server. `ssh -R 9090:localhost:22 remote-server` — anyone connecting to remote-server:9090 reaches your local port 22.
- **Dynamic Port Forwarding** — Creates a SOCKS proxy. `ssh -D 1080 remote-server` — routes traffic through the remote server via SOCKS.

> *Security use:* SSH tunneling is used both legitimately (accessing internal resources) and by attackers (tunneling C2 traffic through SSH to bypass firewalls).

---

**GEN-S29. Birthday Attack**

A cryptographic attack exploiting the Birthday Problem in probability — the counterintuitive fact that in a group of just 23 people, there's a >50% chance two share a birthday.

Applied to hashing: in any set of 2^(n/2) hash values, you have a >50% chance of finding a collision (two inputs producing the same hash). For a 128-bit hash (MD5), this means ~2^64 operations — far less than the expected 2^128.

> *Practical impact:* This is why MD5 and SHA-1 are broken for collision resistance. SHA-256's 256-bit output means birthday attacks require 2^128 operations — computationally infeasible.

---

**GEN-S30. VLAN Types**

- **Default VLAN** — VLAN 1 — all ports belong here by default, carries untagged management traffic
- **Data VLAN** — Carries user-generated traffic, separates different user groups
- **Voice VLAN** — Dedicated VLAN for VoIP traffic, configured with QoS priority to ensure call quality
- **Management VLAN** — Dedicated VLAN for network device management traffic (SSH, SNMP, HTTPS to switch management)
- **Native VLAN** — Untagged VLAN on trunk ports

---

**GEN-S31. What is a Syslog Server?**

Syslog is a standard protocol for sending log messages from network devices (routers, switches, firewalls, servers) to a central logging server.

**Port:** UDP 514 (standard, no authentication, no reliability) — or TCP 514 / TCP 6514 (TLS) for reliable/encrypted transmission.

> *Security use:* Centralising logs to a syslog server means attackers can't cover their tracks by clearing logs on individual devices — the logs are already at the central server. This is the foundation of a SIEM.

---

**GEN-S32. NTLM v1 vs v2 — why was it upgraded?**

**NTLMv1 weaknesses:**
- Uses a weak challenge-response based on DES encryption
- Vulnerable to pass-the-hash, relay attacks, and faster offline cracking
- Challenge is only 8 bytes — limited entropy

**NTLMv2 improvements:**
- Uses HMAC-MD5 instead of DES
- Client also sends its own timestamp and nonce (client challenge) — prevents certain replay attacks
- The response is tied to both server and client nonces — harder to relay
- Longer, more complex response

> *Still weak compared to Kerberos:* NTLMv2 is better than v1 but still vulnerable to relay attacks (mitigated by SMB Signing) and offline cracking. Kerberos should be preferred where possible.

---

**GEN-S33. What is the Responder tool?**

Responder is a tool that poisons LLMNR, NetBIOS Name Service (NBT-NS), and mDNS queries on a local network. When a Windows machine fails to resolve a hostname via DNS, it broadcasts a query via LLMNR/NBT-NS — Responder answers all of these, redirecting the authentication attempt to itself and capturing the Net-NTLMv2 hash.

> *Typical use:* Run Responder on the network → wait passively → collect hashes from Windows machines making typos or browsing → crack with Hashcat or relay with ntlmrelayx.

---

**GEN-S34. ICMPv4 vs ICMPv6**

| Feature | ICMPv4 | ICMPv6 |
|---------|--------|--------|
| IP version | IPv4 | IPv6 |
| Neighbour discovery | ARP (separate protocol) | Built into ICMPv6 (NDP replaces ARP) |
| Router discovery | Separate (RARP, BOOTP) | Built into ICMPv6 (Router Solicitation/Advertisement) |
| Multicast management | IGMP (separate) | MLD (built into ICMPv6) |
| Error messages | Yes | Yes (similar types) |
| Path MTU discovery | Optional | Required |

> *Key point:* ICMPv6 is far more important than ICMPv4 — it handles functions that were separate protocols in IPv4 (ARP, router discovery, multicast management). Blocking ICMPv6 entirely breaks IPv6 networking.

---

**GEN-S35. NetBIOS — detailed**

NetBIOS (Network Basic Input/Output System) is a legacy API/protocol for name resolution and service discovery on local Windows networks, predating DNS. It operates at Layer 5 (Session layer).

**Three services:**
- **NBT-NS (NetBIOS Name Service)** — Port 137 UDP — resolves NetBIOS names to IPs
- **NBT-DGM (Datagram)** — Port 138 UDP — connectionless messaging
- **NBT-SS (Session Service)** — Port 139 TCP — connection-oriented communication

> *Security problem:* NBT-NS broadcasts name queries that any host can answer — exploited by Responder for credential capture. Should be disabled via GPO on modern networks that use DNS exclusively.

---

**GEN-S36. SMTP, POP3, IMAP**

| Protocol | Port | Purpose |
|----------|------|---------|
| **SMTP** | 25 (server-to-server), 587 (client submission), 465 (SMTPS) | Sending email |
| **POP3** | 110 (plain), 995 (SSL) | Receiving email — downloads to local client, deletes from server |
| **IMAP** | 143 (plain), 993 (SSL) | Receiving email — syncs with server, keeps on server |

> *Key differences:* POP3 is one-device — download and gone from server. IMAP keeps email on the server and syncs across multiple devices. Modern email uses IMAP or HTTPS-based access.

> *Security:* All plain (non-SSL) versions transmit credentials in cleartext. Always use SSL/TLS variants.

---

**GEN-S37. DTP (Dynamic Trunking Protocol)**

A Cisco-proprietary protocol that automatically negotiates trunk links between switches. By default, many Cisco ports are in "dynamic auto" or "dynamic desirable" mode — they'll automatically form a trunk if the other end requests it.

> *Security risk:* An attacker can send DTP frames from an access port and negotiate a trunk link — gaining access to all VLANs. This is the initial step in some VLAN hopping attacks.

> *Remediation:* Disable DTP on all access ports: `switchport nonegotiate` and explicitly configure mode: `switchport mode access`.

---

**GEN-S38. SELinux**

SELinux (Security-Enhanced Linux) is a Linux kernel security module implementing **Mandatory Access Control (MAC)**. Developed by the NSA.

Unlike standard Linux permissions (DAC — Discretionary Access Control) which allow file owners to grant access to anyone, SELinux applies policies that even root cannot override.

**Modes:**
- **Enforcing** — Active, enforces policy, blocks and logs violations
- **Permissive** — Logs what would be blocked but doesn't actually block — used for testing/debugging
- **Disabled** — Off

**Contexts:** Every file, process, and port gets a security context label: `user:role:type:level`. Access decisions are based on these contexts, not just ownership.

> *Example:* Even if Apache is running as root, SELinux policy prevents it from reading `/etc/shadow` — a label-based restriction that standard Unix permissions wouldn't stop.

---

**GEN-S39. Hash Cracking Tools**

**Online (lookup tables / rainbow tables):**
- **CrackStation.net** — Large precomputed hash database
- **hashes.com** — Hash lookup and cracking service
- **MD5Decrypt.net** — MD5-specific lookups

**Command-line:**
- **Hashcat** — GPU-accelerated password recovery. Extremely fast. Supports all major hash types. `hashcat -m 1000 hash.txt wordlist.txt` (NTLM cracking)
- **John the Ripper** — CPU-based password cracker. Good for many formats, built into Kali. `john --wordlist=rockyou.txt hash.txt`
- **Hydra** — Online brute-force tool for live services (SSH, RDP, web forms) — not offline hash cracking

---

**GEN-S40. LLMNR — detailed**

LLMNR (Link-Local Multicast Name Resolution) is a protocol that allows name resolution on a local network when DNS fails. It is Microsoft's replacement for NetBIOS name service.

**How it works:** When DNS fails to resolve a name, Windows sends an LLMNR multicast query to `224.0.0.251:5355` — any machine on the subnet can respond.

**The attack:** Responder listens for LLMNR queries and answers them all: "Yes, I'm whatever you're looking for." The victim then authenticates to Responder, which captures the Net-NTLMv2 hash.

> *Why it's still prevalent:* Enabled by default in all Windows versions. Common when users make typos in UNC paths or when DNS isn't configured correctly. Easy to exploit, passive (just listen).

> *Disable via GPO:* Computer Configuration → Administrative Templates → Network → DNS Client → Turn off Multicast Name Resolution → Enabled.

---

*End of Supplementary Guide*
