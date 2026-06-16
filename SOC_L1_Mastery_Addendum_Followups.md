# SOC L1 Interview — Mastery Addendum (Follow-up Questions)

**What this is:** the questions you raised *while reviewing* the main material. These tend to be the exact gaps an interviewer probes after your first answer ("ok, and how does that ticket differ from a Golden one?"), so they're worth holding tight. Concept questions are in the usual format (model answer → know cold → SOC angle); the Russian term translations are collected at the end as a quick glossary.

---

# Concepts

## 1. DCSync

**Model answer:** DCSync abuses Active Directory's **replication** mechanism. The attacker doesn't log into a Domain Controller or touch LSASS — they *impersonate a DC* and ask a real DC to "replicate" account data, including password hashes, for any user.

**Know cold:**
- DCs sync with each other using the replication protocol **DRSUAPI** (spec **MS-DRSR**, over RPC), via the call **`GetNCChanges`** — which returns object changes *including secret attributes (password hashes)*.
- An account with replication rights can issue the same `GetNCChanges` and pull the hash of **any** user — including **krbtgt**, which enables a Golden Ticket.
- Required extended rights on the domain object:
  - `DS-Replication-Get-Changes` — GUID `1131f6aa-9c07-11d1-f79f-00c04fc2dcd2`
  - `DS-Replication-Get-Changes-All` — GUID `1131f6ad-9c07-11d1-f79f-00c04fc2dcd2`
  - By default only DCs and Domain/Enterprise Admins hold these. A normal account having them = **ACL abuse** (what BloodHound highlights).
- Tools: `mimikatz "lsadump::dcsync /user:krbtgt"`, Impacket `secretsdump.py`.

**Why dangerous:** pulls krbtgt → direct path to **Golden Ticket** and full domain control; never logs into a DC; mimics legitimate DC-to-DC traffic.

**SOC angle (the detection):** Event ID **4662** on a DC where the `Properties` field contains a **replication GUID** (`1131f6aa…` / `1131f6ad…`) **and the source is not a Domain Controller**. Legit replication only happens between DCs, so a replication request from anything else = escalate. If confirmed → assume krbtgt is exposed → **double krbtgt password reset**.

**One-liner:** *"DCSync abuses AD replication rights — the attacker impersonates a DC and uses GetNCChanges to pull any account's hash, including krbtgt, without logging into a DC. Detect via Event ID 4662 with a replication GUID from a non-DC source."*

---

## 2. Diamond Ticket

**Model answer:** A stealthier evolution of the Golden Ticket. Instead of forging a TGT from scratch, the attacker takes a **legitimate** TGT and modifies it — so the ticket is backed by a real exchange with the DC.

**How it works:**
1. Request a **legitimate** TGT from the KDC normally (real AS-REQ/AS-REP → a genuine 4768 in the logs).
2. Decrypt that TGT with the **krbtgt** key (the key is still required).
3. Modify the **PAC** inside — e.g., add Domain Admins membership.
4. Re-encrypt with the same krbtgt key.

**Golden vs Diamond:**

| | Golden | Diamond |
|---|---|---|
| Needs krbtgt key | Yes | Yes |
| Real KDC request (4768) | No | Yes (genuine) |
| Construction | Built from scratch | Modifies a real TGT |
| Stealth | Lower | Higher |

**Bonus — Sapphire Ticket:** even stealthier; uses **S4U2self + U2U** to embed the PAC of a real privileged user, so the ticket contents are fully legitimate with no manual PAC edits.

**SOC angle:** harder to catch than Golden precisely because a real 4768 exists. Rely on **PAC anomalies** (account whose privileges don't match its real group membership), abnormal ticket lifetimes, and PAC validation. All three (Golden/Diamond/Sapphire) require the krbtgt key → post-domain-compromise → remediate with double krbtgt reset.

**One-liner:** *"A Diamond Ticket modifies a real TGT — the attacker gets a legitimate ticket, decrypts it with the krbtgt key, edits the PAC, and re-encrypts — so a genuine DC request backs it, making it stealthier than a Golden Ticket."*

---

## 3. Directory Replication Protocol

**Model answer:** Directory replication is how Domain Controllers keep identical copies of the AD database in sync. AD is **multi-master** — changes can be made on any DC and propagate to the rest.

**Know cold:**
- The protocol is **DRSUAPI** (Directory Replication Service Remote Protocol, spec **MS-DRSR**), running over **RPC**.
- Key call: **`IDL_DRSGetNCChanges`** (`GetNCChanges`) — one DC asks another for changes since the last sync; the response **includes password-hash attributes** because every DC must hold all credentials.
- Data is split into **naming contexts (NCs)**: Schema, Configuration, Domain (hence "Get **NC** Changes").
- Changes carry metadata (USN, version, timestamp) for conflict resolution; **KCC** builds the replication topology.

**Security tie-in:** DCSync directly abuses this protocol — same `GetNCChanges` call, but issued by an attacker-controlled account.

**Chain to remember:** `DRSUAPI / MS-DRSR` (protocol) → `GetNCChanges` (call that returns changes + hashes) → DCSync (same call from an attacker) → detect with **4662 + replication GUID from a non-DC**.

---

## 4. What to do if a user clicked a malicious link

**Model answer:** First determine *what the click actually did* — that drives everything — then run contain → investigate → eradicate → recover.

**Triage the click into one of three cases:**
1. Page only loaded (no creds, no download) — lowest impact.
2. **Credentials entered** on a phishing page — assume the password is compromised.
3. **File downloaded/run** or drive-by — possible code execution.
Verify with telemetry; don't just trust "I don't think I typed anything."

**Contain (speed > completeness):**
- **Isolate the host** (EDR network isolation) if any sign of download/execution.
- **Block the URL/domain/IP** at proxy/firewall and the **sender** at the mail gateway.
- If creds entered → **reset the password AND revoke active sessions/tokens** (reset alone doesn't kill a live session).

**Investigate / scope:**
- Endpoint telemetry: **Sysmon EID 1** (process), **EID 3** (network), **EID 22** (DNS), **4688/4104** for PowerShell — did it reach execution + C2, or stop at a web page?
- Check the destination (VirusTotal/URLScan; sandbox any file).
- **Scope the blast radius** in the mail gateway: who else received it, clicked, entered creds. One click = a ticket; many = an incident.
- If creds entered, check identity logs (Entra/Okta) for new-IP logins, impossible travel, MFA prompts, new tokens.

**Eradicate:** remove dropped payload/persistence (4698 tasks, Run keys, 7045 services); purge the email org-wide.

**Recover:** re-image if execution was confirmed; restore the account with new creds + re-enrolled MFA; confirm no further callback.

**Document & escalate** to L2 on confirmed execution, credential use, or multiple affected users.

**Two details that sound senior:**
- "Reset the password **and** kill active sessions" (tokens survive a reset).
- "Scope **who else** clicked" (turning one ticket into an incident assessment).

---

## 5. True Negative vs False Negative

**Model answer:** Both produce **no alert** — the difference is reality underneath.
- **True Negative (TN):** benign activity, correctly no alert. Good silence.
- **False Negative (FN):** real malicious activity that the system **missed** — silent when it should have fired. The *most dangerous* outcome, because the threat goes completely unnoticed.

**The matrix:**

| | Actually malicious | Actually benign |
|---|---|---|
| **Alert fired** | True Positive ✅ | False Positive (noise) |
| **No alert** | **False Negative ⚠ (worst)** | **True Negative ✅** |

**Interview point:** FNs are worst because they're invisible — an FP wastes your time, an FN means an attacker operates undetected. That's why detection engineering tolerates *some* false positives rather than tuning so hard you create blind spots (false negatives).

**One-liner:** *"Both are silent. A true negative is correct silence on benign activity; a false negative is a miss — real malicious activity the system failed to alert on, which is the most dangerous because it goes unnoticed."*

---

# Russian Glossary (English → Russian, security context)

| English term | Russian | Note |
|---|---|---|
| forged (ticket) | поддельный / сфабрикованный | "to forge" = подделать/сфабриковать (not "ковать" in this context) |
| forged with the krbtgt hash | подделывается с помощью хеша krbtgt | how Golden Ticket is described |
| high-fidelity (alert/detection) | высокоточный / высокодостоверный | срабатывание с минимумом ложных (low false positives); antonym = "шумный" (noisy) |

**Sentence translation kept for reference:**
- EN: *"Grants arbitrary access, long validity, survives password resets of the impersonated user."*
- RU: *«Предоставляет доступ к чему угодно, действует очень долго и не сбрасывается при смене пароля учётки, под которую маскируется атакующий.»*
- Context: describes a Golden Ticket — it survives the victim's password reset because it's signed with the **krbtgt** hash, not the impersonated user's password. Only a **double krbtgt reset** kills it.
