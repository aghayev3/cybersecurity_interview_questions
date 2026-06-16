# SOC L1 — The Interview Edge (One-Pager)

*Read this on the way in. Nothing here is out of L1 scope — it's all detection/triage depth, framed to make you sound like someone who's actually worked a SIEM.*

---

## ★ The one move that wins: tell the unified attack story

When asked anything open-ended ("walk me through an attack," "how do these connect"), don't recite topics — narrate **one kill chain** and name the detection at each stage. This shows kill chain + MITRE + event-ID fluency + triage instinct in a single answer.

1. **Delivery** — phishing email, macro invoice.
2. **Execution** — `WINWORD.EXE` spawns `powershell -enc …`. → *Sysmon EID 1, parent=Office = high-fidelity; decode the Base64.*
3. **C2** — download cradle pulls a beacon, calls home. → *EID 3 (network), EID 22 (DNS — long/high-entropy subdomains = tunneling).*
4. **Persistence** — scheduled task / Run key. → *4698, Sysmon EID 13.*
5. **Credential Access** — Mimikatz dumps LSASS. → *Sysmon EID 10 targeting lsass.exe.*
6. **Lateral Movement** — Pass-the-Hash. → *4624 Type 3 + NTLM from odd host; PsExec service = 7045.*
7. **Discovery / Priv-Esc** — BloodHound LDAP recon (T1087); Kerberoasting. → *4769 + RC4 (0x17).*
8. **Domain Dominance** — DCSync pulls krbtgt → Golden Ticket. → *4662 + replication GUID from a non-DC.*
9. **Actions on Objectives** — exfil / ransomware.
10. **Anti-forensics** — clears the log (1102) → *"but it doesn't blind me — logs forward to the SIEM in real time."*

**Closing line:**
> "Phishing, PowerShell, credential theft, lateral movement, AD attacks, DNS — they're not separate topics, they're one kill chain. The L1 job is to catch it as early as possible, because every stage you detect earlier saves you the next one."

---

## ★ The concept that sounds senior: Pyramid of Pain

Use when discussing IOCs vs IOAs or detection strategy. Indicators ranked by how hard they are for an attacker to change:

`hash → IP → domain → host/network artifact → tool → TTP`

Bottom (hash, IP) = trivial for attacker to swap, so blocking it barely slows them. Top (**TTP / behavior**) = expensive to change.

> "I'd rather detect the *technique* than the hash — the hash changes every build, but the behavior doesn't. That's why Office-spawning-PowerShell beats a single file hash, and why IOAs beat IOCs."

---

## ★ The "I've actually done this" tells

Drop these naturally — each signals hands-on experience:

- "I check the **parent process** before I even read the alert detail."
- Kerberoasting: "I'd look for **RC4, encryption type 0x17**, in the 4769s."
- Clicked link: "Reset the password **and revoke active sessions** — a reset alone won't kill a live token."
- Golden Ticket fix: "Reset **krbtgt twice**, with a replication interval between."
- Mindset: "I **assume breach** — the question isn't *if* something got in, it's whether I can see it."
- Triage shape: "Brute force is many passwords on one account; **spraying** is one password across many — different 4625 pattern."

---

## ★ True/False Positive & Negative — say this if asked

| | Malicious | Benign |
|---|---|---|
| Alert | True Positive ✅ | False Positive (noise) |
| No alert | **False Negative ⚠ worst** | True Negative ✅ |

> "False negatives are the most dangerous — they're invisible. That's why we tolerate *some* false positives rather than tuning so hard we create blind spots."

---

## ★ Turn it around — questions to ask THEM

Ask 2–3. Makes you sound like an operator, not a test-taker:

- "What's your SIEM and EDR stack?"
- "How are L1 and L2 split — what's the escalation threshold?"
- "Do you write detections in **Sigma**, and can L1 suggest tuning?"
- "How do you measure SOC success — **MTTD / MTTR**?"

---

## ★ The L1 triage workflow (fall back on this for ANY scenario)

**Validate** (TP/FP?) → **Scope** (isolated or pattern?) → **Enrich** (threat intel, asset criticality) → **Decide** (close + tune, or escalate) → **Document** (timeline, IOCs, recommendation).

> "When in doubt on a critical asset, I escalate."

---

## ⚠ Know where your lane ends (this is itself impressive)

Keep everything at **detection + triage**. If pushed toward malware reverse-engineering or leading IR:

> "I'd preserve the evidence and escalate to L2 with my timeline and IOCs."

Knowing the boundary signals maturity. Pretending you'd solo an incident response is a red flag.

---

## The 3 gotchas — don't get caught

1. **Ping = ICMP** (not TCP/UDP, no ports).
2. **Kerberos uses the password once** (at the AS for the TGT), then tickets — not per request.
3. **LDAP is a directory protocol**, not auth — it only *supports* auth via bind.

*Be able to explain WHY each is right — that's the difference between understanding and memorizing.*
