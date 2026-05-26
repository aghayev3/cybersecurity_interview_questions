# Essentials Track — Interview Questions Bank

*100 questions per subject • Bilingual (Azerbaijani / English) • Modeled on the Template-Questions.docx interview style*

**Sections per subject:**
- **A. General Questions** — short, factual / conceptual (40)
- **B. Scenario-Based Questions** — real-world troubleshooting with concept hints (40)
- **C. Checklist-Style Questions** — rapid-fire verbal-exam prompts (20)

**Subjects:** 1. Cloud • 2. Intro to Cyber • 3. Linux • 4. Microsoft Technologies • 5. Networking Essentials • 6. Python • 7. Security Gateways

---

### A. General Questions (1–40)

**1. What is cloud computing and what are its main service models (IaaS, PaaS, SaaS)?**

* **Cloud Computing:** The on-demand delivery of computing power, database storage, applications, and IT resources over the internet with pay-as-you-go pricing.
* **IaaS (Infrastructure as a Service):** Provides raw computing infrastructure (e.g., virtual machines, storage, networks) where the user manages the OS, middleware, and applications.
* **PaaS (Platform as a Service):** Provides a managed hardware and software environment (e.g., Azure App Services) allowing developers to build and deploy applications without managing the underlying OS or infrastructure.
* **SaaS (Software as a Service):** Provides a fully completed, vendor-hosted application software product (e.g., Microsoft 365) managed entirely by the cloud provider.

**2. What is the Shared Responsibility Model and how is responsibility divided between customer and provider in IaaS, PaaS, and SaaS?**
A security framework defining which security tasks belong to the cloud provider and which belong to the customer.

* **Provider Responsibilities:** Always responsible for physical security of datacenters, core networking, and host virtualization infrastructure.
* **Customer Responsibilities:** Always responsible for information, data governance, endpoint devices, and accounts/identities.
* **Service Model Shift:** In **IaaS**, the customer retains responsibility up through the operating system and applications; in **PaaS**, the customer only manages applications and data; in **SaaS**, the customer only manages identities, access boundaries, and data configurations.

**3. What is the difference between public, private, hybrid, and community cloud?**

* **Public Cloud:** Computing services offered by third-party providers (e.g., Azure, AWS) over the public internet, making them available to anyone who wants to purchase them.
* **Private Cloud:** Computing services offered either over the internet or a private internal network and only to select users, hosted within a single organization’s dedicated datacenter.
* **Hybrid Cloud:** A computing environment that combines a public cloud and a private cloud, allowing data and applications to be shared between them.
* **Community Cloud:** A collaborative infrastructure shared between several organizations with common security, compliance, or jurisdictional requirements.

**4. How is the hierarchy structured between Azure tenant, subscription, and resource group?**
The operational hierarchy spans four distinct levels from top to bottom:

1. **Management Groups:** Containers that help manage access, policy, and compliance across multiple subscriptions.
2. **Azure Tenant (Entra ID IDP):** The top-level logical directory boundary representing an organization’s identity infrastructure.
3. **Subscriptions:** A logical billing and administrative access boundary used to provision resource collections.
4. **Resource Groups:** A logical deployment container within a subscription used to aggregate, deploy, and manage related Azure resources together.

**5. What are the main differences between Azure AD (Entra ID) and on-prem Active Directory?**

* **Protocols:** On-prem AD relies on Kerberos, NTLM, and LDAP authentication frameworks. Entra ID utilizes modern, web-centric HTTP protocols including OAuth 2.0, OpenID Connect, and SAML 2.0.
* **Structure:** On-prem AD is a hierarchical, multi-tiered structure containing forests, domains, and Organizational Units (OUs). Entra ID is a flat identity platform containing users, security groups, and enterprise applications.
* **Device Management:** On-prem AD governs endpoint computers via Group Policy Objects (GPOs); Entra ID manages devices via Mobile Device Management (MDM) policies like Microsoft Intune.

**6. What is a Conditional Access policy and what signals does it use?**

* **Definition:** The zero-trust policy evaluation engine in Microsoft Entra ID used to implement granular access requirements (e.g., allow, block, or require MFA) based on organizational criteria.
* **Signals Used:** Evaluates real-time parameters including User or Group membership, geographic Location (Named IP ranges), Device Compliance state (Intune registration), target Cloud Application identity, and real-time User/Sign-in Risk levels computed by Identity Protection.

**7. Is there a difference between MFA and 2FA? What is phishing-resistant MFA?**

* **2FA vs. MFA:** 2FA requires exactly two distinct authentication factors. MFA requires two or more authentication factors drawn from independent categories (something you know, something you have, something you are).
* **Phishing-Resistant MFA:** Authentication methods that eliminate human vulnerabilities to interception or adversary-in-the-middle (AiTM) proxy attacks. This is achieved by cryptographically binding the user’s authentication session directly to the target application's verified web domain (e.g., FIDO2 WebAuthn security keys, Windows Hello for Business).

**8. What is RBAC and which built-in roles exist in Azure?**

* **RBAC (Role-Based Access Control):** An authorization model used to manage permissions on Azure resources by assigning specific security roles to user identities, groups, or service principals at distinct structural scopes.
* **Core Built-in Roles:** * **Owner:** Full access to all resources including the capacity to delegate permissions to other users.
* **Contributor:** Can create and manage all types of Azure resources but cannot grant access to others.
* **Reader:** Can view existing Azure resources but cannot modify or delete them.
* **User Access Administrator:** Manages user access permissions to Azure resources without granting underlying resource configuration rights.



**9. Which products does Microsoft 365 Defender unify?**
Unifies endpoint, identity, application, and email security tracking within a single portal, combining:

* Microsoft Defender for Endpoint (MDE)
* Microsoft Defender for Identity (MDI)
* Microsoft Defender for Office 365 (MDO)
* Microsoft Defender for Cloud Apps (MDCA)

**10. What is the difference between Defender for Endpoint, Defender for Identity, Defender for Office 365, and Defender for Cloud Apps?**

* **Defender for Endpoint:** An Endpoint Detection and Response (EDR) agent solution that monitors host computer endpoints for malicious behaviors, active compromises, and unauthorized software.
* **Defender for Identity:** A cloud-based solution that uses on-premises Active Directory signals to detect advanced threats, compromised identities, and malicious insider actions directed at Domain Controllers.
* **Defender for Office 365:** Protects organization collaboration channels against email threats, malicious links, phishing attempts, and weaponized attachments (Safe Attachments / Safe Links).
* **Defender for Cloud Apps:** A Cloud Access Security Broker (CASB) platform providing deep visibility, data loss prevention (DLP), and threat protection for corporate SaaS applications.

**11. What is Microsoft Sentinel and how does it combine SIEM/SOAR functions?**
A cloud-native Security Information and Event Management (SIEM) and Security Orchestration, Automation, and Response (SOAR) platform.

* **SIEM Capabilities:** Collects security telemetry across all enterprise tiers at scale, correlates alerts using analytics rules, and facilitates threat hunting.
* **SOAR Capabilities:** Leverages automated Playbooks (built on Azure Logic Apps) to orchestrate defensive tools and execute immediate response playbooks to remediate active threats.

**12. What is the purpose of data connectors, analytics rules, workbooks, and playbooks in Sentinel?**

* **Data Connectors:** Integration mechanisms used to ingest log streams from diverse security, cloud, and infrastructure sources.
* **Analytics Rules:** Scheduled KQL search queries that continuously parse ingested logs to identify malicious activity patterns and generate actionable Alerts/Incidents.
* **Workbooks:** Interactive, customizable visual dashboards used to monitor data trends, track metrics, and analyze log metrics inside Sentinel.
* **Playbooks:** Automated workflows triggered by security incidents to execute automated remediation and response steps.

**13. What is KQL (Kusto Query Language) and where is it used?**
A read-only command query language used to process big data and generate insights within Microsoft cloud repositories. It is used to write threat detection queries, hunt for attackers, and build visualizations inside Azure Log Analytics Workspaces, Microsoft Sentinel, and Microsoft Defender XDR Advanced Hunting.

**14. What is a Log Analytics Workspace and how does it relate to Sentinel?**

* **Log Analytics Workspace (LAW):** The foundational underlying data storage container and analytical platform within Azure Monitor used to house collected data logs.
* **Relation to Sentinel:** Microsoft Sentinel is deployed directly on top of a Log Analytics Workspace. The LAW serves as the underlying physical storage engine where security logs reside, while Sentinel provides the security overlay, intelligence rules, and SOAR capabilities.

**15. What is the difference between fusion, scheduled, NRT, and ML behavioral analytics rules in Sentinel?**

* **Fusion Rules:** Uses graph-based machine learning algorithms to automatically cross-correlate disparate low-fidelity alerts across multiple kill-chain vectors into single high-fidelity multi-stage attack incidents.
* **Scheduled Rules:** Standard user-configured KQL search queries that evaluate log tables at regular, pre-set operational time intervals.
* **Near-Real-Time (NRT) Rules:** Fast execution queries designed to parse log entries immediately upon ingestion, reducing threat visibility delays to minutes.
* **ML Behavioral Analytics:** Proprietary, black-box machine learning engines built by Microsoft to identify anomalies based on user or resource behavioral variations.

**16. What is the difference between an incident and an alert in Sentinel?**

* **Alert:** A singular, discrete security signal generated by an analytics rule pointing to a specific event or anomaly.
* **Incident:** A composite tracking container created by Sentinel that aggregates one or more related alerts along with associated entities (hosts, accounts, IPs) into a single investigation case.

**17. What is UEBA (User and Entity Behavior Analytics) and which attacks does it help detect?**

* **Definition:** An intelligence overlay inside Sentinel that builds behavioral baselines for corporate user identities and hardware entities over 30 days to flag behavioral anomalies.
* **Attacks Detected:** Identifies insider threats, compromised user accounts, active data exfiltration spikes, credential abuse, and lateral movement paths.

**18. What are the three main encryption states in the cloud (at rest, in transit, in use)?**

* **At Rest:** Cryptographic protection applied to static data stored on physical disks, blocks, or database engines (e.g., using AES-256 via Azure Storage Service Encryption).
* **In Transit:** Cryptographic protection applied to data actively traversing network infrastructure paths (e.g., utilizing TLS 1.2/1.3, HTTPS, or IPsec VPN tunnels).
* **In Use:** Cryptographic protection that secures data actively loaded in system RAM during active processor execution contexts (e.g., using confidential computing with hardware-enforced secure enclaves).

**19. What is Azure Key Vault used for and what objects does it store?**

* **Purpose:** A centralized, hardware-backed cloud management repository used to manage and secure access to sensitive application secrets.
* **Objects Kept:** * **Secrets:** Plaintext strings like passwords, API tokens, and database connection strings.
* **Keys:** Cryptographic encryption keys used for wrapping or server-side disk protection tasks.
* **Certificates:** Public and private X.509 certificates used for secure TLS transport configurations.



**20. What is the difference between a customer-managed key (CMK) and a platform-managed key?**

* **Platform-Managed Keys:** Encryption keys generated, rotated, and completely managed by Azure infrastructure defaults, requiring no configuration from the customer.
* **Customer-Managed Keys (CMK):** Keys stored within the customer’s own Azure Key Vault, granting the organization full control over key rotation schedules, fine-grained access control policies, and instant cryptographic revocation capabilities.

**21. What is the security difference between cloud-native and lift-and-shift migration?**

* **Lift-and-Shift:** Moving legacy on-premises workloads (like VMs) directly into the cloud without modifying their underlying architecture. Security liabilities include inherited OS vulnerabilities, unpatched technical debt, and a dependency on traditional network perimeter defenses.
* **Cloud-Native:** Designing systems from scratch to use managed cloud services (e.g., serverless, containers, PaaS). Security advantages include built-in scalability, automated patching, reduced OS-level attack surface, and a tighter focus on data security.

**22. What is the security difference between containers, VMs, and serverless functions?**

* **Virtual Machines:** Provide full OS virtualization isolated by a hypervisor. This setup offers strong isolation boundaries but requires extensive host-level patch management and security hardening.
* **Containers:** Share the underlying host OS kernel while segregating application processes using namespaces and cgroups. They offer faster deployment but a weaker isolation boundary, making kernel vulnerabilities on the host a higher risk.
* **Serverless Functions:** Abstract away all underlying infrastructure layers, running short-lived code blocks managed entirely by the provider. This eliminates OS patching concerns and leaves the developer responsible only for the code security and IAM configurations.

**23. What is a CASB and what is its role in SaaS security?**

* **CASB (Cloud Access Security Broker):** A security gateway positioned between cloud consumers and cloud service providers to enforce security, compliance, and governance policies.
* **Role in SaaS:** Discovers unauthorized software (Shadow IT), monitors user access behaviors, enforces data classification rules, detects cloud-based data exfiltration, and applies Data Loss Prevention (DLP) across enterprise SaaS environments.

**24. What does CSPM (Cloud Security Posture Management) do?**
Continuously monitors cloud infrastructure environments to assess compliance states, identify configuration drift, detect security misconfigurations (such as publicly exposed storage buckets), and provide prescriptive remediation recommendations based on industry standards like CIS benchmarks.

**25. What does CWPP (Cloud Workload Protection Platform) mean?**
A workload-centric security mechanism designed to provide threat protection, vulnerability management, and runtime behavioral security across virtual machines, container clusters, and serverless architectures regardless of location.

**26. What is the difference between a Network Security Group (NSG) and Azure Firewall?**

* **NSG:** A basic Layer 4 stateful packet filter applied at the network interface (NIC) or subnet level, controlling inbound/outbound traffic using basic source/destination IPs, ports, and protocols.
* **Azure Firewall:** A managed, cloud-based Layer 3-7 network security service. It features built-in high availability, threat intelligence filtering, fully qualified domain name (FQDN) application rules, and deep packet inspection (DPI) capabilities.

**27. What is the difference between a Private Endpoint and a Service Endpoint?**

* **Service Endpoint:** Directs traffic over the Microsoft backbone network by extending your VNet identity to the public endpoint of an Azure service. The service still retains its public IP address.
* **Private Endpoint:** Uses a private IP address from your VNet to bring a specific Azure resource directly inside your network. This eliminates public internet routing and fully disables the resource's public IP address.

**28. What is a bastion host and why is it safer than using public IPs for RDP/SSH?**

* **Bastion Host:** A dedicated, hardened gateway resource that provides secure, browser-based administrative access to virtual machines over SSL/TLS.
* **Safety Value:** It removes the need to expose public IP addresses or ports like 3389 (RDP) and 22 (SSH) to the open internet, shielding internal virtual machines from brute-force authentication attacks and port scans.

**29. What is Just-In-Time (JIT) VM access?**
A security control inside Microsoft Defender for Cloud that minimizes internet threat exposure by locking down inbound RDP and SSH traffic to virtual machines. When an administrator requests access, the system evaluates RBAC rules and opens the necessary port for a specific, time-bound window (e.g., 3 hours) before automatically re-closing it.

**30. What is PIM (Privileged Identity Management) and what problems does it solve?**

* **Definition:** A service in Microsoft Entra ID used to manage, control, and monitor access to important organization resources.
* **Problems Solved:** It eliminates "standing privileges" (where users have permanent admin rights) by implementing **Just-In-Time (JIT)** elevation. This ensures users only receive administrative rights for a limited time and require explicit manager approval, multi-factor authentication (MFA), and a documented business justification to activate them.

**31. What is the difference between SSO and federation in the cloud?**

* **Single Sign-On (SSO):** An authentication mechanism that allows a user to authenticate once and access multiple related software systems within a single organizational identity boundary.
* **Federation:** Extends authentication capabilities across distinct corporate boundaries, linking separate identity providers (IdPs) so a user can log in using their home organization's credentials to access resources in an external partner's tenant.

**32. What is the difference between SAML, OAuth, and OpenID Connect?**

* **SAML (Security Assertion Markup Language):** An XML-based framework used to exchange authentication and authorization data, primarily for enterprise web-based Single Sign-On (SSO).
* **OAuth 2.0:** An HTTP-based **authorization framework** that uses JSON web tokens (JWTs) to delegate application access scopes, allowing software to interact with APIs on behalf of a user without exposing their credentials.
* **OpenID Connect (OIDC):** An identity layer built directly on top of the OAuth 2.0 framework to add **authentication** capabilities, using standardized ID tokens to verify user profiles.

**33. Why is tenant isolation important in the cloud?**
It functions as a strict logical barrier within multi-tenant cloud data architectures. It ensures that an organization's compute operations, data stores, configuration environments, and user identities remain secure and inaccessible to other tenants sharing the same physical hardware resources.

**34. What does Microsoft Secure Score measure?**
A relative percentage metric that measures an organization's current cloud security posture. It evaluates active tenant configurations against Microsoft's recommended security baselines, awarding points as you implement controls like enabling MFA, blocking legacy protocols, or setting up data policies.

**35. What does the regulatory compliance dashboard in Defender for Cloud serve?**
It provides real-time visibility into your cloud compliance posture by mapping resource configurations directly to industry benchmarks and regulatory standards (such as SOC 2, ISO 27001, PCI-DSS, and NIST). This allows organizations to track gaps, audit configurations, and generate compliance reports for auditors.

**36. What events do cloud audit logs (Azure Activity Log, M365 Unified Audit Log) record?**

* **Azure Activity Log:** Records control-plane operations across your Azure subscription, tracking who created, modified, or deleted resources (e.g., starting virtual machines or modifying NSG rules).
* **M365 Unified Audit Log (UAL):** Captures user and administrative actions across Microsoft 365 services, logging activities such as file access in SharePoint, email deletions in Exchange, configuration changes in Entra ID, and data downloads.

**37. What is the difference between data residency and data sovereignty?**

* **Data Residency:** The physical geographic location or nation-state where an organization chooses to store its data at rest within a cloud provider's datacenters.
* **Data Sovereignty:** The legal principle that data is subject to the privacy, digital compliance, and law enforcement access structures of the specific nation-state where the data is physically located.

**38. What is Shadow IT and why is it dangerous in a cloud environment?**

* **Definition:** The use of IT systems, software applications, devices, or cloud services within an organization without explicit approval or oversight from the centralized IT and Security departments.
* **Dangers:** It introduces unmonitored data exfiltration risks, leads to regulatory compliance violations, bypasses corporate identity and multi-factor authentication (MFA) controls, and exposes the organization to supply chain vulnerabilities.

**39. Which protocols and behaviors does Defender for Identity monitor?**
Monitors on-premises Active Directory domain controller network traffic, analyzing protocols like **Kerberos, NTLM, LDAP, DNS, and RPC**. It uses this data to flag malicious activities such as Pass-the-Hash, Golden Ticket creation, Kerberoasting, and unauthorized domain replication attempts (DCShadow/DCSync).

**40. What is the difference between a SOAR playbook and a manual response?**

* **SOAR Playbook:** An automated workflow that responds to security alerts instantly without human intervention, executing steps like disabling compromised accounts or isolating hosts in seconds.
* **Manual Response:** Requires a security analyst to review an alert, log into multiple portals, verify the context, and execute remediation steps by hand. This process is slower, prone to human error, and difficult to scale.

---

### B. Scenario-Based Questions (41–80)

**41. A user can normally sign in to the Azure portal, but a Conditional Access policy sometimes blocks them. How would you troubleshoot?**
Analyze the Entra ID **sign-in logs** to identify the specific transaction entry marked as blocked. Inspect the applied policy maps to determine which rule triggered the block. Verify if the block was caused by an unrecognized location by checking **named locations** or if the device failed **device compliance** checks in Microsoft Intune. Check if the block stemmed from a **risk-based policy** threshold, and use the **CA What-If tool** to simulate the user's connection parameters to isolate the root cause.

**42. Your company has multiple Global Admins. How would you explain the risks and which controls would you apply?**

* **Risks:** Multiple permanent Global Admin accounts expand the organization's attack surface. If a single account is compromised, it grants attackers full control over the tenant, allowing them to delete infrastructure, exfiltrate data, or lock out other admins.
* **Controls:** Enforce the principle of **least privilege** by reassigning users to granular roles. Migrate administrators to **PIM JIT activation** so they only hold admin rights for a limited, approved time window. Establish two isolated, cloud-native **break-glass accounts** to maintain tenant access during an emergency, implement strict **role separation**, and mandate phishing-resistant **MFA + FIDO2 security keys** for all administrative access.

**43. Sentinel is producing too many false positives. How would you reduce them?**
Begin **tuning analytics rules** by modifying KQL logic thresholds and optimizing exclusion variables. Implement custom **watchlists** to dynamically reference and filter out approved operational processes or assets, and define organizational **allow lists** for known-safe administrative networks. Adjust trigger thresholds based on baseline activity metrics, and ensure proper **entity mapping** so alerts correlate accurately without creating duplicate alerts.

**44. Defender for Endpoint detected "Suspicious PowerShell activity" on a host. What would your first steps be?**
Immediately **isolate the device** from the network using the Defender console to prevent potential lateral movement. Open the **advanced hunting** interface to review the host's execution history, reconstruct the visual **process tree**, and inspect the exact **command-line** arguments used to identify obfuscation or malicious intents. Identify the **parent process** that initialized PowerShell, and extract key indicators of compromise (IOCs) like file hashes or C2 IP addresses to scan and pivot across the rest of the tenant.

**45. A newly provisioned Azure subscription has a CSPM compliance score of 35%. In what order would you act to improve it?**
Review the prioritized **secure score recommendations** to remediate high-impact vulnerabilities first. Enforce **MFA enforcement** across all user and administrative accounts, and configure **JIT VM access** to close public RDP/SSH exposures. Harden **NSG network rules** to block open internet access, activate targeted **Defender plans** to enable real-time workload tracking, and secure exposed app connection strings by moving them into an **Azure Key Vault**.

**46. A mailbox triggered an "impossible travel" signal (Baku → London in 30 min). How would you investigate this incident?**
Review the Entra ID **sign-in risk** events to evaluate the source IP addresses, hosting providers, and client applications involved. Check for **MFA fatigue** indicators, such as a pattern of repeated, denied push notifications that ended in a successful login. Inspect the tenant for unauthorized **OAuth app grants** or newly added **mailbox rules** used to auto-forward messages, and scan client connection telemetry for tokens pointing to an **adversary-in-the-middle (AiTM) phishing** attack.

**47. A user consented an OAuth app (consent phishing) was discovered. What would your response be?**
Immediately **revoke the app consent** globally across the tenant to cut off data access, and **disable the user account** to stop active compromise pathways. Review the exact **API permissions** granted to the application (e.g., `Mail.ReadWrite`) to determine data exposure, inspect the user's account for malicious **mailbox rules** set up after the integration, and audit tenant discovery logs in **Defender for Cloud Apps** to check for similar unauthorized third-party application connections.

**48. You discovered a public blob in a Storage Account. What analysis and remediation steps would you take?**
Immediately audit and revoke any active **SAS tokens** or public access configurations linked to the storage container. Enforce strict **storage firewall** rules to restrict incoming connections to trusted corporate networks, and deploy a **Private Endpoint** to remove public internet routing pathways. Analyze **storage access logs** to identify who accessed the data, and update your **data classification** labels to ensure automated policies catch future exposure risks.

**49. A Lateral Movement analytics rule fired in Sentinel. Which entities and data would you correlate?**
Query your logs to correlate Windows Security **Event ID 4624 Logon Types** (focusing on Type 3 Network and Type 10 RDP events). Extract **RemoteFromIP** values to identify the source machines, and map the relationships between compromised user **accounts** and target **hosts**. Trace connection paths via network telemetry (**SMB/RPC** traffic records), and aggregate these events within tight **time-window groupings** to reconstruct the attacker's path through the network.

**50. The company says "encrypt all machines". How would you implement this in Azure and what should you plan?**
Deploy **Azure Disk Encryption (ADE)** across all virtual machines to leverage native **BitLocker** for Windows workloads and **DM-Crypt** for Linux platforms. Configure an **Azure Key Vault** to store and manage the encryption keys, and establish a clear **recovery key custody** protocol to ensure keys are available during system recoveries. Plan your architecture by choosing between extension-driven host-based encryption versus server-side storage encryption based on your operational requirements.

**51. Defender for Identity raised a Pass-the-Hash signal. How would you validate it?**
Analyze domain controller logs to audit active **NTLM authentication events** matching the alert timeline. Look for signs of unauthorized **LM/NT hash reuse** across endpoints without matching Kerberos ticket generation sequences, and locate the **source process** driving the authentication request. Compare this activity against the user's historical **behavioral baseline**, and apply **false-positive filters** to rule out legitimate, high-volume service account tools.

**52. The company wants hybrid identity (on-prem AD + Azure AD). What sync options exist and what are their security differences?**

* **Options:** Use Microsoft Entra Connect to deploy **Password Hash Sync (PHS)**, configure **Pass-through Authentication (PTA)**, or set up a federated environment using **Federation/ADFS**.
* **Security Differences:** PHS stores encrypted credential hashes directly in the cloud, allowing cloud authentication even during on-prem outages. PTA validates credentials in real-time using local on-prem agents, keeping password verification inside your local network. ADFS shifts the identity management burden to an external identity provider, which expands your on-prem **attack surface** and requires extensive infrastructure maintenance. Enabling **seamless SSO** across these options minimizes authentication prompt fatigue.

**53. A file uploaded by a user in Teams was flagged as suspicious by Defender for Cloud Apps. How would you proceed?**
Trigger an immediate **file policy** action to isolate the file, and apply cloud **Data Loss Prevention (DLP)** rules to block further distribution. Run an automated **malware scan** to evaluate the file payload, ensure the asset maintains proper corporate **label inheritance**, and move the file into a secure **quarantine** folder. Finally, run a comprehensive **share-link audit** to identify and revoke any external access links generated for the file.

**54. Sentinel ingestion cost suddenly tripled. How would you find the cause?**
Query the Log Analytics workspace `Usage` table to identify which log streams are driving the surge. Check the `Heartbeat` table to verify agent health, and calculate total log data distribution by **table size** metrics. Pinpoint whether a server is streaming verbose **Syslog** data, implement custom **ingestion filters** to drop redundant security events at the gateway, and adjust your workspace **commitment tier** to optimize costs.

```kusto
// Find which tables are ingesting the most data in the last 24 hours
Usage
| where TimeGenerated > ago(24h)
| where DataType != ""
| summarize TotalQuantityGB = sum(Quantity) / 1024 by DataType
| order by TotalQuantityGB desc

```

**55. You suspect an M365 admin account may be compromised. What would you do in the first 30 minutes?**
Immediately **revoke all active user sessions** via the Entra ID portal to terminate active attacker connections, **disable the administrator account**, and execute an emergency **password reset**. Clear the account's authentication options by initiating an **MFA registration reset**, and search your cloud **audit logs** (such as the M365 Unified Audit Log) to trace recent actions. Finally, audit and remove any newly registered **OAuth consents** or application access configurations linked to the account.

**56. Someone questions the "misconfiguration > zero-day" principle in the cloud. How would you justify it with real examples?**

* **Justification:** Zero-day exploits require advanced engineering skills and are typically used in highly targeted attacks. In contrast, cloud misconfigurations (such as open access controls or default settings) can be discovered by automated internet scanners in minutes, making them the primary entry point for cloud breaches.
* **Examples:** Cite public data breaches caused by open **AWS S3 buckets or Azure Blob containers**, organizations compromised due to **default administrative credentials** left active on cloud applications, and data exposures resulting from broad **Network Security Group rules (0.0.0.0/0)** that expose internal corporate databases directly to the public internet.

**57. You want to enable a new "block legacy authentication" Conditional Access policy. What risks and preparation steps?**

* **Risks:** Blocking legacy protocols (like IMAP, POP3, or SMTP Auth) can disrupt older business applications, stop mail flows on legacy mobile devices, and block service accounts that do not support modern authentication.
* **Preparation Steps:** Deploy the policy to a limited **pilot group** first to monitor its operational impact, and run the policy in **report-only mode** to safely collect telemetry. Filter your Entra ID **sign-in logs** to build an inventory of applications using legacy authentication protocols, and coordinate with support teams to build an operational **helpdesk response plan** before enforcing the change.

**58. A SaaS DLP policy blocked an HR director (sending a CV). How would you investigate and balance this?**
Review the alert details to determine if the block was a **false positive** caused by an overly broad rule signature. If the transfer is legitimate and authorized, use an **override workflow** to let the user complete the action safely. Tune your **Sensitive Information Type (SIT)** rules to better differentiate between public resumes and protected internal documents, collect a formal **business justification** from the director, and maintain a clear **audit trail** to document the exception.

**59. A Sentinel playbook should disable an account when fired but doesn’t. What is your debug sequence?**
Open the Azure Logic Apps interface and review the **run history** to locate the exact step where the failure occurred. Verify that the playbook's **managed identity permissions** are properly configured within Azure RBAC, and confirm that the identity has the correct **Microsoft Graph API scopes** (such as `User.ReadWrite.All`) to modify user accounts. Verify your **parameter mapping** variables to ensure the correct User Principal Name (UPN) is passing to the script, and configure automated **retry logic** to handle temporary API timeouts.

**60. What does "backup ≠ disaster recovery" mean in cloud? Explain with a ransomware scenario.**

* **The Distinction:** Backups are periodic copies of data used to protect against data loss. Disaster Recovery (DR) is the architecture and set of processes used to maintain continuous business service availability and minimize downtime during a major disruption.
* **Ransomware Scenario:** If ransomware encrypts your production environment, traditional backups are useless if the malware also located and deleted them. To protect your data, you need **immutable backups** combined with **soft-delete** protections and **geo-redundancy** to prevent unauthorized deletion. For business continuity, your **Disaster Recovery** strategy must meet strict **RTO/RPO** metrics. This requires running regular **restore drills** to spin up clean copies of your infrastructure within an **isolated recovery environment**, allowing you to resume business operations without paying the ransom.

**61. You see "admin consent grant" activity in the tenant and developers request API permissions without clear reason. What is your approach?**
Implement strict **app consent policies** to stop standard users from authorizing third-party applications, and establish an explicit administrative authorization workflow. Conduct an audit of all existing **app registrations** to identify unauthorized configurations, and enforce the principle of least privilege by restricting application requests to the minimum required **permission scopes** (e.g., swapping `Mail.Read.All` for limited target application permissions). Finally, enforce clear **owner accountability** by requiring application owners to provide a documented business justification before any administrative rights are granted.

**62. Defender for Cloud says "VM with public IP and open RDP". This is a production machine. What do you check before closing the port?**
Identify and contact the verified **business owner** to understand why the port was exposed to the internet. Identify secure **alternative access** pathways, such as setting up an **Azure Bastion host** or enabling **Just-In-Time (JIT) VM access**, to remove the open port exposure. Monitor active **management sessions** to ensure no administrator is currently connected, request an approved corporate **change window**, and prepare an operational **rollback plan** to quickly restore access if production services are disrupted.

**63. The company moves to a "cloud-only" strategy. What sequence would you recommend for identity migration?**

1. Set up a secure **hybrid sync** infrastructure using Microsoft Entra Connect to mirror your on-premises identities to the cloud.
2. Enable **Microsoft Entra ID Password Protection** to block weak or compromised passwords.
3. Enforce user onboarding into your **Multi-Factor Authentication (MFA)** policies.
4. Replicate and migrate your security groups to cloud-managed groups.
5. Execute the final domain **cutover** phase to shift authentication entirely to the cloud.
6. Completely **decommission** your legacy on-premises Active Directory domain controllers.

**64. Sentinel UEBA fired "user accessed unusual host". What baseline data would you check to validate?**
Review the user's **30-day peer baseline** data to see if this host access aligns with standard team behaviors, and look for changes within their designated **peer group** metrics. Evaluate the target **host criticality** ranking to assess the potential risk of the asset, check if the login occurred during an anomalous **time-of-day** window, and analyze **peer access overlap** to determine if other team members have interacted with the host.

**65. Someone complains "an admin role was left open" in the cloud. What query (KQL) would you write for audit?**

```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where OperationName in ("Add member to role", "Add eligible member to role")
| extend InitiatedBy = tostring(InitiatedBy.user.userPrincipalName)
| extend TargetPrincipal = tostring(TargetResources[0].userPrincipalName)
| extend AssignedRole = tostring(TargetResources[0].modifiedProperties[1].newValue)
| project TimeGenerated, OperationName, InitiatedBy, TargetPrincipal, AssignedRole
| order by TimeGenerated desc

```

**66. How is DDoS protection structured in cloud and what is the difference between Azure DDoS Standard and Basic?**

* **Structure:** Built on globally distributed **scrubbing centers** that intercept and clean incoming traffic before it reaches your network. The system uses continuous **traffic baselining** and real-time monitoring **telemetry** to identify anomalies, and integrates with internal response teams while providing **cost protection** to absorb scaling infrastructure charges caused by an attack.
* **Basic vs. Standard:** The **Basic (Infrastructure)** tier is enabled by default for all Azure resources at no extra cost, protecting the shared cloud infrastructure from large network layer attacks. The **Standard (DDoS Network Protection)** tier provides policy customizations tailored to your specific VNet traffic flows, advanced metrics alerting, access to the DDoS Rapid Response team, and financial insurance against resource scaling costs during an active attack.

**67. Incident: 500 GB of data was exfiltrated from a storage account. What are your forensic steps?**
Extract your **storage analytics logs** to track data transaction flows and build an inventory of accessed files. Audit historical **Shared Access Signature (SAS)** token generations to identify how the data was accessed, and isolate the attacker's **source IP address** to determine the geographic footprint of the breach. Execute an immediate **access keys rotation** to lock out the attacker, and isolate the affected storage systems to maintain strict **evidence preservation** guidelines for your investigation.

**68. In Defender XDR, automated investigation (AIR) isolated the wrong machine. How would you handle it?**
Log into the Microsoft Defender XDR portal and navigate directly to the **Action Center**. Locate the specific host isolation action log and select **Undo Isolation** to restore the machine's network connectivity. Review your security configurations to tune down the **AIR automation level** to prevent future unauthorized disruptions, update your device **approval policies** to require manual sign-off for disruptive actions, and document your findings in a **postmortem** report.

**69. A SaaS app has suspected OAuth token leak. What cleanup steps would you take?**
Force an immediate **session revocation** to invalidate all active user refresh tokens across the tenant, and **rotate the client secrets** and certificate keys linked to the application registration. Review your **application usage logs** to identify bulk data access anomalies or unauthorized downloads, implement a strict **Conditional Access policy** to restrict application access to verified corporate networks, and send out an identity security notification to all **affected users**.

**70. How would you deploy Microsoft Sentinel in a multi-tenant MSSP model?**
Deploy **Azure Lighthouse** to enable centralized, secure cross-tenant resource management and visibility. Provision a dedicated **Log Analytics workspace per customer tenant** to satisfy data isolation and regulatory compliance guidelines. Delegate precise **RBAC roles** to your security analysts, write cross-workspace **KQL queries** to hunt for threats across multiple tenants simultaneously, and set up centralized billing metrics to manage resource usage efficiently.

**71. An engineer says "no need for MFA, the password is complex". How would you respond?**
"Complex passwords do not protect against modern identity attacks. Attackers use automated **credential stuffing** tools to test passwords stolen in third-party breaches, and deploy targeted **phishing** campaigns or **adversary-in-the-middle (AiTM)** proxy platforms that steal complex passwords and active session tokens simultaneously. Enabling **MFA** blocks over 99.9% of automated account takeover attempts, making it a mandatory security control regardless of password length or complexity."

**72. You are setting up a new M365 tenant. What are your first 7 security steps?**

1. Configure two isolated, cloud-native **break-glass admin accounts**.
2. Enforce registration into **Multi-Factor Authentication (MFA)** policies for all users.
3. Apply baseline **Conditional Access policies** to block legacy authentication and untrusted locations.
4. Toggle tenant-wide **audit logging ON** to capture system activities.
5. Review your **Microsoft Secure Score** dashboard to remediate high-priority security gaps.
6. Configure your **Microsoft Defender** protection plans across identity, endpoints, and email.
7. Establish foundational **Data Loss Prevention (DLP)** profiles to protect sensitive data.

**73. How would you apply the "zero standing privilege" concept to real Azure resources?**
Configure **Microsoft Entra PIM eligible assignments** for all administrative roles, replacing permanent admin assignments. Enforce a mandatory **approval workflow** that requires independent manager sign-off before roles are elevated, and set strict **time-bound activation** windows (e.g., capping activation at 2 hours). Continuously audit your **activation logs**, and use **just-enough access (JEA)** controls to assign granular, task-specific permissions instead of broad admin rights.

**74. A Sentinel analyst wants to speed up their query. What would your recommendations be?**

* Limit your query's **time range** as early as possible in the script to avoid scanning unnecessary data rows.
* Use the **project** operator early to drop unused data columns and reduce memory usage.
* Execute data aggregations using the **summarize** operator to streamline results.
* Avoid using slow leading wildcards on string filters (use indexed string operators like `has` or `==` instead of `contains`).
* Filter your queries using **indexed columns** like `TimeGenerated`, and use the **materialize** function to cache repeated subquery results in memory.

**75. How would you explain the right and wrong sides of the claim "cloud is more secure"?**

* **The Right Side:** Cloud providers operate at a massive security scale, offering automated patching, advanced threat intelligence tracking, built-in redundancy, and deep platform visibility that most on-premises environments cannot duplicate.
* **The Wrong Side:** The cloud is not secure by default if the customer misconfigures it. Misunderstanding the **shared responsibility model** can lead to **default insecure configurations** and exposed resources. Additionally, the public nature of the cloud means attacks can scale rapidly, creating significant security visibility gaps if logs are not properly configured.

**76. Explain the difference between a service principal and a managed identity with a real use case.**

* **Service Principal:** A local application identity registration within an Entra ID tenant that requires manual credential, password, or certificate management and regular **secret rotation**.
* **Managed Identity (MI):** An application identity automatically managed by Azure infrastructure that completely eliminates the need for hardcoded credentials or manual secret rotation. It is available as either a **system-assigned** identity (bound to a single resource) or a **user-assigned** identity (shared across multiple resources).
* **Use Case:** An Azure Function App that needs to read database connection strings securely from an **Azure Key Vault**. Instead of embedding passwords inside the application code, you can enable a System-Assigned Managed Identity on the Function App and grant that identity access to the Key Vault.

**77. Defender for Cloud Apps anomaly detection raised "mass download" in a SaaS. What is your execution sequence?**
Identify and isolate the targeted **user account** identity, and pinpoint the specific **SaaS application** involved in the data transfer. Catalog the impacted **file types** to determine data sensitivity, and compare this download volume against the user's historical **peer baseline** metrics. Apply an immediate **session policy** restriction to block further data transfers, **force an immediate re-authentication** challenge to lock out potential session hijackers, and map out the full scope of the exfiltration event.

**78. How is resource compliance enforced with Azure Policy? Give a real example.**

* **Enforcement Mechanism:** Azure Policy continuously evaluates resource configurations against organizational rules. It enforces compliance using explicit effects like **deny** (blocking non-compliant resource creations), **audit** (flagging non-compliant configurations), or **deployIfNotExists** (automatically deploying missing security extensions). Policies are grouped into **initiatives**, allow for managed **exemptions**, and use automated **remediation tasks** to fix non-compliant systems.
* **Real Example:** An Azure Policy that uses a `deny` effect to block engineers from launching any virtual machine that lacks an assigned cost-center tag, or a policy that automatically deploys a log collection agent to every newly provisioned virtual machine.

**79. An admin says "30 days retention is enough in Sentinel". Explain the legal and operational counterarguments.**

* **Operational Counterarguments:** Threat intelligence reports indicate that the average attacker dwell time—the period an attacker remains hidden inside a network before detection—often exceeds 200 days. If logs are purged after 30 days, security teams lose the historical data needed for retroactive threat hunting and incident root-cause analysis.
* **Legal Counterarguments:** Industry compliance frameworks (such as PCI-DSS, SOC 2, HIPAA, and ISO 27001) mandate explicit log retention minimums ranging from 90 days to several years. Organizations can use long-term **archive tiers** to meet these compliance requirements safely while keeping data storage costs down.

**80. You discovered a tenant-wide "consent phishing" campaign. What cleanup, communication, and protection steps do you plan?**

* **Cleanup Steps:** Run an administrative script to **revoke all malicious app consents** across the tenant, and explicitly **block the malicious application ID** to stop future connections.
* **Communication Steps:** Distribute a clear security **user notification** to all affected employees, instructing them to update their credentials and reset active sessions.
* **Protection Steps:** Enforce an **admin consent workflow** to require administrative approval for all third-party software integrations, apply a restrictive **app consent policy**, and launch a targeted phishing security awareness campaign.

---

### C. Checklist-Style Questions (81–100)

* **[x] Difference between IaaS, PaaS, SaaS?** IaaS provides raw infrastructure (VMs); PaaS provides a managed application platform; SaaS provides fully functional, web-hosted software.
* **[x] What is the Shared Responsibility Model?** A framework that divides security tasks between the cloud provider and the customer based on the cloud service model.
* **[x] What is Conditional Access used for?** It serves as an identity evaluation engine that enforces granular access rules (like requiring MFA or blocking untrusted IPs) based on real-time signals.
* **[x] What does Defender for Endpoint do?** An EDR solution that monitors host endpoint behavior, detects advanced threats, and enables automated device remediation.
* **[x] What does Defender for Identity monitor?** It analyzes on-premises Active Directory network traffic and protocols to detect credential theft and identity compromises.
* **[x] What are the analytics rule types in Sentinel?** Scheduled rules, Near-Real-Time (NRT) rules, Fusion rules, and Machine Learning behavioral analytics rules.
* **[x] Difference between KQL `where`, `summarize`, `join`?** `where` filters rows based on a condition; `summarize` aggregates data fields into groups; `join` combines rows from two tables using matching keys.
* **[x] What does Key Vault store?** Secrets (passwords/tokens), Cryptographic Keys, and TLS/X.509 Certificates.
* **[x] NSG vs Azure Firewall?** An NSG is a basic Layer 4 packet filter applied at the subnet/NIC level; Azure Firewall is an intelligent, stateful Layer 3-7 network security service.
* **[x] What is a Private Endpoint?** A technology that assigns a private IP address from your VNet to an Azure service, removing its exposure to the public internet.
* **[x] Why is Bastion better than direct RDP?** It provides secure, browser-based RDP/SSH access over SSL/TLS, removing the need to assign public IPs to virtual machines.
* **[x] What is PIM used for?** Managing and securing privileged identity access by enforcing just-in-time, time-bound role elevation.
* **[x] SAML vs OAuth?** SAML is an XML-based authentication protocol used for enterprise SSO; OAuth is a token-based authorization framework used for API access delegation.
* **[x] What is a CASB?** A Cloud Access Security Broker; a security gateway used to govern data protection and security policies across corporate SaaS applications.
* **[x] What does CSPM do?** Continuously audits cloud resource configurations against security benchmarks to detect misconfigurations and configuration drift.
* **[x] What does Secure Score measure?** An organization's overall cloud security posture relative percentage, based on the implementation level of recommended security baselines.
* **[x] Where are audit logs stored?** Inside centralized data repositories, such as Azure Monitor Log Analytics Workspaces or secure Storage Accounts.
* **[x] Data residency vs sovereignty?** Data residency is the physical geographic location where data is stored; data sovereignty means the data is subject to the privacy laws of the nation where it is located.
* **[x] Why is Shadow IT dangerous?** It bypasses corporate security controls, introduces unmonitored data leak vulnerabilities, and violates compliance standards.
* **[x] What does SOAR mean?** Security Orchestration, Automation, and Response; a framework used to automatically aggregate security alerts, orchestrate tools, and execute automated response playbooks.

---

## 2. INTRO TO CYBER
*Source modules: Fundamentals of Cyberspace, Security Fundamentals, Cyber Kill Chain, Threat Hunting, DFIR, Digital Forensics, OSINT/Darknet/TOR, Penetration Testing, GRC*

### A. General Questions (1–40)

**1. What is cyberspace and how are its three layers (physical, logical, social) described?**

* **Cyberspace:** A global, interconnected environment of information technology infrastructures, networks, and data repositories.
* **Physical Layer:** The tangible hardware, cabling, satellite links, datacenters, and geographic locations.
* **Logical Layer:** The software, code, operating systems, cryptographic protocols, and network routing configurations that process data.
* **Social/Cognitive Layer:** The human identities, digital personas, user choices, and social dynamics of the actors operating across the networks.

**2. What does each component of the CIA triad mean? Give real examples.**

* **Confidentiality:** Restricting data access to authorized users only. *Example:* Enforcing AES-256 encryption on storage drives.
* **Integrity:** Ensuring data remains accurate, complete, and unaltered by unauthorized parties. *Example:* Implementing SHA-256 cryptographic hashing to verify file downloads.
* **Availability:** Guaranteeing timely, reliable access to systems and information for authorized personnel. *Example:* Deploying multi-datacenter Redundant Arrays of Independent Disks (RAID) and High Availability clusters.

**3. Explain the difference between threat, vulnerability, and risk.**

* **Threat:** Any potential negative event or agent (e.g., a malware payload or threat actor) that can exploit a flaw and cause harm.
* **Vulnerability:** A flaw, bug, or structural weakness within an environment's code, configuration, or physical controls.
* **Risk:** The statistical probability and financial impact of a threat successfully discovering and exploiting an active vulnerability.

**4. What is the main principle of defense in depth?**
The implementation of multiple, layered, redundant security controls across an organization's structural tiers (Physical, Network, Host, Application, Data). This ensures that if a single layer of defense fails, subsequent layers are positioned to contain and neutralize the threat.

**5. What is the Zero Trust model and how is it different from the traditional "castle-and-moat" approach?**
[Image comparing Traditional Castle-and-Moat vs Zero Trust Architecture]

* **Castle-and-Moat:** Relies on perimeter security. Anything inside the local corporate network perimeter is trusted by default, leaving the environment vulnerable to lateral movement if breached.
* **Zero Trust:** Shifts defense to a continuous evaluation framework governed by three principles: Assume Breach, Verify Explicitly, and Enforce Least Privilege. Location does not equal trust; every access request must be authenticated, authorized, and cryptographically validated regardless of its origin.

**6. Name the 7 stages of the Cyber Kill Chain in order.**

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command and Control (C2)
7. Actions on Objectives

**7. What is the difference between the Cyber Kill Chain and MITRE ATT&CK?**

* **Cyber Kill Chain:** A linear, chronological phase model mapping the structural steps an attacker must execute sequentially to complete an intrusion.
* **MITRE ATT&CK:** A non-linear, comprehensive matrix mapping granular real-world adversary tactics, specific techniques, and procedures (TTPs) based on actual threat observations.

**8. Give examples of passive and active techniques used in the reconnaissance phase.**

* **Passive Reconnaissance:** Gathering intelligence without interacting directly with target infrastructure. *Examples:* Querying public WHOIS databases, searching LinkedIn, and using Google Dorking.
* **Active Reconnaissance:** Direct engagement with target assets to map structure. *Examples:* Running Nmap port scans, banner grabbing, and web directory brute-forcing.

**9. What is the difference between weaponization and delivery?**

* **Weaponization:** An offline preparation stage where an attacker pairs an exploit payload with a common file format or tool (e.g., creating a malicious macro inside a Word document).
* **Delivery:** The transmission phase where the weaponized file or payload is launched against the target via specific communication vectors (e.g., spear-phishing emails, drive-by downloads, or malicious USB drives).

**10. What does Command and Control (C2) mean?**
The architecture of external servers, channels, and communication protocols established by an adversary to maintain persistent access and transmit remote execution commands to malware implants operating inside a compromised target network.

**11. What is threat hunting and how does it differ from incident response?**

* **Threat Hunting:** A proactive, hypothesis-driven security search across system telemetry to detect advanced threats that have bypassed existing perimeter controls and are lurking silently in the environment.
* **Incident Response:** A reactive process triggered by a concrete alert, security event, or threshold violation to contain, eradicate, and recover from an active compromise.

**12. How does hypothesis-driven threat hunting work?**

1. Formulate a logical theory regarding threat activity based on current threat intelligence or anomalous operational baselines.
2. Develop KQL/SQL queries to parse historical system logs and endpoint telemetry for matching behaviors.
3. Analyze the results to prove or disprove the theory, initiate remediation for any uncovered threats, and automate the query into a permanent detection rule.

**13. What is the difference between IOC (Indicator of Compromise) and IOA (Indicator of Attack)?**

* **Indicator of Compromise (IOC):** Reactive, historical forensic evidence confirming a past or ongoing breach. *Examples:* Known malicious file MD5 hashes, specific C2 IP addresses, or registry strings.
* **Indicator of Attack (IOA):** Proactive, behavioral evidence mapping real-time intent and execution strategies irrespective of tools. *Examples:* Code injection attempts, rapid local security group modifications, or unauthorized directory harvesting.

**14. What does DFIR mean and which two main functions does it combine?**

* **DFIR:** Digital Forensics and Incident Response.
* **Digital Forensics:** The scientific collection, preservation, analysis, and legal documentation of digital evidence post-incident.
* **Incident Response:** The operational mitigation, containment, eradication, and systemic restoration of enterprise business assets during a security breach.

**15. Name the 6 phases of the Incident Response process per the PICERL model.**

1. Preparation
2. Identification
3. Containment
4. Eradication
5. Recovery
6. Lessons Learned

**16. What is the difference between volatile and non-volatile evidence? Give examples.**

* **Volatile Evidence:** Temporary data stored in fast system memory that is permanently lost when power is removed. *Examples:* System RAM contents, active network socket states (`netstat`), and running process trees.
* **Non-Volatile Evidence:** Persistent data written to physical mediums that remains intact when power is cut. *Examples:* Solid-State Drives (SSDs), hard disks, registry files, and system event logs.

**17. What is Chain of Custody and why is it critical?**
A chronological, legally binding document tracking the collection, transfer, analysis, storage, and disposal of physical or electronic evidence. It preserves evidentiary integrity and prevents claims of tampering, ensuring the evidence remains admissible in court.

**18. Why is a write-blocker used in digital forensics?**
A hardware device or software bridge that prevents the forensic workstation from modifying, writing, or appending metadata to a piece of evidence during the data acquisition process, ensuring an exact bit-for-bit duplicate is captured.

**19. Per the Order of Volatility principle, which evidence should be collected first?**

Evidence with the shortest lifespan must be imaged first. The sequence follows:

1. CPU Cache and Registers
2. System RAM (volatile memory)
3. Network connection tables and process states
4. Local hard drives and persistent storage media
5. Remote logging streams and archival backups

**20. What is OSINT and which source types does it use?**

* **OSINT:** Open Source Intelligence.
* **Source Types:** Publicly available records, search engines, open code repositories (GitHub), social media profiles, public corporate filings, WHOIS databases, DNS maps, and Shodan scanning records.

**21. What is the difference between surface web, deep web, and dark web?**

* **Surface Web:** The standard internet indexable by public search engines (e.g., standard websites, news portals).
* **Deep Web:** Private networks and pages unindexed by search engines requiring direct authentication. *Examples:* Internal banking applications, paywalled medical databases, and email accounts.
* **Dark Web:** Intentional overlays and hidden networks hosted on specialized, encrypted routing infrastructures (e.g., Tor `.onion` networks) that require specific software configurations to access.

**22. How does the TOR network work (onion routing)?**

Anonymizes client source traffic by encrypting the data payload in multiple layers. It routes the data package through a sequence of three randomly selected relays:

1. **Guard/Entry Node:** Knows the client's real IP but only strips the first layer of encryption to see the Middle Node address.
2. **Middle Node:** Receives encrypted data, strips the next layer, and only passes traffic to the Exit Node.
3. **Exit Node:** Strips the final layer of encryption and routes unencrypted traffic to its target destination, ensuring no single point can connect the source IP to the final destination.

**23. What are 3 main weaknesses that can break TOR anonymity?**

1. **Malicious Exit Node Monitoring:** Adversaries running the Exit Node can sniff unencrypted HTTP traffic payloads.
2. **Traffic Correlation / Timing Attacks:** Large nation-states measuring traffic arrival times entering the entry node versus leaving the exit node can use statistical analysis to match identities.
3. **Endpoint Application Leaks:** Browser configuration flaws, out-of-date plug-ins, or local system exploits that force connections outside the Tor loop, leaking the client's public IP.

**24. Difference between penetration test types: black box, white box, grey box?**

* **Black Box:** The tester acts with no prior knowledge of target architecture, configurations, or source code.
* **White Box:** The tester receives full architectural diagrams, API schemas, user credentials, and source code availability.
* **Grey Box:** The tester receives partial target environment insights, typically mimicking a standard authenticated insider or customer.

**25. What is the difference between a vulnerability scan and a penetration test?**

* **Vulnerability Scan:** An automated, broad programmatic sweep designed to list known unpatched vulnerabilities and configuration bugs without testing execution paths.
* **Penetration Test:** A manual, targeted active evaluation where professionals try to exploit identified vulnerabilities to gauge breach depth, pivot capabilities, and business impact.

**26. What are the main phases of a penetration test process?**

1. Planning and Scoping (defining Rules of Engagement)
2. Reconnaissance (information gathering)
3. Vulnerability Assessment (scanning and mapping entry points)
4. Exploitation (gaining access)
5. Post-Exploitation (pivoting, privilege escalation, and persistence evaluation)
6. Reporting and Remediation review

**27. What is the difference between red, blue, and purple teams?**

* **Red Team:** Offensive security professionals who simulate advanced adversary campaigns to test an organization's detection and response capabilities.
* **Blue Team:** Defensive security personnel who manage monitoring, logs, EDR tooling, and incident triage to block and respond to attacks.
* **Purple Team:** A collaborative operational framework where red and blue teams work together in joint exercises to optimize detection rules, close visibility gaps, and improve response times.

**28. What is the difference between a bug bounty and a pentest?**

* **Penetration Test:** A time-bound, structured assessment focused on a predefined scope, executed by a vetted team for a fixed fee regardless of findings.
* **Bug Bounty:** A continuous, crowdsourced public or private program where independent researchers are compensated with financial rewards only when they report valid, unique security bugs.

**29. Briefly explain Governance, Risk Management, and Compliance (GRC).**

* **Governance:** The oversight structures, corporate policies, and strategic alignments that ensure cybersecurity supports business goals.
* **Risk Management:** The systematic identification, analysis, evaluation, and operational treatment of information security liabilities.
* **Compliance:** The procedural verification that the organization adheres to external legal statutes, contractual rules, and industry standard benchmarks (e.g., GDPR, PCI-DSS).

**30. Explain the difference between risk transfer, mitigation, acceptance, and avoidance with examples.**

* **Risk Transfer:** Shifting the financial burden of an identified risk to an external third party. *Example:* Purchasing a comprehensive cyber insurance policy.
* **Risk Mitigation:** Implementing technical or administrative controls to reduce risk likelihood or impact. *Example:* Installing an EDR solution across all endpoints.
* **Risk Acceptance:** Choosing to absorb a potential risk because the cost of fixing it exceeds the asset's value. *Example:* Documenting and allowing a legacy offline testing server to run without modifications.
* **Risk Avoidance:** Eliminating the risk entirely by stopping the activity or removing the component. *Example:* Shutting down a high-risk web application that is no longer core to operations.

**31. Difference between qualitative and quantitative risk analysis?**

* **Qualitative:** Uses subjective, experience-based metrics (e.g., scoring risk levels as High, Medium, or Low) based on impact and likelihood matrices.
* **Quantitative:** Uses objective, empirical financial and mathematical computations to assess risk, assigning concrete monetary numbers to potential losses.

**32. Explain the SLE, ARO, and ALE formulas.**

* **SLE (Single Loss Expectancy):** The monetary loss expected from a single security incident. $\text{SLE} = \text{Asset Value} \times \text{Exposure Factor (EF)}$.
* **ARO (Annualized Rate of Occurrence):** The statistical frequency an incident is expected to happen within a single year.
* **ALE (Annualized Loss Expectancy):** The projected annual financial impact of a specific risk. $\text{ALE} = \text{SLE} \times \text{ARO}$.

**33. Give examples of administrative, technical, and physical controls.**

* **Administrative:** Password complexity policies, background checks, and annual employee security awareness training.
* **Technical:** Firewalls, Endpoint Detection and Response (EDR) agents, and multi-factor authentication (MFA) gateways.
* **Physical:** Biometric turnstiles, security guards, concrete mantrap gates, and CCTV monitoring networks.

**34. Difference between preventive, detective, and corrective controls?**

* **Preventive:** Controls designed to proactively block an unauthorized or malicious action from executing. *Example:* An explicit firewall rule dropping incoming traffic from untrusted sources.
* **Detective:** Controls that identify, log, and alert on unauthorized or malicious actions during or after execution. *Example:* An active SIEM alert flagging unusual data exfiltration activity.
* **Corrective:** Controls that limit damage and restore systems to an operational baseline after an incident. *Example:* Rebuilding compromised systems from secure, offline backups.

**35. What is ISO 27001 for and how does it differ from PCI DSS?**

* **ISO 27001:** An international, comprehensive management framework that outlines the requirements for establishing, implementing, and continually improving an Information Security Management System (ISMS) across any organization.
* **PCI DSS:** A technical and operational standard focused strictly on protecting the cardholder data environment (CDE) for entities that process, store, or transmit credit card data.

**36. What are the main requirements of GDPR?**
The General Data Protection Regulation mandates explicit user consent for data collections, enforces the Right to Erasure (the "right to be forgotten"), requires data minimization and privacy by design, and mandates a maximum 72-hour notification window to supervisory authorities following a data breach.

**37. Difference between strategic, tactical, and operational threat intelligence?**

* **Strategic Threat Intelligence:** High-level, long-term trends and analysis regarding macro risk landscapes and geopolitical actors, written for executive and board visibility.
* **Tactical Threat Intelligence:** Deep insights mapping current attacker methodologies, tool selections, and specific MITRE ATT&CK TTPs, used by security defenders to build detection logic.
* **Operational Threat Intelligence:** Direct, technical real-time telemetry inputs (such as bad IP lists, malware file hashes, or specific domain rules) ingested by SIEM and EDR platforms to accelerate automated parsing.

**38. What is an APT and how does it differ from regular malware?**

* **Regular Malware:** Opportunistic, non-targeted automated software designs seeking quick monetization (such as generic ad injection or mass credential sweeps).
* **APT (Advanced Persistent Threat):** Highly funded, often state-sponsored or organized hacking syndicates that launch customized, targeted campaigns against specific organizations. They prioritize long-term stealth, persistence, and intelligence gathering over immediate disruption.

**39. Name the 5 most common types of social engineering attacks.**

1. Phishing
2. Baiting
3. Pretexting
4. Quid Pro Quo
5. Tailgating

**40. Difference between phishing, spear phishing, and whaling?**

* **Phishing:** Broad, un-targeted malicious email campaigns sent to thousands of recipients hoping a small percentage will click.
* **Spear Phishing:** Highly targeted malicious emails customized using research data to target a specific individual, department, or company.
* **Whaling:** A highly specialized sub-category of spear phishing directed strictly at high-profile senior executives (such as CEOs, CFOs, or Board members).

---

### B. Scenario-Based Questions (41–80)

**41. A user clicked a suspicious link and strange processes appeared. How would you analyze the event from a Cyber Kill Chain perspective?**
Map the link click to the **delivery** phase, while the execution of underlying shellcode represents **exploitation**. The subsequent execution of rogue background files maps to the **installation** phase. If these processes open an external network connection to an unknown server, classify it as **C2 (Command and Control)** activity. If they attempt data scraping, record it as **action on objectives**. Initiate immediate **IR steps** by isolating the host, gathering system logs, and beginning formal triage.

**42. During an incident a manager tells you "just restart it". How do you respond?**
"Restarting the machine will cause an irreversible loss of **volatile evidence**, such as active RAM data, running process injections, and current network connection paths. This directly violates the **Identification phase** of our incident response framework. We must maintain proper **evidence preservation** protocols to isolate the root cause, assess the potential **business impact**, and coordinate formal **incident communications** before executing any system resets."

**43. The CIO asks you for the first time to prepare a risk register. In what order would you act?**

1. Build a comprehensive corporate hardware, software, and data **asset inventory**.
2. Perform a targeted **threat modeling** review to identify vulnerabilities facing those assets.
3. Apply likelihood and impact values to calculate quantitative and qualitative **risk scoring**.
4. Assign individual risk items to designated **risk owners**.
5. Document mitigation plans within a formal **risk treatment plan**.
6. Establish a continuous **review cadence** to track remediation progress and risk updates.

**44. You are doing OSINT on a company. Which 5 sources would you start with?**
Query public **WHOIS** records to check domain registration properties, and search **LinkedIn** to analyze employee structures and technologies mentioned in job listings. Scan corporate public **GitHub** repositories for exposed keys or hardcoded API credentials, check **Shodan** to map internet-facing ports and active services, and audit **Certificate Transparency logs** to build a comprehensive list of corporate subdomains.

**45. During a pentest the client wants to change scope at the last moment. How do you proceed?**
Stop all active testing campaigns affecting the modified zones and review the signed **Rules of Engagement (RoE)**. Insist on securing formalized, **written approval** and a signed contract addendum outlining the updated scope parameters to mitigate **legal risks** and clarify liability boundaries. Adjust the formal **retest plan** to mirror these changes and implement controls to prevent unauthorized **scope creep**.

**46. A threat intel report discloses new APT29 TTPs. Which protection tests do you run in order at your company?**
Map the newly discovered adversary behaviors directly against the **MITRE ATT&CK framework**. Conduct an internal audit of existing logging profiles to identify **detection coverage gaps**. Execute simulated, controlled attacks using automated **atomic tests** (such as Atomic Red Team) to evaluate defenses. Use the findings to tune **EDR rules** and prioritize **gap remediation** tasks to update security coverage.

**47. A user's password leaked on the dark web. What steps must follow?**
Enforce an immediate, mandatory **credential rotation** across all corporate and domain networks. Verify that multi-factor authentication (**MFA enforcement**) is fully operational on the account, and search SIEM and firewall logs for matching account-driven **IOC entries** or anomalous logins. Audit the environment for potential **reused passwords** on secondary services, and enroll the user in targeted security **awareness training**.

**48. The company says "we're prepping for a PCI DSS audit" but has no cardholder data inventory. What is your first step?**
Initiate a structured **scoping** exercise to map where primary account numbers are handled. Construct a detailed **data flow diagram** that tracks the ingestion, storage, and transmission paths of credit card data. Enforce strict **network segmentation** controls to isolate the Cardholder Data Environment (CDE) from the rest of the corporate network, determine whether a Self-Assessment Questionnaire (**SAQ**) or Report on Compliance (**ROC**) audit is required, and implement **compensating controls** where gaps exist.

**49. A host is suspected of C2 traffic. What is your forensic triage plan?**
Capture a full **forensic memory image** of system RAM before altering its operational state. Run `netstat` and review the active network **connection list** to map out unusual outbound sockets. Reconstruct the system **process tree** to locate hidden injections, analyze network traffic for consistent **beaconing patterns**, and inspect historical local **DNS query lookups**. Finally, network **isolate** the machine to prevent lateral movement while preserving data for investigation.

**50. An org says "we have backups, we're safe". How do you challenge this using a ransomware scenario?**
"Traditional backups are a frequent target for modern attackers. If your environment lacks **immutable storage** configurations or completely **offline, air-gapped copies**, ransomware operators will locate and encrypt your backups first. Additionally, the **3-2-1 rule** cannot protect you against extortion schemes if the attacker uses **exfiltration leverage** to threaten public data leaks, or if your enterprise has not verified its real-world **restore times** under pressure."

**51. Dark web monitoring shows the company's domain being sold. How do you proceed?**
Initiate steps to **validate the threat source** and confirm the authenticity of the listing. Launch an internal investigation to determine the **scope of exposure** and check for compromised assets or credentials. Coordinate with domain registrars and external legal teams to execute corporate **takedown options**, formally **declare a security incident**, and draft necessary **customer notification** templates to satisfy regulatory obligations.

**52. A new CISO asks "what are the 5 most important quick wins in 3 months?". Your answer?**

1. Mandate **MFA enforcement** across all remote access vectors, email channels, and administrative interfaces.
2. Deploy a centralized **EDR agent solution** with active blocking modes to all workstations and servers.
3. Establish automated, risk-prioritized infrastructure **patch management** loops.
4. Deploy comprehensive visibility profiles over core business assets to establish **asset visibility**.
5. Implement hardened, **immutable backup architectures** to secure disaster recovery routes.

**53. A team member says "we have no attack risk, we're small". How do you frame your answer with facts?**
"Adversaries rely heavily on automated, opportunistic port scanning to locate soft targets, meaning small businesses are frequently hit regardless of size. Industry **SMB ransomware statistics** show that a high percentage of small organizations go bankrupt following a major data breach. Furthermore, attackers exploit small vendors as low-barrier entry points to launch **supply chain attacks** against their larger enterprise partners, making basic cyber hygiene a high-value business investment."

**54. An employee suddenly downloads a large volume of files (potential insider threat). How do you investigate?**
Use user and entity behavioral analytics (**UEBA**) tools to verify if the file download volume deviates significantly from the user's historical **peer baseline**. Cross-reference the activity with data loss prevention (**DLP**) rule metrics to check if data was copied to external storage or personal cloud drives. Contact the user's management to check for a valid **business justification**, loop in **HR and Legal departments** to maintain compliance, and collect all log telemetry using **forensic evidence handling** standards.

**55. A new intern asks, "why is threat hunting different from looking at alerts?" Give the answer.**
"Alert triage is a **reactive** process triggered only after an established security tool matches an activity against a known signature or rule threshold. Threat hunting is a **proactive** technique where analysts assume a breach has already occurred. Hunters form behavioral **hypotheses** to look for subtle, low-fidelity signals that bypass standard security filters, focusing on significantly reducing attacker **dwell time** before a major incident occurs."

**56. During a pentest you found a critical RCE; the client manager asks you to stay quiet. What is your ethical and professional response?**
Refuse the request to hide the vulnerability. Point directly to the terms of disclosure within the pre-signed **written agreement**, which dictates an immediate **disclosure obligation** for vulnerabilities that present severe, imminent operational risks. Document the vulnerability details using standard **CVSS scoring** metrics and **escalate the findings** directly to executive leadership to preserve professional **integrity** and contract terms.

**57. During an incident, logs are scattered across 3 systems. What approach do you recommend for an effective investigation?**
Recommend the immediate ingestion of all distributed data into a centralized SIEM platform to establish a **single pane of glass**. Reconstruct a comprehensive, chronological  incident timeline, apply **log normalization** policies to standardize varying syntax types, and implement **event correlation** rules to connect actions across systems and trace the attacker's path.

**58. A vendor sells you "AI-powered EDR". What questions test the reality?**

* "Can you demonstrate your tool's explicit **detection coverage mapping** against the MITRE ATT&CK matrix?"
* "What baseline **false-positive (FP) rates** does this engine produce in an enterprise environment, and how granular is its rule **tunability**?"
* "Does the AI analysis engine execute on the local endpoint, or is it completely dependent on a continuous connection to your **cloud infrastructure**?"
* "What specific automated **endpoint response actions** can the agent run autonomously if it identifies zero-day behavior?"

**59. An employee found a USB at the office and wants to plug it in. How do you warn them?**
"Never connect unknown storage drives to any computer. Rogue USB drives can be engineered to execute malicious **Human Interface Device (HID) injections**, where the device emulates a virtual keyboard to type out and execute malicious code in seconds. They can also be part of a deliberate **USB drop attack** designed to bypass perimeter controls. Disabling OS **autorun settings** cannot fully block hardware-level exploits, so you must turn the drive over to the **security team** immediately."

**60. A compliance auditor asks "why is encryption still weak?". How do you frame risk meaningfully (language and metrics)?**
Document the exact system exceptions within the corporate **risk register**, outlining the concrete **business impact** and financial exposure associated with the legacy configuration. Define the remaining **residual risk** limits, outline a structured remediation timeline within a formalized **risk treatment plan**, and assign clear, documented **owner accountability** to a business stakeholder to ensure visibility.

**61. A threat intel feed lists your company's IP as C2. What is your response?**
Initiate validation steps to cross-verify the context of the external threat report and confirm corporate **IP ownership parameters**. Launch an immediate internal **reverse hunt** across SIEM and network logs to check for compromised internal systems beaconing out from that address. If the IP is spoofed or misattributed, coordinate with the feed maintainer to log it as a **false positive**; if verified, initiate **incident isolation** and containment.

**62. The company asks "shouldn't we keep all emails for 7 years?". How do you answer legally and operationally?**
"Retaining all corporate email communications indiscriminately creates significant data liabilities. Under modern **privacy regulations** like GDPR, organizations must adhere to strict **data minimization** mandates, which require data to be purged once its business utility ends. Storing unnecessary data inflates corporate **storage infrastructure costs** and increases exposure risks during a breach. We should implement a targeted **retention policy** that applies selective **legal holds** only to specific accounts or regulatory data profiles."

**63. As a threat hunter, which log sources do you check for "pass-the-hash" traces?**
Query Windows Security event logs to check for **Event ID 4624 using Logon Type 3 (Network)** combined with explicit **NTLM authentication** protocols. Use **EDR telemetry** to monitor for unauthorized access attempts directed at the **LSASS process memory space**, and build analytics rules to catch credentials being used from **unusual source endpoints** that deviate from historical access patterns.

**64. In an incident, "patient zero" must be identified. Which techniques do you use?**
Audit your **email gateway logs** to trace inbound spear-phishing messages, and reconstruct the entry timeline using granular **EDR endpoint events**. Analyze **parent-child process relationships** to pinpoint the exact moment an application spawned a malicious shell, and trace network traffic records backward through **lateral movement paths** to identify the initial point of compromise.

**65. A vendor asks you to label a "critical" finding as "high" in the pentest report. Your response?**
Refuse to downgrade the severity rating. Emphasize that your findings are calculated using objective, transparent **CVSS metrics** to ensure professional **integrity** and contract compliance. Explain that changing the severity assessment misrepresents the organization's actual risk posture, and formally **escalate the matter** to executive leadership if pressure continues.

**66. How should a vulnerability disclosure process be structured?**
Establish a secure, monitored communication **intake portal** for external security researchers. Implement a consistent **takedown/triage workflow** to filter incoming submissions, and apply standard **CVSS scoring** rules to map vulnerability severity. Enforce internal **remediation SLAs** to resolve verified bugs, maintain open lines of communication with the reporter, and publish a **coordinated public advisory** once patches are deployed.

**67. How do you perform threat modeling for a new web application (STRIDE)?**
Construct a comprehensive application **data flow diagram (DFD)**. Systematically analyze every data boundary, process, and datastore against the six components of the **STRIDE framework**:

* **S**poofing (Identity verification controls)
* **T**ampering (Data integrity protection)
* **R**epudiation (Audit trails and logging)
* **I**nformation Disclosure (Data confidentiality and encryption)
* **D**enial of Service (Availability and rate-limiting)
* **E**levation of Privilege (Authorization and access controls)

**68. A company says "awareness training isn't effective". How would you measure and improve it?**
Track long-term behavioral metrics rather than basic completion rates. Monitor your baseline **phishing simulation failure rates**, isolate metrics for **repeat offenders** to deliver focused coaching, and track the organization's **click-to-report ratio**. Tailor the educational content directly to specific business departments (e.g., targeting finance teams with deep-dive business email compromise scenarios) to make training more relevant.

**69. You're drafting an IR Plan for a new org. Which 6 phases and what artifacts per phase do you include?**
Structure the master framework around the **PICERL model**, defining clear deliverables for each operational phase:

* **Preparation:** Corporate incident response policies, technical **runbooks**, a documented **contact tree**, and an approved forensic **tools list**.
* **Identification:** Alert triage log procedures and centralized **incident intake templates**.
* **Containment:** Network isolation playbooks, firewall blocklist templates, and crisis **communication templates**.
* **Eradication:** Malware removal scripts and system patching verification checklists.
* **Recovery:** Server verification checklists and data restoration validation logs.
* **Lessons Learned:** A formalized **Post-Incident Report (PIR)**.

**70. An admin says "we have VPN, no need for MFA". Strengthen your reply with a real scenario.**
"A VPN without MFA creates a single point of failure. If an adversary steals an administrator's VPN credentials via an infostealer infection or targeted phishing campaign, they gain direct access to the internal network perimeter. This allows them to bypass perimeter controls, initiate **lateral movement**, and deploy ransomware across the datacenter. Implementing a defense-in-depth framework requires multi-factor authentication to protect access points."

**71. Is "bit-for-bit" imaging important in forensics? Why?**
Yes. Generating a bit-for-bit cryptographic clone captures the entire storage medium, including **deleted files, unallocated clusters, and slack space** that standard file copies miss. Forensic software uses this process to calculate matching **cryptographic hashes** (e.g., SHA-256) across the drive, proving data integrity and ensuring the evidence remains **admissible in court**.

**72. Why should the post-incident lessons learned session never be skipped?**
The post-incident review is essential for driving **continuous process improvement**. Skipping this step causes organizations to miss systemic **security control gaps**, leading to **repeat incidents** from the same root causes. It also helps identify team training needs and provides executive leadership with visibility into security performance to guide future budget allocations.

**73. A social engineering attack uses OSINT. As defender, which traces can be minimized?**
Enforce strict corporate **employee privacy hygiene policies** regarding what corporate details can be shared on public social networks. Implement automated gateway tools to **strip metadata** fields from public corporate documents and PDF attachments. Monitor executive digital exposure profiles, deliver role-tailored anti-phishing training to high-risk personnel, and run regular **OSINT footprint scans** to locate and remove exposed corporate information.

**74. An intern asks "why can't we connect to the network from anywhere?". Answer using Zero Trust principles.**
"Under a Zero Trust architecture, physical location no longer guarantees security trust. Before granting access to any resource, the system must explicitly validate your digital **identity** and evaluate your endpoint's real-time **device posture** to confirm it runs updated patches and active EDR tools. Access is tightly restricted using **least privilege** controls and network **micro-segmentation**, ensuring connections are continually verified rather than blindly trusted based on location."

**75. When writing a pentest report, why is the remediation recommendation considered as important as the finding?**
Remediation recommendations deliver the primary **business value** of a penetration test. They provide administrators with actionable, risk-prioritized steps to fix security gaps, assign clear **owner accountability**, and establish measurable criteria to verify that vulnerabilities have been successfully closed during subsequent **retests**.

**76. During an incident the media is interested. What position should the company take?**
Activate the pre-approved corporate **crisis communications plan** and route all media inquiries through a single, designated **corporate spokesperson**. Ensure all public statements undergo strict **legal review** to prevent liability exposure, share only confirmed **forensic facts**, and coordinate releases to comply with mandatory regulatory disclosure timelines.

**77. After a pentest you have 200+ findings; the client is stressed. How do you prioritize?**
Sort the vulnerabilities objectively using **CVSS scoring** metrics, then cross-reference the results against real-world **exploitability** and the organization's unique **business context** (such as asset criticality). Group individual vulnerabilities by their underlying **root-cause commonalities** (e.g., broad patching failures or missing headers) so developers can deploy fixes efficiently and secure quick wins.

**78. An intern asks "what's the difference between digital forensics and IR?". Write a simple answer.**

* **Incident Response (IR):** A time-bound, operationally focused containment process aimed at stopping an active attack and recovering business operations as quickly as possible.
* **Digital Forensics:** A deep investigative process focused on collecting and preserving digital evidence to determine the exact root cause of a breach and maintain legal integrity for potential court proceedings.

**79. An auditor asks for your ATT&CK coverage map. How would you prepare it?**
Ingest your corporate security detection tool inventory directly into the **MITRE ATT&CK Navigator** web application. Construct a color-coded **heatmap visualization** where different shades represent active prevention, logging, or alerting capabilities. Identify remaining security **telemetry gaps** and map out a remediation plan to build out missing coverage.

**80. At which stage of the cyber kill chain is stopping the attack most effective, and why?**
The **Reconnaissance and Delivery stages**. Intercepting an attack during these initial phases stops the threat before exploitation or installation can occur. This keeps remediation costs low, avoids operational downtime, and prevents attackers from gaining a foothold inside the network.

---

### C. Checklist-Style Questions (81–100)

* **[x] What is the CIA triad?** The foundational security model consisting of Confidentiality, Integrity, and Availability.
* **[x] Threat vs vulnerability vs risk?** A threat is the external danger; a vulnerability is the internal flaw; risk is the calculated likelihood and impact of that flaw being exploited.
* **[x] What is defense in depth?** The strategy of layering multiple, redundant security controls across an organization to eliminate single points of failure.
* **[x] What is Zero Trust?** A security framework built on the principle of "never trust, always verify," requiring continuous authentication for all access requests.
* **[x] Kill Chain phases?** Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control, and Actions on Objectives.
* **[x] What is MITRE ATT&CK?** A globally accessible knowledge base of real-world adversary tactics, techniques, and procedures (TTPs).
* **[x] Passive vs active reconnaissance?** Passive recon uses public sources without directly touching target infrastructure; active recon directly scans and interacts with target systems.
* **[x] Threat hunting vs IR?** Threat hunting is a proactive search for hidden threats; Incident Response is a reactive process triggered by active alerts.
* **[x] IOC vs IOA?** An IOC provides forensic evidence of a past compromise (e.g., a file hash); an IOA tracks real-time behavior and intent during an active attack.
* **[x] PICERL phases?** Preparation, Identification, Containment, Eradication, Recovery, and Lessons Learned.
* **[x] Order of Volatility?** The prioritized sequence of digital evidence collection based on data lifespan, starting with CPU cache and RAM before moving to hard drives.
* **[x] What is Chain of Custody?** Chronological documentation that tracks the handling and transfer of evidence to preserve its integrity for legal proceedings.
* **[x] OSINT source examples?** Shodan, WHOIS records, public GitHub repositories, LinkedIn profiles, and Certificate Transparency logs.
* **[x] How does TOR onion routing work?** It encrypts traffic in multiple layers and routes it through three random nodes (Entry, Middle, Exit), hiding the user's source IP from the destination website.
* **[x] Black/white/grey box pentest difference?** Black box starts with zero prior knowledge; white box provides full access to diagrams and code; grey box gives partial information, mimicking a standard user.
* **[x] Red/blue/purple team roles?** Red teams simulate offensive attacks; blue teams manage defensive security operations; purple teams facilitate collaboration to optimize detection rules.
* **[x] What are SLE, ARO, ALE?** Quantitative risk formulas where Single Loss Expectancy $\times$ Annualized Rate of Occurrence $=$ Annualized Loss Expectancy.
* **[x] Preventive vs detective vs corrective control?** Preventive blocks threats; detective alerts on active security events; corrective repairs damage and restores systems post-incident.
* **[x] Phishing vs spear phishing vs whaling?** Phishing is mass, un-targeted email malicious sweeps; spear phishing targets a specific individual or company; whaling targets senior executives.
* **[x] GDPR main requirements?** Mandates user consent for data processing, enforces the right to erasure, requires data minimization, and dictates a maximum 72-hour breach notification window.
---

## 3. LINUX
*Source modules: Intro to Linux, Installation, CLI Basics 1–3, Users & Groups, Ownership & Permissions, Sudo, Text Processing, Regex, Network Config, Bash Scripting*

### A. General Questions (1–40)

**1. What is the Linux kernel and how does it differ from the OS?**

* **Linux Kernel:** The core architectural component of the system that interacts directly with hardware, managing CPU allocation, memory space, file system operations, and device peripheral interactions.
* **Operating System (OS):** The entire functional ecosystem, which bundles the Linux kernel alongside userland utilities, system libraries, package managers, and command-line shells.

**2. What does a Linux distribution mean and name the popular distribution families.**

* **Linux Distribution (Distro):** A custom operating system build that takes the upstream Linux kernel and packages it with specific configuration files, package management systems, and default applications.
* **Popular Families:** * Debian-based (Ubuntu, Linux Mint, Kali Linux)
* Red Hat-based (RHEL, Fedora, Rocky Linux, AlmaLinux)
* Arch-based (Arch Linux, Manjaro)
* SUSE-based (openSUSE, SLES)



**3. Why are Linux servers widely used from a security standpoint?**

* **Open-Source Auditing:** Publicly visible source code allows rapid distributed vulnerability identification and patching.
* **Strict Privilege Separation:** Granular Discretionary Access Control (DAC) models isolate system accounts.
* **Mandatory Access Control (MAC):** Advanced native kernel sandboxing sub-frameworks (SELinux, AppArmor) strictly contain process capabilities.
* **Minimalist Attack Surface:** Servers typically drop visual environments (headless execution builds) to eliminate extraneous vulnerable code vectors.

**4. What are /etc, /var, /home, /tmp, /usr, /opt directories for?**

* `/etc`: Stores system-wide static configuration files and deployment scripts.
* `/var`: Holds dynamic variable files that change during system runtime (system logs, databases, mail spools).
* `/home`: Contains the personal storage directories, configurations, and profiles for non-root users.
* `/tmp`: Offers temporary scratch storage space, accessible by all users, routinely wiped during system reboots.
* `/usr`: Houses user-facing executable binaries, read-only system libraries, documentation, and source configurations.
* `/opt`: Acts as an installation directory for monolithic, third-party unmanaged proprietary software packages.

**5. What is the difference between /etc/passwd and /etc/shadow?**

* `/etc/passwd`: A world-readable text database tracking primary user account configuration metadata (username, UID, primary GID, home directory path, default shell).
* `/etc/shadow`: A highly restricted file (accessible only by root/shadow) that stores secure user password hashes encrypted with modern salting schemes (e.g., SHA-512, bcrypt), along with account expiration parameters.

**6. What does UID 0 represent and why is it dangerous?**

* **UID 0:** Represents the `root` superuser account.
* **Danger:** UID 0 completely bypasses standard Discretionary Access Control checks. An attacker or single faulty script operating under UID 0 can manipulate kernel memory, alter low-level configuration maps, clear transaction records, or completely destroy file system partitions.

**7. What is the difference between primary and secondary groups?**

* **Primary Group:** The single default group assigned to a user account (defined in `/etc/passwd`). Any file or directory created by the user is automatically assigned to this group.
* **Secondary (Supplementary) Groups:** Auxiliary groups a user is appended to (defined in `/etc/group`) to grant supplemental read, write, or execute permissions to targeted shared resources.

**8. What does chmod 755 mean? Explain each digit.**

It establishes file access controls using octal permission values applied across three distinct authorization blocks:

* **7 (First Digit - Owner):** Read (4) + Write (2) + Execute (1) = Full control permissions.
* **5 (Second Digit - Group):** Read (4) + No Write (0) + Execute (1) = Read and execute permissions.
* **5 (Third Digit - Others):** Read (4) + No Write (0) + Execute (1) = Read and execute permissions.

**9. Why is chmod 777 bad practice?**
It opens maximum world-writable permissions (`rwxrwxrwx`). Any local unprivileged execution process, compromised system service, or guest user can overwrite, malicious-inject, or permanently delete the target asset.

**10. What is the difference between chown and chgrp?**

* `chown`: Alter the owner user account assignments of a file or folder (and can optionally change the group alignment simultaneously).
* `chgrp`: Exclusively alters the target file or directory's group ownership alignment.

**11. What security purposes do setuid, setgid, and sticky bit serve?**

* **setuid (SUID / Octal Value 4000):** Forces a binary file to execute under the privilege context of the file owner rather than the user invoking the command.
* **setgid (SGID / Octal Value 2000):** Forces execution under the file's group context, or ensures files newly created inside a directory inherit that parent folder's group identity.
* **Sticky Bit (Octal Value 1000):** Restricts file modifications within a directory, ensuring only the explicit creator of a file can rename or delete it.

**12. Why is the sticky bit set on /tmp?**
Because `/tmp` is a shared, world-writable ecosystem (`777`). Without the sticky bit, any low-privileged user could delete or overwrite crucial temporary data packages belonging to other users or system processes.

**13. What is the difference between a hard link and a soft (symbolic) link?**

* **Hard Link:** A duplicate directory entry that points directly to the exact underlying asset file inode. It shares data instantly, remains valid if the initial filename is wiped, and cannot cross file system partition boundaries.
* **Soft Link (Symlink):** A lightweight pointer file containing a path string pointing to another filename. It can easily cross separate file system partitions but breaks if the original source file is renamed or moved.

**14. Why is /etc/sudoers critical and why is visudo better than direct editing?**

* `/etc/sudoers`: Dictates the strict role-based access configurations governing which users can run administrative commands as root.
* `visudo`: It performs active syntax error validation before saving modifications. This prevents syntax errors that could lock all administrative accounts out of the system.

**15. What is the difference between sudo and su?**

* `sudo`: Runs a single targeted task (or drops to a privileged shell) utilizing the invoking user's own password for validation, tracked via audit logging.
* `su`: Switches the terminal environment entirely to the targeted account context (usually root), requiring the password of that specific target account.

**16. What do the bash `>`, `>>`, `<`, `|`, `&` operators do?**

* `>`: Redirects standard output data streams into a file, completely overwriting existing contents.
* `>>`: Appends standard output data streams onto the end of an existing file.
* `<`: Redirects file contents into a command as standard input data.
* `|`: Pipes the standard output stream of the preceding command directly into the standard input stream of the subsequent command.
* `&`: Instructs the shell processor to run the command asynchronously in the background.

**17. Difference between stdin, stdout, stderr? File descriptor numbers?**

* `stdin` (File Descriptor 0): Stream interface accepting data input into a process.
* `stdout` (File Descriptor 1): Stream interface routing standard output data emitted from a process.
* `stderr` (File Descriptor 2): Stream interface reserved for isolating runtime error messages emitted from a process.

**18. Difference between grep and grep -E (egrep)?**

* `grep`: Parses text using Basic Regular Expressions (BRE), requiring backslashes to escape structural metacharacters like `?`, `+`, `{`, `|`, `(`, and `)`.
* `grep -E`: Invokes Extended Regular Expressions (ERE), allowing regular expression operators to be evaluated natively without backslashes.

**19. When is sed used vs awk?**

* `sed`: A stream editor optimized for localized, line-by-line string substitutions, deletions, and basic file filtering.
* `awk`: A full text-processing programming language optimized for parsing data fields organized into columns or separated by distinct character delimiters.

**20. Difference between find and locate?**

* `find`: Performs a live search of the live filesystem structure in real-time. It is highly accurate and flexible but slower and resource-intensive.
* `locate`: Queries an indexed, pre-compiled system file location path database (`locatedb`). It is nearly instantaneous but can miss newly added files if the index has not been updated via `updatedb`.

**21. What do regex metacharacters `.`, `*`, `+`, `?`, `^`, `$` do?**

* `.`: Matches exactly one single character except a newline.
* `*`: Matches zero or more repetitions of the preceding character or group expression.
* `+`: Matches one or more repetitions of the preceding expression element.
* `?`: Matches zero or exactly one occurrence of the preceding expression block.
* `^`: Anchor token pinning matching logic strictly to the beginning of a line text block.
* `$`: Anchor token pinning matching logic strictly to the absolute end of a line text block.

**22. What do bash variables `$?`, `$#`, `$@`, `$$` mean?**

* `$?`: Captures the numerical exit status code generated by the most recently executed command.
* `$#`: Counts the total number of positional parameters or arguments passed into a script.
* `$@`: Expands out to all positional parameters passed into a script as distinct, individually quoted arguments.
* `$$`: Evaluates to the unique Process ID (PID) of the current active shell runtime environment.

**23. Why is the shebang (`#!/bin/bash`) important in a bash script?**
It acts as a kernel instruction. When a script file is executed, the shebang tells the operating system's loader process which binary interpreter path to run to parse and execute the script's instructions.

**24. What do the process states (R, S, D, Z, T) mean?**

* `R` (Running/Runnable): Process actively using CPU resources or waiting in the execution scheduler queue.
* `S` (Interruptible Sleep): Waiting for an event, system signal, or resource allocation.
* `D` (Uninterruptible Sleep): Typically waiting on kernel hardware disk I/O operations; cannot be interrupted by signals.
* `Z` (Zombie / Defunct): Terminated process whose parent has not yet read its exit state via a `wait()` system call.
* `T` (Stopped/Traced): Process actively paused by a user signal (e.g., `Ctrl+Z`) or under debugging observation.

**25. What is a zombie process and how is it created?**
A process that has finished execution but still occupies an entry slot in the system's process table. It is created when a child process exits and its parent process fails to invoke the `wait()` system call to clean its exit code from the process table.

**26. Difference between ps, top, and htop?**

* `ps`: Generates a static snapshot of processes active on the system at the moment of execution.
* `top`: Provides a dynamic, real-time interactive text terminal interface for monitoring running system processes and CPU utilization.
* `htop`: A modernized, colorized, user-friendly interactive terminal monitoring tool that supports text-based scrolling and visual process-tree layouts.

**27. Difference between kill -9 and kill -15?**

* `kill -15` (SIGTERM): Sends a standard termination request. It allows a process to catch the signal, finish current tasks, release locks, and close open files safely before exiting.
* `kill -9` (SIGKILL): Forces an immediate shutdown at the kernel layer. The process cannot catch or ignore this signal, which can leave open files corrupted.

**28. Difference between systemd and init?**

* `init` (SysVinit): A legacy init design that starts system initialization scripts sequentially. This slows down boots and relies on rigid runlevel groupings.
* `systemd`: A modern init architecture that boots components in parallel using socket activation, manages dependencies through declarative targets, and isolates processes using cgroups.

**29. How do you start/stop/enable a service with systemctl?**

* Start a service immediately: `systemctl start <service>`
* Stop a running service immediately: `systemctl stop <service>`
* Configure a service to launch automatically during system boot phases: `systemctl enable <service>`

**30. What is journalctl for and which log sources does it unify?**

* **Purpose:** Queries structured binary log logs managed by the `systemd-journald` engine.
* **Unified Sources:** Combines kernel log messages (`dmesg`), early boot messages, standard output (`stdout`) and error (`stderr`) streams from systemd service units, and system audit logging facilities.

**31. Difference between iptables and nftables?**

* `iptables`: A legacy packet filtering framework where distinct tables (filter, nat, mangle) evaluate rules sequentially, which can degrade performance in larger rulesets.
* `nftables`: The modern successor framework. It runs on a unified abstraction VM engine that uses high-performance lookup tables (maps) and features a cleaner, more efficient configuration syntax.

**32. Difference between ifconfig and ip a? Why is the ip command recommended?**

* `ifconfig`: A deprecated legacy utility from the `net-tools` package that relies on outdated system calls.
* `ip a`: Part of the modern `iproute2` network management suite. It interacts directly with the kernel using netlink routing sockets, providing faster execution and deeper control over modern network configurations.

**33. Difference between /etc/resolv.conf and /etc/hosts?**

* `/etc/resolv.conf`: Configures the IP addresses of external DNS nameservers the system queries to resolve domain names.
* `/etc/hosts`: A local, static plain-text mapping table connecting hostnames directly to specific IP addresses. It takes priority over external DNS lookups.

**34. Difference between cron and at?**

* `cron`: A daemon designed to automate recurring schedule workflows based on consistent time intervals.
* `at`: A utility configured to run a single, one-time automation task at a specific time in the future.

**35. Explain the field order of /etc/crontab.**
The fields follow a strict left-to-right chronological arrangement sequence:
`Minute (0-59)` $\rightarrow$ `Hour (0-23)` $\rightarrow$ `Day of Month (1-31)` $\rightarrow$ `Month (1-12)` $\rightarrow$ `Day of Week (0-6)` $\rightarrow$ `[User Account context]` $\rightarrow$ `[Target Script/Command]`

**36. What does exit code 0 in a bash script mean?**
It indicates that the script completed all operations successfully without encountering unhandled runtime errors or failures.

**37. Package managers: differences between apt, yum, dnf, pacman?**

* `apt`: Manages `.deb` binary software packages on Debian and Ubuntu distributions.
* `yum`: A legacy dependency-resolving tool for managing `.rpm` packages on older Red Hat enterprise builds.
* `dnf`: The modern, optimized successor to `yum` used on current RHEL, Fedora, and Rocky Linux systems.
* `pacman`: The lightweight, fast package manager used on Arch Linux to track rolling-release software dependencies.

**38. What is the PATH variable for and why is it security-relevant?**

* **Purpose:** An environment variable containing a colon-separated list of system directories. When a user runs a command without its absolute path, the shell searches these directories in order for the executable binary.
* **Security Relevance:** If an attacker appends a world-writable directory or the current working directory (`.`) to the beginning of the `PATH`, they can intercept commands. When an admin types a common command, the shell runs the attacker's malicious binary instead (Path Hijacking).

**39. What is umask for?**
The user file-creation mask. It defines default access constraints by subtracting its bitmask from standard file systems default permission values (`666` for files, `777` for directories) whenever a new asset is created.

**40. When are Linux ACLs (getfacl/setfacl) better than standard permissions?**
Standard permissions restrict access settings to exactly one owner user, one owner group, and everyone else. POSIX Access Control Lists (ACLs) allow you to assign granular, specific permission combinations to multiple distinct users and groups on a single file or folder.

---

### B. Scenario-Based Questions (41–80)

**41. A user can read a file but not edit it. In what order would you check?**

1. Check standard permission bits via `ls -l` to see if the user has write access.
2. Verify file ownership to see if the user is the owner or part of the assigned group.
3. Check POSIX extended ACL attributes using `getfacl`.
4. Run `lsattr` to verify if the file has the immutable (`+i`) or append-only (`+a`) attribute set.
5. Check if the underlying filesystem is mounted read-only using `mount`.
6. Run `sudo -l` to verify if the user's write access is restricted by sudo configurations.

**42. The disk is full but `df -h` still shows free space. What could be the cause?**

* **Inode Exhaustion:** The file system has run out of index nodes (inodes) because it contains millions of zero-byte or tiny files. Check this using `df -i`.
* **Deleted-but-Open Files:** A process is holding open a large log file that was deleted from the directory structure, preventing the filesystem from reclaiming the disk space. Find these processes using `lsof +L1` or `lsof | grep deleted` and restart them.

**43. A cron job fails but works manually. Debug sequence?**

1. Check environmental variations. Cron jobs execute under a minimal environment and do not source `/etc/profile` or `~/.bashrc`.
2. Convert all relative commands inside the script to absolute paths (e.g., use `/usr/bin/curl` instead of `curl`).
3. Explicitly declare the `PATH` environment variable at the beginning of the script.
4. Verify the working directory by explicitly adding a `cd /target/dir/` step within the automation logic.
5. Capture runtime errors by redirecting stdout and stderr into a local log file: `* * * * * /path/to/script.sh > /tmp/cron_debug.log 2>&1`.

**44. You must allow an employee to run a single script as root without full sudo su. How do you set it up?**
Open the configuration using `visudo` and add an explicit, single rule for the user account:
`username ALL=(root) NOPASSWD: /usr/local/bin/target_script.sh`
Ensure that `/usr/local/bin/target_script.sh` is owned strictly by `root:root` with permissions set to `755` or `700`. This prevents the employee or anyone else from modifying the script to execute unauthorized commands as root.

**45. You see unauthorized SSH login attempts. In what order do you harden?**

1. Disable password-based logins in `/etc/ssh/sshd_config` and enforce SSH key-based authentication (`PasswordAuthentication no`).
2. Block direct root access over SSH (`PermitRootLogin no`).
3. Limit access using an explicit user whitelist line (`AllowUsers user1 user2`).
4. Install an intrusion prevention tool like `Fail2Ban` to automatically block brute-forcing IP addresses.
5. Move the SSH service away from standard port 22 to a non-standard alternative to cut down on automated scanner traffic.
6. Mandate multi-factor authentication (MFA) using a pluggable authentication module (PAM) like Google Authenticator.

**46. You need a bash script to extract suspicious IPs from a log file. What toolchain would you build?**

```bash
#!/bin/bash
# Extract IPv4 strings, count frequencies, sort by top offenders, filter past a threshold
THRESHOLD=100
grep -E -o "([0-9]{1,3}\.){3}[0-9]{1,3}" /var/log/auth.log | \
sort | \
uniq -c | \
sort -rn | \
awk -v limit="$THRESHOLD" '$1 > limit {print "Suspicious IP: " $2 " [Occurrences: " $1 "]"}'

```

**47. /var/log is full and logs are not rotating. Cause and solution?**

* **Causes:** A syntax error inside `/etc/logrotate.conf` or files under `/etc/logrotate.d/`, a stuck logrotate cron job, or the `logrotate` state database (`/var/lib/logrotate/status`) has become corrupted.
* **Solution:** Run a manual syntax check and force execution using `logrotate -vf /etc/logrotate.conf`. Check for process limits in `/etc/systemd/journald.conf` and apply caps to `SystemMaxUse`.

**48. chmod -R 777 / was run by mistake. How do you remediate?**
Running this breaks critical system security boundaries, SUID flags, and authentication mechanisms, making the OS highly unstable and insecure.

* **Primary Remediation:** Restore the system from a known good bare-metal backup or redeploy the server from scratch.
* **Emergency Stabilization:** If you must recover a running system, boot using live rescue media, mount the target drive, and use package manager verification tools to restore base permissions (e.g., run `rpm --setperms -a` on Red Hat systems, or use standard permission blueprints to fix critical files like `/etc/passwd`, `/etc/shadow`, and `/usr/bin/sudo`).

**49. SSH key-based login fails, password works. Debug?**

1. Check file permissions on the target server. The user's home directory must not be world-writable; `~/.ssh/` must be set to `700`, and `~/.ssh/authorized_keys` must be set to `600`.
2. Inspect `/etc/ssh/sshd_config` to ensure `PubkeyAuthentication yes` is enabled.
3. Review authentication errors in real time by checking `/var/log/auth.log` or running `journalctl -u ssh`.
4. Verify if SELinux is blocking the connection by running `setenforce 0` to test if it works in permissive mode. If it does, fix the security context using `restorecon -R -v ~/.ssh`.

**50. A user accidentally requests resetting the root password. How do you proceed?**

1. Reboot the system and press the appropriate keys to intercept the GRUB bootloader menu.
2. Select the kernel boot line, press `e` to edit, append `rd.break` or `init=/bin/bash` to the end of the kernel parameter string, and press `Ctrl+X` to boot.
3. Remount the root filesystem with write permissions: `mount -o remount,rw /sysroot` (or `/`).
4. Switch to the root filesystem environment: `chroot /sysroot`.
5. Reset the password using the command line: `passwd root`.
6. Force an SELinux filesystem relabel if needed: `touch /.autorelabel`. Save changes, exit, and reboot.

**51. A process won't die (kill -9 fails). Cause and approach?**

* **Cause:** The process is stuck in an Uninterruptible Sleep state (`D state`), waiting for hardware I/O operations to complete, or it has become a zombie process waiting for its parent.
* **Approach:** Check for hardware errors using `dmesg`, investigate stuck network storage shares (like dead NFS mounts) using `df`, and try to restart the parent process or clear the underlying block device. If the process is completely frozen in the kernel, a system reboot is required.

**52. Auto-updates are on but a patch isn't applied. Your investigation path?**

1. Inspect the update logs located at `/var/log/unattended-upgrades/unattended-upgrades.log` (or the equivalent logs for your distribution).
2. Check if a package management lock is blocking updates by verifying that no other installer tools are running.
3. Verify if the patch requires a system restart to complete by checking for the presence of `/var/run/reboot-required`.
4. Check if the target package has been locked at a specific version or excluded from updates by running `apt-mark showhold` or inspecting yum/dnf configurations.

**53. A Python service eats 99% CPU. Real-time diagnostic sequence?**

1. Isolate the exact process ID and active thread allocations using `top -H -p <PID>` or `htop`.
2. Use `strace -p <PID> -c` to generate a summary of system calls and trace performance issues.
3. Use a Python profiling tool like `py-spy dump --pid <PID>` to capture a real-time stack trace of the running Python code without pausing the service. This helps locate infinite busy loops or locks within the application.

**54. A new build needs a service to run without a login user. How do you set it up?**

1. Create a restricted system account without interactive shell capabilities or a home directory:
`useradd -r -s /usr/sbin/nologin serviceuser`
2. Configure the systemd service unit file (e.g., `/etc/systemd/system/myservice.service`) to run under this specific account context:
```ini
[Service]
User=serviceuser
Group=serviceuser
ProtectSystem=strict
CapabilityBoundingSet=~CAP_SYS_ADMIN

```


3. Reload systemd configurations using `systemctl daemon-reload` and start the service.

**55. An admin asks "why isn't setuid bit always bad practice?". Answer with examples.**
"The SUID bit is necessary for normal operation when unprivileged users must safely run commands that modify restricted system files. For example, `passwd` requires SUID root to update salted password hashes inside `/etc/shadow`, and `sudo` requires it to validate privileges. SUID is safe when applied to audited, well-tested binaries. However, for custom or unvetted scripts, you should avoid SUID and instead assign specific, granular permissions using Linux capabilities (`setcap`)."

**56. A Linux box has a suspicious process listening on TCP 4444. Write the triage path.**

1. Identify the process ID and executable binary name handling the port: `ss -ltnp | grep 4444` or `lsof -i :4444`.
2. Inspect the absolute path of the binary link: `ls -l /proc/<PID>/exe`.
3. Check the exact command-line arguments used to start the process: `cat /proc/<PID>/cmdline`.
4. Calculate the cryptographic hash of the binary and run it against intelligence feeds like VirusTotal: `sha256sum /proc/<PID>/exe`.
5. Check for persistence mechanism configurations by auditing active cron jobs, systemd service units, and user startup scripts (`~/.bashrc`).

**57. A bash script behaves oddly due to weak quoting. Cause and fix?**

* **Cause:** Referencing variables without double quotes (e.g., `$var`) allows the shell interpreter to split words on spaces and expand wildcard globs based on Internal Field Separator (`IFS`) settings. This behavior can cause paths with spaces to break or be misinterpreted.
* **Fix:** Enclose variable references in double quotes (use `"$var"` instead of `$var`) to preserve whitespaces and strings. Use automated analysis tools like `shellcheck` to identify and fix quoting issues.

**58. /etc/passwd was accidentally edited and login fails. Proven recovery steps?**

1. Restart the machine into a live rescue media environment.
2. Create a mount point and mount the root disk partition: `mount /dev/sda1 /mnt`.
3. Run syntax validation checks against the file: `pwck -r /mnt/etc/passwd`.
4. If the file is corrupted, restore it using the system's automatically generated backup file: `cp /mnt/etc/passwd- /mnt/etc/passwd`.
5. Unmount the drive partition and safely restart the server.

**59. They say "no sudo logs". How do you configure them?**
Run `visudo` to open the configuration and append explicit logging rules:

```ini
Defaults log_output
Defaults logfile="/var/log/sudo.log"
Defaults syslog=auth

```

Ensure that `rsyslog` or `journald` is configured to monitor the authentication facility, and configure your logging agent to forward `/var/log/sudo.log` directly to a secure, centralized SIEM platform.

**60. You need per-user quota in /home. Which technologies?**

1. Enable quota monitoring flags (`usrquota,grpquota`) inside the `/etc/fstab` configuration file for the `/home` filesystem mount point.
2. Remount the filesystem to apply changes: `mount -o remount /home`.
3. Generate the required quota index data files using `quotacheck -cvug /home`.
4. Activate the quota monitoring system: `quotaon -v /home`.
5. Set specific soft and hard disk consumption limits for individual users using `edquota -u username`.

**61. The company wants FIPS-compliant crypto on Linux. What changes are required?**

1. Enable FIPS mode in the kernel configuration using your package manager or by appending `fips=1` to the boot loader's kernel command-line parameters.
2. Install FIPS-validated cryptographic libraries (such as FIPS-compliant builds of OpenSSL).
3. Restart the server and verify that FIPS compliance is active by checking the system state: `cat /proc/sys/crypto/fips_enabled` (it should return `1`).

**62. A vendor app installs under /root. Why is this bad and what is the alternative?**

* **Why it is Bad:** Storing application files inside `/root` requires the service to run with full root privileges to access its data. If an attacker compromises the vendor application, they instantly inherit superuser control over the entire system.
* **Alternative:** Move the application installation path to `/opt/vendorapp/`. Create a dedicated, unprivileged system user account to run the service, and restrict access permissions on that folder so only the service account can read and write to it.

**63. A bash one-liner is about to delete 1000 files by mistake. How do you safeguard?**

1. Run a non-destructive dry run first by replacing the `rm` command with `echo` or `ls` to preview the list of targeted files.
2. Use protective confirmation flags like `rm -i` to prompt before each file deletion.
3. Use safety-focused CLI utilities like `trash-cli` to move deleted files to a temporary trash folder instead of purging them permanently.
4. Take a temporary filesystem snapshot before running large modification commands.

**64. A new admin asks "why are sudo logs important?". Answer with an IR scenario.**
"Sudo logs are critical because they provide an audit trail during an investigation. For example, if an attacker accesses a standard employee account via a web vulnerability, they will try to run privilege escalation commands to gain root access. Sudo logs record the exact commands, execution timestamps, and user accounts involved in the escalation. This detailed history allows the incident response team to trace the attacker's lateral movement, identify compromised files, and pinpoint the root cause of the breach."

**65. An app needs a privileged (<1024) port but you don't want to run as root. Safer alternative?**
Assign the specific network socket binding capability to the application binary using Linux capabilities:
`setcap 'cap_net_bind_service=+ep' /usr/local/bin/myapp`
This grants the application permission to bind to low ports without giving it full root privileges. Alternatively, use a reverse proxy (like Nginx) to handle external traffic on port 443 and route it to an unprivileged high port (like 8443) where the application is listening.

**66. You suspect a process is leaking file descriptors. How do you check?**

1. Count the number of active open file descriptors associated with the process ID: `ls -l /proc/<PID>/fd/ | wc -l`.
2. Compare that count against the maximum limits assigned to the process by running `ulimit -n` or inspecting `/proc/<PID>/limits`.
3. Monitor trends over time using `lsof -p <PID>` to see if the process continuously opens files without closing them.

**67. Network interface is down but `ip link` shows up. Next checks?**

1. Check the physical layer link status to see if a carrier signal is present: `ip link show` (look for `NO-CARRIER`).
2. Verify hardware link connections and auto-negotiation settings using `ethtool <interface_name>`.
3. Check the kernel log ring buffer for network driver or interface errors using `dmesg | grep <interface_name>`.
4. Verify that the network cable is securely plugged into both the server and the upstream switch port.

**68. Server clock is off by 5 seconds. Why does it matter and how to fix?**

* **Why it Matters:** Even a small time drift can break secure authentication protocols like Kerberos, which typically rejects requests if clocks differ by more than 5 minutes. Time drift also complicates forensic investigations by making it difficult to accurately correlate logs across different servers.
* **Fix:** Install and configure `chrony` or `ntpd`. Point the configuration file to trusted, accurate Network Time Protocol (NTP) pool servers, enable the service, and verify synchronization using `chronyc tracking` or `ntpq -p`.

**69. A new admin: "why isn't chmod 755 always enough?". Your answer?**
"Setting permissions to `755` allows any unprivileged local user or service account on the system to read your scripts and configuration files. If these files contain proprietary code or internal system information, it creates an unnecessary security risk. Following the principle of least privilege, sensitive files should use more restrictive permissions like `700` (owner-only access) or `750` (restricted to a specific group), rather than allowing universal read access."

**70. The company wants an SSH bastion. What design elements do you recommend?**

* Enforce strict public key authentication and completely disable password logins.
* Mandate multi-factor authentication (MFA) for all incoming connections.
* Restrict the bastion to traffic routing operations only by disabling local port forwarding and shell access for unauthorized accounts.
* Implement automated keystroke and session recording using tools like `tlog` or `sudosh`.
* Configure automated IP rate-limiting using `Fail2Ban` and restrict incoming access to whitelisted source IP ranges.

**71. A script's output must show on console and be saved to a file. How?**
Pipe the command's standard output directly into the `tee` utility:
`/path/to/script.sh | tee /var/log/script_output.log`
To append data to an existing log file instead of overwriting it, include the append flag:
`/path/to/script.sh | tee -a /var/log/script_output.log`

**72. %wheel was added to /etc/sudoers. Explain the risk.**
If the wheel group configuration uses broad administrative privileges (e.g., `%wheel ALL=(ALL:ALL) ALL`), any user added to the `%wheel` group gains full root capabilities on the system. If an attacker compromises even one account belonging to that group, they instantly gain a path to take complete control of the server.

**73. You pentest a Linux server for the first time. What do you do in the first 30 minutes?**

1. Check your current user privileges and available sudo rules by running `id` and `sudo -l`.
2. Search the entire filesystem for binaries with the SUID or SGID bit set: `find / -perm -4000 -type f 2>/dev/null`.
3. Scan for active processes listening on network ports using `ss -ltnp` or `netstat -antp`.
4. Inspect world-writable directories and search for plain-text credentials left in configuration files, history logs, or scripts.

**74. You're told "SELinux is blocking weird stuff, disable it". Your answer?**
"Disabling SELinux lowers our system security by removing Mandatory Access Control (MAC) protections. Instead of disabling it, we should switch SELinux to `permissive` mode temporarily to log the behavior without blocking it. We can then find the specific denials using `ausearch -m AVC -ts recent`, pass those logs to `audit2allow -M my_policy` to generate a custom security policy module, apply the policy using `semodule -i my_policy.pp`, and safely switch SELinux back to `enforcing` mode."

**75. You need the same patch on many Linux servers. Your approach?**
Use an automation and configuration management tool like `Ansible`. Write a declarative playbook to handle the package update, test it first on a small group of canary development servers to verify stability, and then schedule a rolling deployment across production servers during an approved maintenance window.

**76. How should user input be safely handled in a bash script?**

* Never pass unsanitized user input strings directly into dangerous execution commands like `eval`.
* Enclose all variables that handle user input inside double quotes (`"$USER_INPUT"`) to prevent word splitting and command injection.
* Validate input patterns against strict alphanumeric regular expression whitelists.
* Parse incoming script parameters using standard, structured argument parsers like `getopts`.

**77. Explain consequences of "rm -rf /" and how to prevent it on a test server?**

* **Consequences:** This command forces the recursive deletion of all accessible files, system configurations, binaries, and mounted storage drives, causing an immediate system crash and unrecoverable data loss.
* **Prevention:** Modern versions of `rm` include built-in protections that block this command unless `--no-preserve-root` is explicitly added. To add extra layers of safety on critical systems, you can create a protective shell alias (`alias rm='rm -I'`), set immutable flags (`chattr +i`) on critical directories, or run services inside isolated container environments.

**78. How would you use auditd to set up an audit trail on Linux?**

1. Define explicit security monitoring rules inside `/etc/audit/rules.d/audit.rules`.
2. Add file watches on critical files to monitor changes (e.g., `-w /etc/passwd -p wa -k identity_modification`).
3. Set up rules to log specific execution system calls (e.g., `-a always,exit -F arch=b64 -S execve -k command_execution`).
4. Review the captured logs using auditing tools like `ausearch` and `aureport`, and configure the system to forward audit logs to a secure, centralized SIEM.

**79. A server shows high load average but low CPU. Why?**
The system is bottlenecked by intensive disk I/O operations or processes waiting on storage access. The CPU sits idle because the running processes are stuck in an Uninterruptible Sleep state (`D state`), waiting for responses from slow disk drives, busy flash arrays, or unresponsive network storage shares (like a stalled NFS server).

**80. An employee added a suspicious function in .bashrc. What does this mean for persistence?**
This means an attacker has successfully established user-level persistence. Every time that specific user opens a new interactive terminal shell session, the malicious function script automatically runs under their account context. You can find these unauthorized changes by comparing the user's `.bashrc` against clean default configuration templates located inside `/etc/skel/`.

---

### C. Checklist-Style Questions (81–100)

* **[x] /etc/passwd vs /etc/shadow?** `/etc/passwd` holds public account metadata; `/etc/shadow` restricts access and stores secure, salted password hashes.
* **[x] What does chmod 755 mean?** Owner gets full access (`rwx`); group and others get read and execute access (`r-x`).
* **[x] setuid, setgid, sticky bit?** SUID runs a binary as the file owner; SGID runs it as the file group; the Sticky Bit prevents users from deleting files owned by others within a shared directory.
* **[x] What is umask?** A user file creation mask that subtracts permission bits to automatically restrict default access levels on newly created files and folders.
* **[x] Hard link vs soft link?** A hard link points directly to an existing file's inode; a soft link is a shortcut file containing a path string to another filename.
* **[x] sudo vs su?** `sudo` executes a command using the calling user's own password and logs the action; `su` switches to another user account context and requires that target account's password.
* **[x] grep vs grep -E?** `grep` uses Basic Regular Expressions (BRE); `grep -E` enables Extended Regular Expressions (ERE) natively without extra escape characters.
* **[x] sed vs awk?** `sed` is a stream editor designed for simple line substitutions; `awk` is a text-processing language built for parsing structured data columns.
* **[x] find vs locate?** `find` searches the filesystem in real-time; `locate` quickly queries a pre-compiled index database path map.
* **[x] stdin/stdout/stderr FD numbers?** `stdin` is File Descriptor 0; `stdout` is File Descriptor 1; `stderr` is File Descriptor 2.
* **[x] kill -9 vs kill -15?** `kill -15` (SIGTERM) requests a graceful process shutdown; `kill -9` (SIGKILL) forces an immediate kernel-level termination.
* **[x] What is systemd?** The modern init system and service manager used by Linux distributions to initialize system components in parallel.
* **[x] What is journalctl for?** A command utility designed to query binary log logs captured by the `systemd-journald` engine.
* **[x] iptables vs nftables?** `iptables` is a legacy firewall that processes rules sequentially; `nftables` is its modern replacement that uses a faster, virtual-machine-based rules engine.
* **[x] Why is the bash shebang important?** It specifies the absolute path of the interpreter binary that the kernel loader must run to parse and execute the script.
* **[x] Cron syntax?** Minute $\rightarrow$ Hour $\rightarrow$ Day of Month $\rightarrow$ Month $\rightarrow$ Day of Week $\rightarrow$ Command String.
* **[x] Process states?** Processes can be Running (`R`), Interruptible Sleep (`S`), Uninterruptible Sleep (`D`), Zombie (`Z`), or Stopped (`T`).
* **[x] PATH variable security?** Misconfiguring `PATH` by adding loose permissions or unvetted directories can allow an attacker to place malicious binaries that hijack standard commands (Path Hijacking).
* **[x] Why is /etc/sudoers critical?** It defines which users have administrative privileges and can run commands as root.
* **[x] Why are ACLs sometimes better than perms?** Access Control Lists let you assign specific permissions to multiple distinct users and groups on a single file, bypassing the simple owner-group-others limits of standard permissions.
---

## 4. MICROSOFT TECHNOLOGIES
*Source modules: Virtualization, Windows Intro, Servers & Domains, Active Directory, GPO, Files & Folders Permissions, Windows Authentication, Remote Admin Tools, Windows Naming Systems (DNS), Server Hardening, Intro to PowerShell*

### A. General Questions (1–40)

**1. Difference between Type 1 and Type 2 hypervisors? Give examples.**

* **Type 1 (Bare-Metal):** Runs directly on the physical host hardware without an underlying host operating system. Highly efficient, secure, and used for enterprise datacenters.
* *Examples:* VMware ESXi, Microsoft Hyper-V (core role), Proxmox VE.


* **Type 2 (Hosted):** Runs as an application layer on top of an existing host operating system. Slower due to OS overhead; used primarily for testing and development.
* *Examples:* VMware Workstation, Oracle VirtualBox.



**2. What is Hyper-V and which editions include it?**

* **Definition:** Microsoft's native Type 1 virtualization hypervisor that creates and manages isolated virtual machines (VMs).
* **Editions:** Included in 64-bit editions of Windows 10/11 Pro, Enterprise, and Education, as well as Windows Server (Standard and Datacenter editions). It is *not* available on Windows Home editions.

**3. What is the difference between Workgroup and Domain in Windows?**

* **Workgroup:** A decentralized, peer-to-peer network model where every machine maintains its own local Security Accounts Manager (SAM) database. Authentication is localized to each machine.
* **Domain:** A centralized client-server network architecture managed by Active Directory Domain Services (AD DS). Users, permissions, and security policies are validated globally by centralized Domain Controllers.

**4. Explain the difference between Forest, Tree, Domain, and OU in Active Directory.**

* **Forest:** The top-level security and administrative boundary in Active Directory. It contains one or more Trees that share a common schema, configuration partition, and global catalog.
* **Tree:** A hierarchical grouping of one or more domains that share a contiguous, fluid DNS namespace.
* **Domain:** A logical administrative boundary containing objects (users, computers, groups) that share an identical directory database and security policy baseline.
* **Organizational Unit (OU):** Sub-containers inside a domain used to organize objects for granular Group Policy application and administrative delegation.

**5. What is a Domain Controller (DC) and what roles does it perform?**

* **Definition:** A physical or virtual server running Active Directory Domain Services (AD DS) that acts as the absolute authority for identity and security within a domain.
* **Roles:** Handles identity authentication (via Kerberos/NTLM), processes group policy deployments, hosts directory lookup partitions, maintains DNS records, and replicates updates to peer DCs.

**6. What are the 5 FSMO roles and what is each for?**
Flexible Single Master Operation (FSMO) roles prevent conflicts by ensuring specific tasks are handled by a single DC at a time:

* **Schema Master (Forest-wide):** Controls all modifications and extensions made to the Active Directory database structure definition (the blueprint).
* **Domain Naming Master (Forest-wide):** Coordinates and authorizes the addition, deletion, or renaming of domains within the entire forest namespace.
* **PDC Emulator (Domain-wide):** Acts as the authoritative master clock for time synchronization, processes password updates instantly to prevent replication lag lockouts, and maintains legacy support.
* **RID Master (Domain-wide):** Allocates blocks of Relative Identifiers (RIDs) to DCs, ensuring every newly created security object receives a globally unique Security Identifier (SID).
* **Infrastructure Master (Domain-wide):** Updates cross-domain object references, mapping localized user IDs accurately when objects point across distinct domains.

**7. What is the Global Catalog and where is it stored?**

* **Definition:** A centralized directory index that stores a full replica of all objects inside its local domain, plus a partial, read-only attribute snapshot of every object across *all* domains within the Active Directory forest.
* **Storage:** Stored on designated Domain Controllers configured as Global Catalog servers (GC) inside the AD database (`ntds.dit`).

**8. How does AD replication work? Intra-site vs inter-site difference?**
Active Directory uses a multi-master replication scheme where changes made on one DC are synchronized across all other peer DCs.

* **Intra-site Replication:** Occurs between DCs located within the same well-connected network site. It uses a change-notification system to sync updates almost immediately (within seconds) over uncompressed RPC/IP.
* **Inter-site Replication:** Occurs between distinct network sites across slower WAN links. It compresses traffic to save bandwidth and runs on a controlled, customizable schedule (defaulting to every 180 minutes).

**9. What is a Group Policy Object (GPO) and at which levels is it applied?**

* **Definition:** A collection of registry-based configuration preferences and security settings applied to users or computers within an Active Directory environment.
* **Levels of Application:** Group Policies are linked and evaluated at four distinct layers: **L**ocal, **S**ite, **D**omain, and **O**rganizational **U**nit (OU).

**10. What is the GPO processing order (LSDOU)?**

Policies are processed sequentially from broadest to most specific:


$$\text{Local Policy} \longrightarrow \text{Site} \longrightarrow \text{Domain} \longrightarrow \text{Organizational Unit (OU)}$$


*Note: Policies applied later in the sequence overwrite conflicting configurations set by earlier policies. This means settings applied at the OU level take precedence over domain-level settings.*

**11. Difference between Block Inheritance, Enforced, and Loopback processing?**

* **Block Inheritance:** An OU-level setting that blocks all upstream policies inherited from parent domains or OUs.
* **Enforced (No Override):** A GPO-level setting that forces a policy to apply downstream, overriding any conflicting settings or "Block Inheritance" flags configured on lower-level OUs.
* **Loopback Processing:** A policy mode that ensures a computer applies the user settings linked to *its own* OU, regardless of which user logs in or where that user's account resides in AD.

**12. When NTFS and Share permissions apply together, which rule wins?**
When traffic originates from across a network share, the system evaluates both the Share permissions and the local NTFS permissions, and **the most restrictive permission rule wins.** For example, if Share is *Read* and NTFS is *Full Control*, the effective permission is *Read*.

**13. How does NTFS permission inheritance work?**
By default, any file or subfolder automatically inherits all file access settings configured on its parent directory. Administrators can explicitly break inheritance at a specific subfolder level to stop parent permissions from applying and choose to either copy the existing rules as static permissions or clear them entirely.

**14. What is Effective Permissions used for?**
An administrative diagnostic tool within the Windows Advanced Security file properties. It calculates and displays the exact cumulative access rights a specific user or group has over an asset after factoring in group memberships, permission inheritance, nested groups, and explicit *Deny* rules.

**15. Why is Take Ownership a security-critical privilege?**
The `SeTakeOwnershipPrivilege` right allows a user or group to claim full legal ownership of any object or file across the local system, regardless of its current access rules. Once a user takes ownership, they can modify its permissions to grant themselves full control, bypassing all existing security restrictions.

**16. Difference between NTLM and Kerberos in Windows Authentication?**

* **NTLM (NT LAN Manager):** A legacy authentication protocol that uses a challenge-response mechanism. It relies on caching password hashes on endpoints or passing them over the network to a Domain Controller for validation.
* **Kerberos:** The modern, standard authentication protocol for Active Directory. It is a ticket-based system that uses a trusted third party (the Key Distribution Center) to authenticate identities without ever transmitting password hashes across the wire.

**17. What are the main Kerberos components (KDC, AS, TGS, TGT, ST)?**

* **KDC (Key Distribution Center):** The centralized authentication service running on a Domain Controller, consisting of the AS and the TGS.
* **AS (Authentication Service):** Authenticates the initial client login request and issues the Ticket Granting Ticket (TGT).
* **TGS (Ticket Granting Service):** Validates a user's TGT and issues short-lived Service Tickets (ST) for specific resources.
* **TGT (Ticket Granting Ticket):** A temporary master credential issued to a user after a successful login, allowing them to request access to specific services without re-entering their password.
* **ST (Service Ticket / Session Ticket):** A single-use credential presented directly to a target network resource (like a file share) to gain access.

**18. Why is Kerberos more secure than NTLM?**

* Kerberos uses mutual authentication, meaning both the client and the server verify each other's identity before establishing a connection.
* It relies on timestamped, encrypted tickets, which protects against relay and replay attacks.
* It does not send password hashes over the network during authentication, unlike NTLM.
* It supports modern, strong cryptographic algorithms like AES-256, whereas NTLM relies on weaker legacy hashing schemes.

**19. Which authentication protocol does Pass-the-Hash target?**
Pass-the-Hash targets the legacy **NTLM** authentication protocol. Because NTLM validates authentication using password hashes rather than cleartext passwords, an attacker who steals an NTLM hash from system memory can use it directly to authenticate to remote servers without needing to crack the actual password.

**20. What do Pass-the-Ticket and Golden Ticket attacks mean?**

* **Pass-the-Ticket (PtT):** A post-exploitation technique where an attacker extracts valid Kerberos tickets (TGTs or Service Tickets) from the memory of a compromised system and injects them into their own session to access other network resources.
* **Golden Ticket:** A catastrophic privilege escalation exploit where an attacker uses the stolen password hash of the Active Directory Key Distribution Center service account (`krbtgt`) to forge their own Ticket Granting Tickets (TGTs). This grants them unrestricted administrative control over any resource in the Active Directory forest indefinitely.

**21. Difference between Silver Ticket and Golden Ticket?**

* **Golden Ticket:** Forged by compromising the `krbtgt` account hash. It yields a master Ticket Granting Ticket (TGT) that grants full access to *any* service across the entire Active Directory domain or forest, typically lasting up to 10 years.
* **Silver Ticket:** Forged by compromising the password hash of a specific computer or service account. It yields a targeted Service Ticket (ST) that only grants access to services running on that particular machine (e.g., HTTP or CIFS), without interacting with the Domain Controller.

**22. How does a Kerberoasting attack work?**

1. An attacker with standard user access queries Active Directory for service accounts that have a registered Service Principal Name (SPN).
2. The attacker requests a valid Kerberos Service Ticket (ST) for these accounts from the Domain Controller.
3. The Domain Controller issues the ticket, which is encrypted using the password hash of the target service account.
4. The attacker extracts the ticket from their local system memory and takes it offline, using brute-force tools (like John the Ripper or Hashcat) to attempt to crack the service account's plain-text password.

**23. What is AS-REP Roasting?**
A targeted attack against Active Directory user accounts that have the option *"Do not require Kerberos preauthentication"* explicitly enabled. An attacker can send an authentication request (AS-REQ) for these accounts to the Domain Controller, and the DC will immediately respond with an AS-REP ticket encrypted with that user's password hash. The attacker can then capture this response and attempt to crack the account password offline.

**24. Difference between Domain Admin and Enterprise Admin?**

* **Domain Admin (DA):** Grants full administrative control over a single specific domain. Members can manage all objects, systems, and configurations within that domain boundary.
* **Enterprise Admin (EA):** A highly privileged group that exists only in the root domain of an Active Directory forest. Members have complete administrative authority over *all* domains, configuration partitions, and schemas across the entire forest.

**25. What is the main purpose of the Tier 0/1/2 admin model?**

The Microsoft tiering model enforces strict isolation of administrative credentials to prevent privilege escalation and lateral movement by attackers:

* **Tier 0:** The highest security zone, containing identities that control the entire forest (Domain Controllers, PKI, Enterprise Admins). Tier 0 credentials must never be used on lower-tier systems.
* **Tier 1:** Controls enterprise server assets, cloud infrastructure, and core business applications.
* **Tier 2:** Controls end-user devices, workstations, and local user support systems.

**26. Difference between RDP, WinRM, and SSH in Windows context?**

* **RDP (Remote Desktop Protocol):** Microsoft's graphical remote management interface, allowing users to log into and interact with a full desktop environment over the network.
* **WinRM (Windows Remote Management):** A firewall-friendly, SOAP-based remote management protocol designed for command-line access. It serves as the underlying transport layer for PowerShell Remoting.
* **SSH (Secure Shell):** An open-standard, cross-platform command-line execution protocol now natively integrated into modern Windows versions to allow secure remote terminal management.

**27. Which ports does PowerShell Remoting (WinRM) use?**

* **HTTP (Unencrypted):** Port **5985**
* **HTTPS (Encrypted):** Port **5986**

**28. Why is DNS critical for AD in Windows?**
Active Directory requires DNS to function. It relies on DNS to locate domain resources and infrastructure. Without a functioning DNS environment, domain-joined clients cannot find Domain Controllers to log in, process Group Policies, or replicate changes across the network.

**29. Difference between AD-Integrated DNS zone and Primary DNS zone?**

* **Standard Primary Zone:** Stores its DNS records in a single, local read-write text file (`.dns`) on a specific primary server. Updates must be made on this server, creating a single point of failure.
* **AD-Integrated Zone:** Stores its DNS records directly inside the Active Directory database partition (`ntds.dit`). This allows multi-master replication, meaning *any* Domain Controller can process updates, and traffic is automatically encrypted and replicated alongside standard AD data.

**30. What is the role of _msdcs._domain.local SRV records?**
These specialized DNS Service Location (SRV) locator records map domain services to specific hostnames. They act as a directory directory for the network, allowing client machines to find available Domain Controllers, Global Catalog servers, and Key Distribution Centers within the Active Directory infrastructure.

**31. What are the main phases of Server Hardening?**

* **Phase 1: Planning & Assessment:** Establish an approved security baseline benchmark (e.g., CIS Benchmarks or Microsoft Security Baselines).
* **Phase 2: Network & Access Hardening:** Disable unnecessary network ports, restrict administrative access using the tiering model, and block direct internet access.
* **Phase 3: Attack Surface Reduction:** Uninstall unused server roles, features, and legacy software components; disable weak cryptographic protocols (like TLS 1.0/1.1 and NTLMv1).
* **Phase 4: Identity Hardening:** Implement unique local administrator passwords using LAPS and enforce multi-factor authentication (MFA).
* **Phase 5: Defensive Controls & Auditing:** Install security monitoring agents (AV, EDR, SIEM forwarders) and enable detailed event logging.

**32. What is LAPS and what is it for?**

* **Definition:** Local Administrator Password Solution (LAPS).
* **Purpose:** Automates the management of local administrator account passwords on domain-joined computers. It generates a unique, complex password for each local admin account, rotates it on a regular schedule, and stores it securely as an encrypted attribute within the Active Directory database. This prevents lateral movement attacks that exploit identical local administrator passwords across multiple machines.

**33. Difference between Local Administrators and Domain Administrators?**

* **Local Administrators:** Have full administrative control over a single specific workstation or member server where the account is defined. They have no rights or visibility into other network systems or Active Directory objects.
* **Domain Administrators:** Have administrative authority across the entire Active Directory domain, including all member servers, workstations, and the Domain Controllers themselves.

**34. What is the Restricted Groups GPO for?**
An administrative policy control used to centrally enforce and audit membership within sensitive local security groups (such as the local `Administrators` or `Remote Desktop Users` groups) across domain-joined computers. This policy automatically removes unauthorized accounts that users try to add locally.

**35. What are Credential Guard and Device Guard?**

* **Credential Guard:** Uses virtualization-based security (VBS) to isolate sensitive OS credential structures (like the LSASS process memory) within an encrypted, hypervisor-managed container. This prevents administrative tools or malware (such as Mimikatz) from stealing credential hashes from system memory.
* **Device Guard:** A legacy term for a combination of enterprise security features, including hypervisor-protected code integrity (HVCI) and application control, designed to ensure that only trusted, signed drivers and code can run on the operating system kernel.

**36. What is Windows Defender Application Control (WDAC)?**
An enterprise-grade, application-whitelisting security solution built into the Windows kernel. It enforces application execution policies, blocking unauthorized software, scripts, installers, and untrusted digital signatures from running on the system.

**37. What is AppLocker?**
An administrative application control feature used to restrict which applications, scripts, and installers users can run on a machine. It allows administrators to define rules based on file attributes like digital signatures, publisher identities, file paths, or cryptographic hashes.

**38. Why is PowerShell Constrained Language Mode used?**
It is a protective execution mode designed to reduce the attack surface of PowerShell. It restricts access to advanced language features—such as direct execution of unvetted .NET classes, Win32 API interactions, or untrusted memory manipulations—preventing attackers from using complex fileless malware scripts.

**39. Difference between cmdlet, function, and alias in PowerShell?**

* **Cmdlet:** Built-in, pre-compiled .NET program binaries integrated into the PowerShell environment (written in a standard `Verb-Noun` format, like `Get-Process`).
* **Function:** A reusable block of PowerShell code written directly in the PowerShell scripting language.
* **Alias:** A lightweight shortcut name mapped to an existing command or cmdlet (for example, mapping `dir` or `ls` to `Get-ChildItem`).

**40. What execution policy levels does PowerShell have?**
PowerShell execution policies control the conditions under which the shell loads scripts and configuration files:

* `Restricted`: The default policy; blocks all script execution, allowing only individual interactive commands.
* `AllSigned`: Allows only scripts signed by a trusted digital publisher to run.
* `RemoteSigned`: Allows locally written scripts to run without a signature, but requires scripts downloaded from the internet to be signed by a trusted publisher.
* `Unrestricted`: Loads and runs all scripts regardless of origin or signature, though it displays a warning for unsigned files downloaded from the web.
* `Bypass`: Nothing is blocked and no warnings or prompts are displayed; used primarily for administrative automation deployments.

---

### B. Scenario-Based Questions (41–80)

**41. Users report that they can log on with their domain account on some machines but not others. How do you investigate step by step?**

1. Check basic network connectivity from the problematic machine to the Domain Controller using `ping` or `Test-NetConnection`.
2. Verify DNS client settings to ensure the machine is querying the correct Active Directory DNS servers.
3. Check for time synchronization drift between the workstation and the Domain Controller; if the time skew exceeds 5 minutes, Kerberos authentication will fail.
4. Test the machine's secure channel connection to AD by running `Test-ComputerSecureChannel -VerifySecureChannel`. If it fails, reset the computer account password.
5. Review active Group Policy Objects using `gpresult /h` to see if restrictive settings, such as *"Deny log on locally"* or user rights assignments, are blocking access on those specific machines.
6. Check Active Directory Users and Computers (ADUC) to see if the user's account is locked out or if their login hours are restricted.

**42. If DNS is broken in an AD environment, what problems arise with login, GPO, and other services?**

* Clients will be unable to locate Domain Controllers because they cannot resolve the required `_msdcs` SRV locator records, causing logins to fall back on slow, cached credentials.
* Group Policy updates will fail because machines cannot resolve the target DFS file paths or connect to the `SYSVOL` share.
* Active Directory replication between Domain Controllers will stop completely because peer DCs will be unable to find and communicate with each other.

**43. A user was added to a security group but still can't access a shared folder. Possible causes?**

* **Token Refresh Issue:** The user's Kerberos group membership token is only updated during initial login. The user needs to log out and log back in (or renew their ticket using `klist purge`) for the new group membership to take effect.
* **Permission Overlap:** An explicit *Deny* rule on the shared folder or NTFS permissions is overriding the *Allow* permission granted by the new group membership.
* **Replication Delay:** The group update has not yet replicated from the DC where the change was made to the DC validating the user's current session.

**44. Why are too many Domain Admin accounts a major risk?**
Having excessive Domain Admin accounts increases your attack surface and credential exposure. If an attacker compromises just one of these accounts through phishing or credential theft, they instantly gain full control over the entire domain (a massive blast radius). It also makes it harder to maintain clear accountability during security audits and incident response investigations.

**45. NTLM is still enabled. What risks would worry you?**

* **Pass-the-Hash Attacks:** Attackers can capture an NTLM password hash from a compromised system's memory and use it to authenticate to other servers without needing the plain-text password.
* **NTLM Relay Attacks:** Attackers can intercept NTLM authentication requests on the network and relay them to another server to perform unauthorized actions on behalf of the victim.
* **Lack of Mutual Authentication:** NTLM does not verify the identity of the server, leaving clients vulnerable to rogue server impersonation attacks.

**46. Operational and security differences between editing Default Domain Policy vs creating a new GPO?**

* **Default Domain Policy:** Should be reserved exclusively for broad, high-level core configurations that apply to every account in the domain (such as account lockout thresholds and password complexity rules). Modifying it for everyday settings increases the blast radius if an error occurs and complicates rollback procedures.
* **New GPOs:** Allow administrators to follow a modular design, testing and deploying specific settings to targeted groups or Organizational Units (OUs). This limits risk, simplifies auditing, and allows for clean rollbacks if a policy causes issues.

**47. A server shows many failed logons. How do you tell brute force from misconfig vs a service account issue?**
Analyze Windows Security Event Log **Event ID 4625** (Failed Logon) and look for patterns:

* **Brute-Force Attack:** Characterized by hundreds of failed login attempts targeting multiple different user accounts (or a single high-value account) from an unknown external IP address, occurring rapidly within a short time frame.
* **Misconfigured User Account:** Shows failed login attempts for a single specific user account originating from a workstation they recently changed their password on, often triggered by stale cached credentials in credential manager.
* **Stuck Service Account:** Displays continuous, regularly timed failed login attempts for a specific non-human service account name, originating from a server where an application or scheduled task is running with an outdated password.

**48. First security steps before pushing a new Windows server to production?**

1. Install all outstanding security patches and software updates.
2. Disable or uninstall all unnecessary server roles, features, and default background services.
3. Configure the Windows Defender Firewall to block all incoming traffic by default, whitelisting only the explicit network ports required by the server's role.
4. Enforce secure remote access by restricting RDP access to specific administrative users and enabling Network Level Authentication (NLA).
5. Install and configure enterprise endpoint protection (AV/EDR) and verify the agent is communicating with the central management console.
6. Place the server into its appropriate administrative Organizational Unit (OU) so it automatically inherits company security group policies, and verify that centralized event logging is working.

**49. "Server works, why harden?" — your response?**
"An unhardened server uses default configurations designed for compatibility rather than security. It often runs unnecessary services, leaves weak legacy protocols active, and keeps open network ports that significantly increase its attack surface. If an attacker compromises an unhardened server, they can easily exploit these default settings to steal administrative credentials, move laterally across our network, and compromise higher-value systems like our Domain Controllers."

**50. You can't remove local admin accounts. How do you reduce risk?**

* Deploy **Microsoft LAPS** to ensure that every machine has a unique, complex local administrator password that changes automatically on a regular schedule.
* Implement Group Policies to block local administrator accounts from connecting over the network or logging in via Remote Desktop Protocol (RDP).
* Enable enhanced login auditing and configure alerting rules to monitor any activity or interactive login events generated by local administrator accounts.

**51. GPO replication isn't working. Diagnostic sequence?**

1. Run `dcdiag /test:replications` to check the general replication health of the Domain Controllers.
2. Use `repadmin /replsummary` and `repadmin /showrepl` to pinpoint exactly which Domain Controllers are failing to replicate.
3. Check the replication state of the `SYSVOL` file system share by reviewing the Distributed File System Replication (DFSR) event logs for errors or out-of-sync update sequence numbers (USNs).
4. Run `gpresult /h` on a client machine to see if there are version number mismatches between the Active Directory template and the physical files stored in `SYSVOL`.

**52. The company asks for "a new OU structure". What design principles do you recommend?**

* Separate computer objects from user objects to keep the structure organized and simplify policy application.
* Create a dedicated administrative hierarchy that mirrors your access control model (Tier 0, Tier 1, Tier 2) to ensure strict credential isolation.
* Design the OU layout to match your administrative delegation requirements and Group Policy scope boundaries, rather than simply copying the company's organizational chart.
* Avoid nesting OUs too deeply, as this makes it harder to troubleshoot permission inheritance and figure out which policies are applying.

**53. Kerberos auth fails on a host but NTLM works. Cause?**

* The client and server clocks are out of sync by more than the allowed 5-minute threshold, causing Kerberos validation checks to fail.
* The target host is missing its Service Principal Name (SPN) configuration in Active Directory, or there is a duplicate SPN registered to a different account.
* The client machine cannot communicate with the Kerberos Key Distribution Center (KDC) over UDP/TCP port 88 due to a network routing or firewall issue.

**54. An employee is hit with an RDP password spray. Defense layers?**

* Enforce an account lockout policy that temporarily locks accounts after a specific number of failed login attempts within a set time frame.
* Require multi-factor authentication (MFA) on all remote access connections, ideally routed through a secure Remote Desktop Gateway.
* Enable Network Level Authentication (NLA) on all servers to force authentication to occur before an RDP session window is opened.
* Implement automated network defense tools (such as IP rate-limiting policies or Fail2Ban-style scripts) to block source IP addresses that trigger high volumes of failed login events.

**55. How do you detect Kerberoasting?**

* Monitor Windows Security Event Logs for **Event ID 4769** (A Kerberos service ticket was requested).
* Build detection queries to look for anomalies, such as a single user account requesting service tickets for multiple distinct Service Principal Names (SPNs) in a short period.
* Filter for ticket requests that use weak, legacy encryption types like **RC4 (0x17)**, which are easier to crack offline than modern AES encryption.
* Deploy unprivileged "honey accounts" with tempting SPNs in Active Directory, and configure high-severity alerts to trigger the moment any ticket is requested for them.

**56. You're designing an RDP gateway. How?**
Deploy a dedicated Remote Desktop Gateway (RD Gateway) role within a secure perimeter network (DMZ). Configure the gateway to accept incoming HTTPS traffic on port 443, wrap the RDP sessions inside that encrypted tunnel, and enforce multi-factor authentication (MFA) before allowing connections into the internal corporate network. Use Remote Desktop Authorization Policies (RD CAPs and RD RAPs) to define exactly which user groups are allowed to connect to specific internal server resources.

**57. A suspicious new Administrator account appears in AD. Your first steps?**

1. Identify the exact time the account was created and locate the origin system by searching the Domain Controller event logs for **Event ID 4720** (A user account was created).
2. Find the user account that created the unauthorized administrator and immediately disable or contain both accounts to prevent further activity.
3. Review Active Directory replication logs to confirm that the change has synced across all Domain Controllers and ensure no other unauthorized accounts exist.
4. Check your endpoint protection and SIEM logs for related Indicators of Compromise (IOCs) on the originating machine to see if an administrative session was hijacked.

**58. A GPO was linked to the wrong OU and reached 5000 machines. Rollback path?**

1. Open the Group Policy Management Console (GPMC) and immediately delete the incorrect GPO link from the target OU.
2. If the policy applied harmful settings that need to be reverted quickly, create a temporary corrective GPO that explicitly reverses those configurations and link it to the OU.
3. Use administrative tools to force endpoints to refresh their policies, or let them update automatically during their next background check cycle.
4. Review your change control procedures and set up stricter peer review guidelines to prevent unverified policy links from being deployed in the future.

**59. The company says "disable PowerShell". Your response and alternative?**
"Disabling PowerShell completely is not practical, as modern Windows operating systems and administrative tools rely heavily on it for core management tasks. Instead of trying to disable it, we should secure it by enforcing **PowerShell Constrained Language Mode** via AppLocker to block malicious scripting techniques. We should also enable deep ScriptBlock Logging and Transcription logging to forward all PowerShell activity directly to our SIEM for real-time security monitoring."

**60. A new admin asks "is AD Recycle Bin needed?". Benefits and setup?**

* **Benefits:** Allows administrators to instantly recover accidentally deleted Active Directory objects (users, computers, groups) along with all of their original security attributes, group memberships, and SIDs, without needing to restore a backup or restart Domain Controllers into Directory Services Restore Mode (DSRM).
* **Setup:** Ensure that the Active Directory Forest Functional Level is set to at least Windows Server 2008 R2 or higher. Then, open the Active Directory Administrative Center (ADAC), select the local domain, and click **Enable Recycle Bin** (note that this change is permanent and cannot be disabled).

**61. You need to set up inter-forest trust with a partner. Risks and preparation?**

* **Risks:** Creating a trust relationship extends your attack surface across organizations; if an attacker compromises the partner forest, they could attempt to move laterally into your network.
* **Preparation:** Configure DNS conditional forwarders so both networks can locate each other's domain resources. Establish a highly restricted, one-way forest trust relationship, enable **SID Filtering** to block unauthorized access claims, and use **Selective Authentication** to manually specify exactly which servers and resources the external partner users are allowed to access.

**62. Hyper-V host VMs don't come back after restart. Triage?**

1. Check the Hyper-V Virtual Machine Management service (`vmms`) in the Windows services panel to ensure it is running properly.
2. Review the Hyper-V event logs under Applications and Services Logs to check for resource attachment or storage initialization errors.
3. Verify that the physical storage disks or network paths holding the virtual machine files (`.vhdx` and configuration files) are online and accessible.
4. Check if the virtual machine states are locked due to orphaned hypervisor checkpoints or interrupted replication workflows.

**63. A service account password hasn't been rotated in 5 years. Strategy?**

1. Audit the environment to identify exactly which systems, scheduled tasks, and applications are using this service account.
2. Migrate the application to use a **Group Managed Service Account (gMSA)** if supported, which allows Windows to handle complex password generation and automatic rotations without administrative overhead or downtime.
3. If a gMSA is not supported, schedule a maintenance window, update the password to a strong value inside Active Directory, and update the credential properties across the dependent application servers.
4. Monitor system logs for any authentication errors or failed task executions immediately following the update to ensure the service is running normally.

**64. An admin wants to enable LSA Protection via GPO. Benefit and risk?**

* **Benefit:** It runs the Local Security Authority (LSA) process as a Protected Process Light (PPL), preventing non-protected administrative applications or credential-dumping utilities (like Mimikatz) from reading its memory space.
* **Risk:** It will block third-party authentication packages, legacy single sign-on tools, or unsigned custom drivers that need to interact directly with the LSA process, potentially causing critical applications to fail. Always run a staged rollout in audit mode first to identify compatibility issues.

**65. "Why not expose RDP to the internet?" — your reply?**
"Exposing the standard RDP port directly to the internet leaves it vulnerable to continuous automated brute-force attacks and password spray campaigns. It also exposes the server to severe remote code execution vulnerabilities like BlueKeep. Attackers regularly target exposed RDP ports as an entry point to compromise servers, deploy ransomware, and move laterally through internal corporate networks. Remote access should always be placed securely behind a VPN or a protected Remote Desktop Gateway with multi-factor authentication (MFA) enabled."

**66. Windows Update on a server is failing. Investigation paths?**

* Check network connectivity to your local WSUS server or the public Microsoft Update endpoints.
* Run the System File Checker (`sfc /scannow`) and the Deployment Image Servicing and Management tool (`DISM /Online /Cleanup-Image /RestoreHealth`) to repair corrupted operating system components.
* Stop the Windows Update background services, clear the cached data inside the temporary update folder (`C:\Windows\SoftwareDistribution\`), and restart the update process to clear corrupted downloads.
* Review the update failure codes inside the system update logs or use PowerShell's `Get-WindowsUpdateLog` command to pinpoint the exact root cause.

**67. "After GPO changes, the computer doesn't apply the policy". Cause?**

* The endpoint has not yet fetched the update; run `gpupdate /force` to manually trigger an immediate policy refresh.
* Active Directory replication lag is delaying the synchronization of the new policy template from the DC where it was modified to the DC authenticating the client machine.
* The policy's security filtering configuration or a custom WMI filter is blocking the computer object from reading or applying the GPO.

**68. What is a Privileged Access Workstation (PAW) and why is it important?**

* **Definition:** A highly hardened, dedicated physical computer reserved exclusively for performing sensitive administrative tasks (such as managing Domain Controllers, cloud tenants, and identity systems).
* **Importance:** It isolates high-value administrative credentials from common attack vectors like email, standard web browsing, and unvetted productivity applications, ensuring that everyday user vulnerabilities do not compromise your core infrastructure.

**69. Why is placing a "honey account" in AD useful?**
A honey account is a dummy user account created with no legitimate operational purpose, designed to look like a high-value target (such as an administrative service account with an attractive Service Principal Name). Because real employees have no reason to interact with it, any attempt to query its attributes, log into it, or request a Kerberos service ticket for it acts as an immediate Indicator of Compromise (IOC), triggering high-severity security alerts.

**70. The company wants passwordless authentication. Which technologies?**

* **Windows Hello for Business:** Uses asymmetric key pairs bound directly to physical devices and validated through cryptographic PINs or biometrics.
* **FIDO2 Hardware Keys:** External USB or NFC security tokens that provide secure, possession-based authentication across endpoints and web services.
* **Certificate-Based Authentication (Smart Cards):** Uses a Public Key Infrastructure (PKI) environment to store authentication certificates on cryptographically secure hardware devices.

**71. A GPO must apply only to a specific group. How do you configure?**

1. Select the GPO in the Group Policy Management Console and locate the **Security Filtering** section.
2. Remove the default `Authenticated Users` group from the list.
3. Explicitly add the targeted security group, granting them both **Read** and **Apply Group Policy** permissions.
4. Ensure that `Authenticated Users` or `Domain Computers` retains **Read** permissions (but *not* Apply permissions) on the Delegation tab so computers can still parse the policy architecture.

**72. A domain-joined machine says "trust relationship failed". Cause and fix?**

* **Cause:** The machine's local password hash has fallen out of sync with the corresponding password value stored in the Active Directory domain database, breaking the secure communication channel.
* **Fix:** Log into the machine using a local administrator account, open an elevated PowerShell terminal, and run the repair command:
`Reset-ComputerMachinePassword -Server "DC-Name"`
*(Alternatively, remove the workstation from the domain into a temporary workgroup, and then rejoin it to the domain).*

**73. For someone starting server hardening, which baselines do you recommend?**

* **CIS Benchmarks:** Universally recognized, vendor-neutral security guidelines that offer granular, step-by-step instructions for hardening systems.
* **Microsoft Security Baselines:** Official, pre-configured policy baselines provided by Microsoft that can be imported directly into Group Policies.
* **DISA STIGs:** Highly restrictive security technical implementation guides mandated for United States Department of Defense installations, ideal for high-security environments.

**74. A vendor asks "log in with a DA account". Your reaction?**
"You should reject this request, as granting external vendors Domain Admin access violates the principle of least privilege and creates a major security risk. Instead, ask the vendor to specify the exact system privileges and technical requirements their application needs. Then, create a dedicated, unprivileged service account, grant it only the specific permissions required on the local server, and ensure all vendor activity is fully monitored and audited."

**75. Which Windows event IDs may indicate initial compromise?**

* **Event ID 4624 (Logon Type 10):** Indicates a successful interactive remote desktop (RDP) login session.
* **Event ID 4688:** Logs when a new process is created, which can help spot anomalies like a web server process (`w3wp.exe`) launching a command shell (`cmd.exe` or `powershell.exe`).
* **Event ID 4672:** Logs when administrative privileges are assigned to a newly established user session.
* **Event ID 7045:** Logs the installation of a new background system service, a method frequently used by attackers to establish persistence.
* **Event ID 4720:** Tracks the creation of a new user account within Active Directory.

**76. A new PKI design is needed. Main layers?**

* **Offline Root CA:** The absolute master trust authority of your Public Key Infrastructure. It should be kept completely disconnected from the network and powered on only to issue or renew certificates for Intermediate CAs.
* **Subordinate / Enterprise Intermediate CA:** An online server joined to the domain that manages everyday certificate requests, issues certificates to users and services, and publishes Certificate Revocation Lists (CRLs).
* **Hardware Security Module (HSM):** A dedicated hardware device used to secure and manage the private cryptographic keys of your root and intermediate CAs.

**77. How to defend against AD CS Misconfiguration (ESC1–ESC8)?**

* Audit all certificate templates and ensure that the `ENROLLEE_SUPPLIES_SUBJECT` option is disabled on templates that grant administrative access (prevents ESC1).
* Restrict Extended Key Usage (EKU) properties to ensure certificate templates can only be used for their intended purposes.
* Enforce CA manager approval requirements on all sensitive certificate enrollment templates.
* Regularly scan your Active Directory Certificate Services (AD CS) environment using automated security tools like **Certify** or **PSPkiAudit** to find and fix misconfigurations.

**78. Which 4 levels of PowerShell logging should be enabled?**

* **Module Logging:** Records pipeline execution details and structural data for specified PowerShell modules.
* **Script Block Logging:** Captures the full content of code blocks and commands as they are processed, which helps unwrap obfuscated or fileless scripts in real time.
* **Transcription Logging:** Creates a text-based recording of every input command and output response within a PowerShell session.
* **Operational Logging:** Captures core lifecycle entries, service startups, and provider initialization events inside the dedicated Windows event logs.

**79. A DC suddenly went offline. Your recovery plan?**

1. Check the server's availability and assess the severity of the outage to see if the Domain Controller can be brought back online quickly.
2. If the offline DC held any FSMO roles and cannot be recovered, perform an emergency seizure of those roles onto a healthy, operational Domain Controller using `ntdsutil` or PowerShell.
3. Clean up the Active Directory database by removing metadata references to the failed server to prevent replication errors.
4. Verify the overall health of your remaining Domain Controllers and review your backup strategy to ensure system redundancy.

**80. "AD has 30+ years of stale accounts". Your cleanup plan?**

1. Use PowerShell commands to query Active Directory and find inactive user and computer accounts based on their `LastLogonTimestamp` values (e.g., accounts that haven't logged in for over 90 days).
2. Work with business unit managers to review the list of inactive accounts and verify which ones are no longer needed.
3. Implement a phased cleanup approach: first, move the stale accounts to a restricted staging OU and disable them; after a set retention period (e.g., 30 days) with no reported issues, delete the accounts permanently.
4. Ensure the Active Directory Recycle Bin is enabled so you can quickly restore an account if something breaks during the cleanup process.

---

### C. Checklist-Style Questions (81–100)

* **[x] AD Forest, Tree, Domain, OU?** A Forest is the main security boundary; a Tree is a contiguous DNS namespace; a Domain is an administrative boundary; an OU is a sub-container used for organizing objects and applying Group Policies.
* **[x] 5 FSMO roles?** Schema Master, Domain Naming Master, PDC Emulator, RID Master, and Infrastructure Master.
* **[x] GPO LSDOU order?** **L**ocal $\rightarrow$ **S**ite $\rightarrow$ **D**omain $\rightarrow$ **O**rganizational **U**nit (OU). Settings applied later override earlier ones.
* **[x] Enforced vs Block Inheritance?** "Block Inheritance" stops parent policies from applying down to an OU; "Enforced" overrides blocks and forces parent settings to apply downstream.
* **[x] NTFS + Share permission rule?** When accessing files over a network share, the system evaluates both Share and NTFS permissions, and **the most restrictive rule wins.**
* **[x] NTLM vs Kerberos?** NTLM uses a challenge-response model that can be vulnerable to relay attacks; Kerberos is a modern, secure, ticket-based system that supports mutual authentication.
* **[x] Kerberos TGT vs ST?** A Ticket Granting Ticket (TGT) is a master session credential issued at login; a Service Ticket (ST) is a single-use ticket requested using a TGT to access a specific network resource.
* **[x] Pass-the-Hash target?** This attack explicitly targets the **NTLM** protocol by using stolen password hashes to authenticate without needing the plain-text password.
* **[x] Kerberoasting in brief?** An exploitation technique where an attacker requests Kerberos service tickets for accounts with registered SPNs and extracts them from memory to crack their passwords offline.
* **[x] What is a Golden Ticket?** A severe exploit where an attacker uses a stolen `krbtgt` account hash to forge master Kerberos tickets, granting them unrestricted access to the entire Active Directory forest.
* **[x] Tier 0/1/2 model?** An identity tiering model designed to isolate credentials: Tier 0 controls identity infrastructure, Tier 1 manages enterprise servers, and Tier 2 manages end-user workstations.
* **[x] What is LAPS for?** Automatically generates, updates, and securely stores unique local administrator passwords for domain-joined computers, preventing lateral movement.
* **[x] RDP port?** Port **3389** (TCP/UDP).
* **[x] WinRM ports?** HTTP uses port **5985**; HTTPS uses port **5986**.
* **[x] AD-Integrated DNS benefit?** Stores DNS data within the Active Directory database partition, enabling secure multi-master replication and eliminating single points of failure.
* **[x] Why are _msdcs SRV records important?** They act as a locator service that allows domain-joined client machines to find available Domain Controllers and security services on the network.
* **[x] What is Credential Guard?** Uses virtualization-based security (VBS) to isolate and protect sensitive credential processes like LSASS memory inside a secure container.
* **[x] AppLocker vs WDAC?** AppLocker is an application control solution managed through Group Policies; WDAC is a highly secure application control system built directly into the Windows kernel.
* **[x] PowerShell execution policy levels?** Restricted, AllSigned, RemoteSigned, Unrestricted, and Bypass.
* **[x] What is a PAW?** A Privileged Access Workstation—a highly hardened, dedicated computer used exclusively for performing sensitive administrative tasks to protect credentials.

---

## 5. NETWORKING ESSENTIALS
*Source modules: Intro to Networking, Physical Layer, Number Systems, Data Link, Network Layer, Transport Layer, Session/Presentation, Application Layer*

### A. General Questions (1–40)

**1. How many layers does the OSI model have and what is the main responsibility of each?**
The Open Systems Interconnection (OSI) model has **7 layers**:

* **Layer 7 - Application:** Provides network services directly to end-user applications (e.g., HTTP, FTP, SMTP).
* **Layer 6 - Presentation:** Handles data formatting, encryption, decryption, and compression (e.g., SSL/TLS, JPEG, ASCII).
* **Layer 5 - Session:** Establishes, manages, and terminates communication sessions between endpoints.
* **Layer 4 - Transport:** Manages end-to-end communication, error recovery, flow control, and data segmentation (e.g., TCP, UDP).
* **Layer 3 - Network:** Handles logical addressing, routing, and packet forwarding across different networks (e.g., IP, ICMP, routers).
* **Layer 2 - Data Link:** Manages physical addressing (MAC), error detection, and framing within the same local network (e.g., Ethernet, switches).
* **Layer 1 - Physical:** Transmits raw unstructured bit streams over physical media (e.g., cables, fiber, wireless, hubs).

**2. What is the difference between the OSI and TCP/IP models?**

* **Structure:** The OSI model is a theoretical 7-layer reference model, while the TCP/IP model is a practical 4-layer (or updated 5-layer) implementation model.
* **Layer Mapping:** * In the TCP/IP model, the OSI Application, Presentation, and Session layers are combined into a single **Application layer**.
* The OSI Physical and Data Link layers are combined into the **Network Access / Link layer** in the original 4-layer TCP/IP model.


* **Development:** OSI was designed by committee before protocols were written; TCP/IP was built around pre-existing protocols.

**3. How do encapsulation and de-encapsulation work?**

* **Encapsulation (Top-Down):** As data moves down from the Application layer to the Physical layer, each layer adds its own control information (headers and sometimes trailers) to the payload.

$$\text{Data (L5-L7)} \longrightarrow \text{Segment/Datagram (L4)} \longrightarrow \text{Packet (L3)} \longrightarrow \text{Frame (L2)} \longrightarrow \text{Bits (L1)}$$


* **De-encapsulation (Bottom-Up):** When the receiving host gets the raw bits, the data moves up the stack. Each layer strips off its respective header, processes the control information, and passes the remaining payload up to the next layer.

**4. How do you convert between binary, decimal, and hexadecimal systems?**

* **Binary to Decimal:** Multiply each binary digit ($0$ or $1$) by $2^n$, where $n$ is its positional weight from right to left (starting at $0$), and sum the results. (e.g., $10000001_2 = 128 + 1 = 129_{10}$).
* **Decimal to Binary:** Repeatedly divide the decimal number by $2$ and record the remainders from bottom to top, or subtract the largest possible powers of $2$ ($128, 64, 32, 16, 8, 4, 2, 1$).
* **Hexadecimal Conversion:** Hex uses base-16 ($0\text{-}9, \text{A}\text{-}\text{F}$). Each hex digit corresponds directly to a 4-bit binary nibble (e.g., $\text{A}_{16} = 10_{10} = 1010_2$; $\text{FF}_{16} = 255_{10} = 11111111_2$).

**5. How many bits is an IPv4 address and how is it formatted?**
An IPv4 address is **32 bits** long. It is formatted in **dotted-decimal notation**, consisting of four 8-bit sections called octets, separated by periods (e.g., `192.168.1.1`).

**6. Difference between private and public IPs?**

* **Public IP Addresses:** Globally unique addresses assigned by IANA/ISPs that are publicly routable over the internet.
* **Private IP Addresses:** Reserved for internal use within private networks and are not routable over the public internet (defined by RFC 1918):
* **Class A:** `10.0.0.0` – `10.255.255.255`
* **Class B:** `172.16.0.0` – `172.31.255.255`
* **Class C:** `192.168.0.0` – `192.168.255.255`



**7. What does CIDR notation (e.g. /24) represent?**
Classless Inter-Domain Routing (CIDR) notation denotes the length of the subnet mask. The value following the slash indicates how many contiguous bits from the left represent the fixed network portion of the address, leaving the remaining bits for host assignment. For example, `/24` means the first 24 bits are the network mask (`255.255.255.0`), allowing for $2^{(32-24)} - 2 = 254$ usable host addresses.

**8. Why is subnetting important?**
Subnetting divides a large network into smaller, manageable sub-networks. This is important to:

* Reduce broadcast traffic and conserve network bandwidth.
* Enforce security boundaries by isolating sensitive departments or asset tiers.
* Optimize IP address allocation and reduce address wastage.

**9. Difference between broadcast, multicast, and unicast?**

* **Unicast:** One-to-one communication. Traffic is sent from a single source to a single specific destination.
* **Multicast:** One-to-many-selected communication. Traffic is delivered from a single source to a specific group of interested hosts matching a multicast IP address (e.g., `224.0.0.1`).
* **Broadcast:** One-to-all communication. Traffic is sent from a single source to every host within the local broadcast domain (e.g., MAC `FF:FF:FF:FF:FF:FF` or IP `255.255.255.255`).

**10. What is a MAC address and how does a switch use it?**

* **MAC Address:** A 48-bit, unique hardware identifier burned into a device's Network Interface Card (NIC) at Layer 2.
* **Switch Utilization:** A switch reads the source MAC address of incoming frames to populate its Content Addressable Memory (CAM) table. It then checks the frame's destination MAC address against this table to forward the frame out of the specific port where that device is connected, rather than broadcasting it to all ports.

**11. How does a MAC flooding attack work?**
An attacker floods a switch with thousands of fake source MAC addresses. This exhausts the storage capacity of the switch's CAM table. Once full, the switch can no longer learn new addresses and falls back into a fail-open state, broadcasting all subsequent incoming frames out of every port (acting like a hub). This allows the attacker to capture network traffic using a packet sniffer.

**12. What is the ARP protocol for?**
The Address Resolution Protocol (ARP) resolves known Layer 3 logical IP addresses into corresponding Layer 2 physical MAC addresses within a local network segment.

**13. How does ARP spoofing/poisoning work?**

Because the ARP protocol lacks authentication, an attacker can send unsolicited, forged ARP responses to devices on the local network. These responses map the IP address of a legitimate device (such as the default gateway) to the attacker's own physical MAC address. This redirects network traffic through the attacker's machine, enabling Man-in-the-Middle (MITM) sniffing or data manipulation.

**14. What are VLANs used for?**
Virtual Local Area Networks (VLANs) logically segment a single physical switch infrastructure into multiple distinct broadcast domains. This enhances security, controls broadcast storms, and isolates network traffic without requiring separate physical hardware.

**15. How does a VLAN hopping attack happen?**

* **Switch Spoofing:** An attacker configures their device to spoof Dynamic Trunking Protocol (DTP) signaling, tricking an access port into establishing a trunk link. This grants the attacker direct access to all VLANs permitted across that trunk.
* **Double Tagging:** An attacker transmits a frame appended with two explicit $802.1\text{Q}$ headers. The native switch strips away the outer tag and forwards the frame along its native trunk port without inspecting the inner tag. The secondary receiving switch then processes the second tag, delivering the packet to a different target VLAN.

**16. Difference between trunk port and access port?**

* **Access Port:** Belongs to a single specific VLAN. It carries untagged traffic directly to and from end devices like workstations or printers.
* **Trunk Port:** Configured to carry traffic for multiple VLANs simultaneously across interconnecting switches or routers. It identifies individual streams by adding an $802.1\text{Q}$ tag to the frame headers.

**17. Difference between router, switch, and hub?**

* **Hub (Layer 1):** A legacy device that repeats incoming bit streams out of all physical ports, creating a single collision and broadcast domain.
* **Switch (Layer 2):** An intelligent device that forwards traffic inside a local network based on destination MAC addresses, isolating collision domains per port.
* **Router (Layer 3):** A device that forwards data packets between different networks based on logical destination IP addresses and routing tables.

**18. Difference between L2 and L3 switch?**

* **Layer 2 Switch:** Forwards traffic within the same broadcast domain using MAC addresses. It cannot route packets between different subnets or VLANs without an external router ("router-on-a-stick").
* **Layer 3 Switch:** Combines the wire-speed packet forwarding of a switch with the routing capabilities of a router. It uses application-specific integrated circuits (ASICs) to route traffic between internal VLANs at hardware speed.

**19. What is a default gateway for?**
The default gateway is the local router interface that host devices use to forward traffic destined for IP addresses outside their local subnet.

**20. Name the 4 DHCP phases (DORA).**

1. **Discover (Broadcast):** The client broadcasts a request to locate available DHCP servers on the network segment.
2. **Offer (Unicast/Broadcast):** Available DHCP servers respond with an available IP configuration offer.
3. **Request (Broadcast):** The client broadcasts an official request to accept the specific configuration offer received.
4. **Acknowledge (Unicast/Broadcast):** The server sends a final confirmation mapping the IP address, lease duration, and options to the client.

**21. What does DHCP spoofing mean?**
An attacker sets up an unauthorized, rogue DHCP server on the local network segment. When legitimate clients broadcast a DHCP Discover request, the rogue server responds faster than the official server, issuing IP configurations that assign the attacker's machine as the default gateway or primary DNS server. This places the attacker in a Man-in-the-Middle position.

**22. How does the TCP 3-way handshake work?**

1. **SYN (Synchronize):** The client sends a packet with the `SYN` flag set and a random initial sequence number ($ISN_C$) to request a connection.
2. **SYN-ACK (Synchronize-Acknowledge):** The server responds with both the `SYN` and `ACK` flags set, acknowledging the client's sequence number ($ISN_C + 1$) and sending its own initial sequence number ($ISN_S$).
3. **ACK (Acknowledge):** The client sends a final packet with the `ACK` flag set, confirming receipt of the server's sequence number ($ISN_S + 1$), and the connection is established.

**23. Difference between TCP and UDP? Beyond "reliable/fast" what other details?**

* **TCP (Transmission Control Protocol):** Byte-stream-oriented protocol. Uses a 20-byte minimum header, manages sequence numbering, requires acknowledgments for data delivery, tracks window sizes to control traffic flow, handles packet reassembly, and retransmits dropped packets.
* **UDP (User Datagram Protocol):** Message-oriented protocol. Uses a minimal 8-byte header, has no connection state, does not guarantee packet delivery or ordering, has no built-in flow control or congestion management mechanisms, and relies on the upper-layer application to handle data loss.

**24. Difference between connection-oriented and connectionless?**

* **Connection-Oriented:** Requires endpoints to establish a logical session before transmitting data (e.g., the TCP 3-way handshake). It maintains state variables to ensure reliable delivery and orderly teardown.
* **Connectionless:** Devices transmit data immediately to the destination without verifying if the receiver is online, available, or ready (e.g., UDP, IP).

**25. What is a SYN flood attack based on the three-way handshake?**
An attacker sends a high volume of `SYN` requests to a target server using spoofed source IP addresses. The server responds with `SYN-ACK` packets and allocates system resources in its memory connection table to await the final `ACK`. Because the source IPs are spoofed, the final `ACK` packets never arrive. These incomplete "half-open" connections consume the server's connection table capacity, preventing legitimate users from establishing sessions.

**26. Port number categories (well-known, registered, dynamic)?**

* **Well-Known Ports ($0$ – $1023$):** Reserved for core system privileges and standard network protocols (e.g., HTTP, SSH).
* **Registered Ports ($1024$ – $49151$):** Assigned by IANA to specific vendor applications and proprietary software services (e.g., RDP, Microsoft SQL Server).
* **Dynamic / Private Ports ($49152$ – $65535$):** Used as temporary, short-lived ephemeral source ports by client machines initiating outbound network sessions.

**27. Default ports for HTTP, HTTPS, FTP, SSH, RDP, DNS, SMB?**

* **HTTP:** 80
* **HTTPS:** 443
* **FTP:** 20 (Data Transfer), 21 (Control Connection)
* **SSH:** 22
* **RDP:** 3389
* **DNS:** 53
* **SMB:** 445

**28. When does DNS use UDP vs TCP?**

* **UDP Port 53:** Used for standard, everyday name resolution lookups (queries and responses) because the small payload size allows for fast processing with minimal network overhead.
* **TCP Port 53:** Used when response data exceeds 512 bytes (such as responses using DNSSEC extensions) and for executing complete DNS Zone Transfers (`AXFR`/`IXFR`) between primary and secondary name servers to ensure reliable delivery.

**29. Difference between recursive and iterative DNS queries?**

* **Recursive Query:** The client asks its local DNS resolver to find an address, shifting the responsibility to the resolver. The resolver must return either the requested IP address or a definitive error message, querying other name servers on the client's behalf if needed.
* **Iterative Query:** The DNS resolver queries authoritative nameservers step-by-step. If a nameserver doesn't know the exact address, it responds with a referral to the next authoritative nameserver down the hierarchy (Root $\rightarrow$ TLD $\rightarrow$ Authoritative), and the resolver queries that next server.

**30. How does a DNS cache poisoning attack work?**
An attacker sends forged DNS response packets to a caching resolver before the legitimate authoritative nameserver can respond to an active query. If the attacker successfully guesses the query's Transaction ID (TXID) and source port, the resolver accepts the fake response and saves the malicious IP mapping into its cache. As a result, subsequent users who query that domain are directed to the attacker's malicious site.

**31. What is DNS Tunneling?**
A method used by attackers to bypass network security controls by tunneling non-DNS traffic through the standard DNS protocol. An attacker encodes malicious commands, stolen data, or full command-and-control (C2) traffic inside DNS query structures (such as subdomains in `TXT` or `AAAA` requests). This traffic is sent to an external authoritative nameserver controlled by the attacker, allowing them to bypass traditional firewalls that leave port 53 unmonitored.

**32. Difference between NAT and PAT?**

* **NAT (Static/Dynamic 1-to-1):** Maps an internal private IP address directly to a single public IP address. It modifies the Layer 3 IP header but does not change Layer 4 port information.
* **PAT (Port Address Translation / NAT Overload):** Maps multiple internal private IP addresses to a single public IP address by tracking unique Layer 4 source port numbers. This allows thousands of internal hosts to share one public IP address simultaneously.

**33. What is ICMP used for and what packet types does it contain?**

* **Purpose:** The Internet Control Message Protocol (ICMP) is used by network devices to send error messages, diagnostic information, and operational status reports (operating at Layer 3).
* **Common Packet Types:**
* **Type 8:** Echo Request (used by `ping`).
* **Type 0:** Echo Reply (response to a `ping`).
* **Type 3:** Destination Unreachable (indicates a packet cannot be delivered).
* **Type 11:** Time Exceeded (indicates a packet's Time-To-Live counter dropped to zero).



**34. Which protocols do ping and traceroute use?**

* **Ping:** Uses **ICMP** exclusively (Type 8 Echo Request and Type 0 Echo Reply).
* **Traceroute:** * On **Windows** (`tracert`), it uses **ICMP Echo Requests** with incrementally increasing Time-To-Live (TTL) values.
* On **Linux/Unix** (`traceroute`), it typically sends **UDP datagrams** targeting high-numbered ports, alongside increasing TTL values, while listening for incoming ICMP Time Exceeded responses.



**35. What are the main functions of Session and Presentation layers?**

* **Session Layer (Layer 5):** Establishes, maintains, synchronizes, checkpoint-recovers, and tears down logical communication sessions between applications.
* **Presentation Layer (Layer 6):** Standardizes data formats, translates data between application encodings (e.g., ASCII to EBCDIC), and handles data compression and encryption/decryption (e.g., TLS processing).

**36. Difference between SSL and TLS?**

* **SSL (Secure Sockets Layer):** A legacy, deprecated cryptographic protocol developed by Netscape. All versions (SSLv1, SSLv2, SSLv3) are considered insecure due to architectural vulnerabilities like POODLE.
* **TLS (Transport Layer Security):** The modern, secure successor to SSL defined by the IETF. It uses stronger cryptographic algorithms and safer key-derivation functions. Current secure standards are TLS 1.2 and TLS 1.3.

**37. How does the TLS handshake work?**

1. **Client Hello:** The client sends its supported TLS versions, a list of compatible cipher suites, and a random string.
2. **Server Hello:** The server responds with its selected TLS version, chosen cipher suite, its digital certificate (containing its public key), and another random string.
3. **Authentication & Key Exchange:** The client verifies the server's certificate against its trusted Root Certificate Authorities. It then generates a pre-master secret key, encrypts it with the server's public key, and sends it to the server.
4. **Session Key Derivation:** Both parties use the random strings and the pre-master secret to generate identical, symmetric session keys.
5. **Finished:** Both sides send encrypted messages confirming that future communications will use the new symmetric session keys, completing the secure connection.

**38. What makes HTTPS secure (SSL/TLS)?**

* **Confidentiality:** Encrypts data transmitted over the network using symmetric session keys, preventing eavesdroppers from reading the traffic.
* **Integrity:** Appends a Message Authentication Code (MAC) or hashed checksum to each packet, ensuring data cannot be altered or tampered with in transit without detection.
* **Authentication:** Uses digital certificates verified by trusted third-party Certificate Authorities (CAs) to confirm the user is connecting to the legitimate website, preventing impersonation attacks.

**39. Difference between bandwidth and throughput?**

* **Bandwidth:** The theoretical maximum volume of data that a network link can transmit over a specific timeframe (e.g., a 1 Gbps fiber link).
* **Throughput:** The actual volume of successful data delivered over that link under real-world conditions, after factoring in protocol overhead, network congestion, and packet errors.

**40. What do latency, jitter, and packet loss mean?**

* **Latency:** The total time delay it takes for a data packet to travel from its source to its destination across the network.
* **Jitter:** The variance or instability in packet arrival times over a network connection, which can disrupt real-time traffic like VoIP or video streams.
* **Packet Loss:** The percentage of data packets transmitted by a source that fail to reach the destination, typically caused by network congestion, faulty hardware, or signal degradation.

---

### B. Scenario-Based Questions (41–80)

**41. A user says "internet works but I can't reach an internal service". How do you troubleshoot using OSI?**

* **Layer 1/2 (Physical/Data Link):** Skip detailed checks since the internet works, confirming the physical connection and local link are fine.
* **Layer 3 (Network):** Run `ping [internal-IP]` to check layer connectivity. Check the local routing table using `ip route` or `route print` to ensure internal traffic is routed correctly.
* **Layer 4 (Transport):** Use `nslookup` or `dig` to verify internal DNS name resolution. If the name resolves, check if the specific service port is open and reachable using `nc -zv [IP] [Port]` or `telnet`.
* **Layer 5-7 (Application):** Inspect application firewall rules and review local logs to see if access is blocked by an explicit security policy or a TLS handshake error.

**42. What happens when a switch's MAC table fills up?**
When a switch's CAM table fills up during a MAC flooding attack, it can no longer store new address mappings. To ensure network availability, the switch drops down into a fail-open mode, treating all new incoming traffic as unknown unicast frames. It broadcasts these frames out of every port within the VLAN segment. This changes the switch's behavior to match a hub, allowing any device connected to that segment to sniff and capture other systems' network traffic. Administrators can defend against this by enabling **Port Security** to limit the number of allowed MAC addresses per interface.

**43. VLANs help with security, but why aren't they a complete solution?**
VLANs isolate Layer 2 broadcast domains but do not provide deep security inspection on their own. Attackers can bypass this isolation using techniques like **VLAN Hopping** (via double-tagging or exploiting misconfigured dynamic trunk links using DTP). Additionally, once traffic leaves a VLAN and passes through a Layer 3 router or switch to communicate with other subnets, it can bypass security controls unless restricted by explicit Access Control Lists (ACLs) or inspected by an internal firewall.

**44. What can happen to a user after DHCP spoofing?**
If a user accepts an IP configuration from an unauthorized rogue DHCP server, their internet traffic can be compromised. By assigning the attacker's IP address as the client's default gateway, the attacker places themselves in a Man-in-the-Middle position. This allows them to capture, analyze, or modify unencrypted network data. If the rogue configuration also changes the primary DNS server, the attacker can redirect the user's web requests away from legitimate sites toward malicious clone pages or phishing portals.

**45. Why is ARP's trust-based behavior a serious L2 problem?**
The ARP protocol is inherently vulnerable because it lacks any form of identity verification or authentication. Devices on the network accept and process ARP replies even if they never sent a corresponding ARP request. This trust-based design allows an attacker to send forged ARP responses across the local segment, mapping another device's IP address to their own MAC address. To mitigate this risk, administrators can deploy **Dynamic ARP Inspection (DAI)** on their switches, which cross-references incoming ARP replies against a trusted DHCP Snooping database.

**46. Why isn't "TCP reliable, UDP fast" a sufficient answer?**
An incomplete answer misses the technical mechanisms that define these behaviors:

* **TCP** achieves reliability through precise structural features: it uses a 3-way handshake to establish connection states, assigns sequence numbers to guarantee in-order delivery, requires explicit packet acknowledgments (ACKs), and uses sliding windows for flow and congestion control. This structure adds processing overhead and requires retransmitting lost packets, which can increase latency.
* **UDP** avoids this overhead by transmitting standalone datagrams without establishing a connection state, tracking sequence numbers, or awaiting acknowledgments. This makes it ideal for real-time services like streaming or VoIP, where speed is critical and handling packet loss is left to the application layer.

**47. How is subnetting used as a security tool?**
Subnetting improves security by dividing a flat network into isolated network segments. This minimizes the size of broadcast domains, preventing network sniffing tools from capturing traffic outside their specific group. It also establishes logical boundaries where network administrators can apply strict Access Control Lists (ACLs) and stateful firewall rules. This segment isolation limits an attacker's ability to move laterally across the network if an individual endpoint is compromised.

**48. An employee says "ping fails but the site loads". Cause?**
This scenario usually occurs when a network security control blocks ICMP traffic while allowing standard web traffic. A firewall along the transit path or on the destination server may be configured with a security policy that drops ICMP Echo Requests (Type 8) to prevent network scanning and mapping. However, the same firewall keeps ports 80 and 443 open, allowing the website's HTTP/HTTPS application traffic to load normally.

**49. A new admin asks "why is DNS so important?". Explain with real scenarios.**

* **Active Directory Reliance:** In Windows enterprise networks, DNS is critical. If DNS resolution fails, workstations cannot locate Domain Controllers via `_msdcs` SRV records, which blocks domain logins, prevents Group Policy updates, and halts Active Directory replication.
* **Security & Attack Surface:** DNS is frequently targeted by attackers. If an organization lacks proper DNS security, attackers can use DNS cache poisoning to redirect corporate users to phishing sites, or exploit unmonitored outbound port 53 traffic to steal data via DNS tunneling.

**50. A web service is slow. In what order do you troubleshoot?**
Troubleshoot by following the network path step-by-step:


$$\text{Client Resolution (DNS)} \longrightarrow \text{Transport Session (TCP)} \longrightarrow \text{Cryptographic Handshake (TLS)} \longrightarrow \text{Application Processing (Server)}$$

1. **DNS Performance:** Use `dig` or `nslookup` to measure name resolution response times and ensure DNS isn't causing initial lookup delays.
2. **Network Transport:** Run `ping` and `traceroute` to analyze network latency, locate bottleneck nodes, and check for packet loss or high jitter.
3. **TCP Connection:** Use packet capture tools like Wireshark to inspect the TCP 3-way handshake, looking for signs of network issues like small window sizes or high retransmission rates.
4. **Cryptographic Negotiation:** Analyze the TLS handshake to ensure slow cipher suite processing isn't causing delays.
5. **Application Layer:** Check the server's CPU, memory, and disk usage, and review application log files to see if backend database queries or server-side scripts are slowing down performance.

**51. What technical solutions exist to detect ARP spoofing?**

* **Dynamic ARP Inspection (DAI):** A security feature on switches that validates incoming ARP packets on untrusted ports by cross-referencing them against a verified DHCP Snooping binding database.
* **ARPwatch:** A software tool that monitors network traffic and logs changes to IP-to-MAC address mappings, sending alerts when an existing IP is suddenly associated with a new MAC address.
* **SIEM / IDS Security Rules:** Intrusion Detection System rules configured to detect anomalous patterns, such as multiple identical MAC addresses claiming different IP addresses, or a high volume of unsolicited ARP replies.

**52. How does "window size" work between sender and receiver at Layer 4?**
Window size is a flow-control mechanism used by TCP to manage data transmission and prevent a fast sender from overwhelming a slow receiver. During a session, the receiver uses the `Window` field in the TCP header to specify the maximum volume of bytes it can buffer before requiring an explicit acknowledgment (ACK). The sender must stop transmitting once this threshold is reached, restarting only after receiving an ACK from the host. In modern networks, a **Window Scaling** option is included in the initial handshake to expand this limit beyond the default 65,535 bytes, allowing for faster throughput on high-speed connections.

**53. Frequent TCP retransmissions — where to look?**

* Check the physical layer for faulty cabling, damaged fiber links, or bad connectors that cause frame errors.
* Use switch management consoles to look for **duplex mismatches** or port input/output errors on interconnected interfaces.
* Look for network congestion bottlenecks that cause switch interface buffers to drop packets.
* Verify that the Maximum Transmission Unit (**MTU**) sizes match across the path to prevent packet fragmentation issues.

**54. What is "switch port security" for and what options exist?**

* **Purpose:** A Layer 2 security feature on switches that restricts port access by controlling which MAC addresses can connect to an interface.
* **Configuration Options:** Administrators can set a hard limit on the maximum number of allowed MAC addresses per port or use **Sticky MAC** settings to learn a connected device's MAC address and save it dynamically to the running configuration.
* **Violation Modes:** If an unauthorized device connects, the switch port can trigger one of three security violation modes:
* `Protect`: Drops unauthorized frames silently without logging an alert; the port remains online.
* `Restrict`: Drops unauthorized frames and increments the security violation counter, sending an SNMP trap alert.
* `Shutdown`: Immediately disables the physical interface, putting it into an `err-disable` state that requires administrative intervention to restore.



**55. What indicators help you detect a possible MITM on the network?**

* Host-level ARP tables that show the local default gateway's IP address mapped to an incorrect or duplicate physical MAC address.
* Web browsers displaying unexpected digital certificate validation warnings or untrusted root CA errors for secure HTTPS websites.
* Network monitoring tools detecting high volumes of unexpected gratuitous ARP responses or rapid MAC address changes on switch ports.
* Packet captures containing duplicate packets or out-of-order execution flows that suggest traffic is being redirected through an intermediate node.

**56. As SOC, how do you detect DNS Tunneling?**

* Monitor DNS logs for an unusually high volume of queries targeting a single, rare, or newly registered top-level domain (TLD).
* Look for an increase in non-standard query record types, such as a high ratio of `TXT`, `AAAA`, or `CNAME` requests relative to standard `A` records.
* Use security analysis tools to flag subdomains that contain high character **entropy**, long strings, or encoded payloads (e.g., `af8392dnd92.attacker.com`).
* Set up alerting rules to detect individual internal hosts that generate unusually high volumes of daily DNS traffic or exceed standard query baseline rates.

**57. An admin says "HTTPS means no sniffing risk". Your response?**
"While HTTPS encrypts the payload of a connection, it does not hide all network activity from a packet sniffer. An observer on the network can still see the source and destination IP addresses, the Layer 4 port numbers, and the target domain name transmitted in cleartext within the Server Name Indication (**SNI**) field during the initial TLS handshake. Additionally, if an organization uses outbound TLS interception gateways or lacks certificate pinning controls, encrypted traffic can be decrypted and inspected at the network perimeter."

**58. Wireshark shows TCP RST. Meaning and likely causes?**

* **Meaning:** A packet with the `RST` (Reset) flag set indicates an immediate, ungraceful termination of the TCP connection, telling the receiving host to close the session without a standard teardown.
* **Likely Causes:**
* The client attempted to connect to a closed port where no active service is listening.
* A firewall or Intrusion Prevention System (IPS) actively injected the `RST` packet to block traffic that violated a security policy.
* The backend application crashed or closed unexpectedly mid-session, forcing the operating system kernel to reset the open connection socket.



**59. ICMP echo fails both directions. Possible cause?**

* A stateful network firewall or local host software policy is explicitly dropping all incoming and outgoing ICMP packets.
* An **asymmetric routing** configuration error is directing the return packets along a different network path where they are dropped by security controls.
* A Path MTU Discovery (**PMTUD**) failure is causing packets to exceed network size limits without generating an error, creating a black hole that drops the traffic.

**60. You must allow only certain VLANs on a trunk port. How do you configure?**

1. Access the switch configuration interface and enter the target trunk interface profile.
2. Use explicit management commands to restrict the trunk, replacing default wide-open settings with an allowed list (e.g., `switchport trunk allowed vlan 10,20`).
3. Change the **Native VLAN** setting away from the default factory value (VLAN 1) to an isolated, unused VLAN ID (e.g., VLAN 999) and disable untagged parsing on that ID. This helps protect the interface from VLAN hopping and double-tagging attacks.

**61. "LAN file server is slow but internet is fast". Causes?**

* The file server or switch port interface is experiencing a duplex mismatch or auto-negotiation failure, causing frame collisions and packet drops.
* Legacy or unoptimized configuration settings are active within the **SMB** file transfer protocol, causing high protocol overhead over local links.
* Faulty Large Receive Offload (LRO) or TCP Segmentation Offload (TSO) driver configurations on the server's network card are introducing processing delays.

**62. The company asks for QoS for VoIP. How do you configure?**

* Configure edge devices to classify and tag voice traffic headers at Layer 3 using Differentiated Services Code Point (**DSCP**) markings (typically assigning **EF / Expedited Forwarding** to voice payloads and **CS3/AF31** to call signaling).
* Map these markings to Layer 2 Class of Service (**CoS**) values (typically CoS 5) on trunk links.
* Configure network switches and routers to place this tagged traffic into a high-priority **Low-Latency Queue (LLQ)**, ensuring voice packets are processed ahead of standard data traffic to minimize latency and jitter.

**63. Forensic collection in an ARP-spoofed network — how?**

1. Capture immediate snapshots of the local ARP tables (`arp -a`) on compromised systems to document the malicious IP-to-MAC address mappings before they clear.
2. Extract the active CAM MAC address tables and log histories from the interconnecting local network switches to find the physical port where the attacker is connected.
3. Set up a secure **SPAN port** or network TAP on the switch to redirect and preserve a copy of the network traffic into a secure `.pcap` file for analysis.

**64. What practical problems can NAT create?**

* NAT breaks end-to-end trace mapping, making it harder to track security incidents back to the original internal host IP address using external perimeter logs.
* It can disrupt legacy protocols that embed IP addresses inside the application payload (such as FTP Active Mode or SIP for VoIP), requiring specialized Application Layer Gateways (ALGs) to process the traffic.
* It complicates peer-to-peer connections and incoming session setups, requiring complex workaround techniques like STUN or manual port forwarding.

**65. Ordered CLI commands to investigate layer-by-layer?**
Use these commands in order to narrow down network issues:

1. `ip addr` / `ip route` (Check Layer 1–3 local IP configurations and interface states).
2. `ping [Target-IP]` (Verify Layer 3 network connectivity to the destination).
3. `traceroute [Target-IP]` / `tracert` (Identify the network hops along the path and locate routing drops).
4. `nslookup [Domain]` / `dig` (Test Layer 7 DNS name resolution functionality).
5. `nc -zv [Target-IP] [Port]` / `telnet` (Verify if the specific Layer 4 target service port is open).
6. `ss -tuna` / `netstat` (Inspect active network socket connections and listening ports on the local host).

**66. An admin says "MAC filtering is enough for Wi-Fi". Your response?**
"MAC filtering provides weak protection because wireless frames transmit MAC addresses in cleartext over the air. Anyone with a basic wireless packet sniffer can capture legitimate MAC addresses from active users and use standard software tools to spoof their own network card's address. To properly secure a wireless network, you should implement strong enterprise authentication standards like **WPA3 Enterprise** using $802.1\text{X}$, which requires individual cryptographic credentials rather than relying on easily faked hardware identifiers."

**67. Why is detecting a Layer 7 attack different from Layer 3?**

* **Layer 3 Inspection:** Simple stateful firewalls look only at packet headers, checking the source/destination IP addresses and protocol types without analyzing the data payload.
* **Layer 7 Inspection:** Next-Generation Firewalls (NGFWs) and Web Application Firewalls (WAFs) must fully decode and reconstruct the application-layer payload. This allows them to inspect the actual data content (such as HTTP requests or SQL statements) to detect complex threats like SQL injection, cross-site scripting (XSS), or fileless malware commands.

**68. How would you describe a TCP window scaling problem?**
A window scaling problem occurs when a network device or firewall between endpoints alters or drops the **Window Scale option** during the initial TCP 3-way handshake. If this option is stripped from the headers, the communication falls back to the default maximum window size of 65,535 bytes. On high-speed, high-latency connections (like long-distance WAN links), this limitation creates a throughput bottleneck, capping data transfer speeds regardless of the available network bandwidth. Administrators can resolve this by enabling **MSS Clamping** or fixing the configuration on the intermediate security device.

**69. During a mass DDoS, the network team's first reactions should be?**

* Route incoming network traffic through an upstream **DDoS cloud scrubbing service** to filter out malicious packets before they reach the organization's network links.
* Configure perimeter edge routers to apply strict rate-limiting policies and drop traffic from known malicious IP ranges or unneeded protocols.
* Coordinate with the upstream Internet Service Provider (ISP) to apply Remotely Triggered Black Hole (RTBH) filtering if necessary to protect core infrastructure.
* Move internal monitoring systems to alternate network bands to preserve communication channels for the incident response team.

**70. Difference between Layer 2 vs Layer 3 attack — explain with a real example.**

* **Layer 2 Attack:** Occurs within the local broadcast domain segment and targets hardware-based address learning mechanisms.
* *Example:* **ARP Poisoning**, where an attacker sends fake ARP responses to redirect local traffic to their MAC address. This attack cannot cross a local router interface.


* **Layer 3 Attack:** Targets logical routing, IP addressing schemes, or network-wide services across different subnets.
* *Example:* **IP Spoofing** used in an unauthenticated UDP amplification DDoS attack, where an attacker sends packets with forged source IP addresses across the internet to flood a remote target.



**71. "Why is DNSSEC still difficult?" — your answer?**
"Implementing DNSSEC is challenging because it adds complexity and administrative overhead to domain management. It requires cryptographic key management, including regular key-signing rollouts and secure parent-zone handshakes. DNSSEC responses are also significantly larger than standard DNS packets because they include digital signatures. This increase in packet size can lead to fragmentation issues and makes the infrastructure a target for UDP-based amplification DDoS attacks. Additionally, DNSSEC only secures data integrity between the zone and the resolver; it does not encrypt DNS queries to ensure user privacy."

**72. Why use a three-tier "core/aggregation/access" network design?**

* **Core Layer:** Optimized for speed and reliability, focusing on switching high-volume network traffic as fast as possible across the backbone without packet inspection delays.
* **Aggregation / Distribution Layer:** Implements network policies, handles routing between internal VLANs, manages security Access Control Lists (ACLs), and enforces traffic boundaries.
* **Access Layer:** Provides physical network connection points for end-user workstations, printers, and devices, enforcing initial port-level security controls.

**73. A vendor says "L3 switches aren't needed". Your response?**
"Layer 3 switches are necessary for high-performance corporate networks. Without them, all inter-VLAN traffic must leave the access switch and travel across a single router link to be processed before returning. This creates a network bottleneck and increases latency. Layer 3 switches use dedicated hardware (ASICs) to route traffic between subnets at wire speed directly within the switch fabric. This reduces the load on core routers and provides scalable network segmentation."

**74. A user connects to VPN but internal services fail. Causes?**

* The VPN gateway is configured with a **split-tunneling** policy that fails to push the correct internal network routing rules to the user's client machine, causing internal traffic to route toward the public internet instead.
* The client's connection is missing the required internal DNS search suffix configuration, preventing them from resolving internal service hostnames.
* Security Access Control Lists (ACLs) or firewall policies on the VPN termination gateway are blocking that specific user group from reaching internal subnets.

**75. TLS 1.0 must be disabled. How to plan across all services?**

1. Use network scanning tools (like Nessus or Qualys) and external testing services (like SSL Labs) to build a complete inventory of all systems running TLS 1.0/1.1.
2. Review client systems to ensure all user web browsers, operating systems, and partner applications support TLS 1.2 or TLS 1.3.
3. Coordinate with application vendors to update software configurations, disabling legacy protocols in staging environments first to test for compatibility issues.
4. Schedule the changes in production during a maintenance window, and have a clear rollback plan ready in case critical legacy services fail to connect.

**76. "If MTU < 1500, what happens?" — a specific app-level result?**
If a network link's Maximum Transmission Unit (MTU) is configured below the standard 1500 bytes, any data packet larger than that threshold must be broken down into smaller pieces through IP fragmentation. If an intermediate network device blocks ICMP traffic, it can break Path MTU Discovery (PMTUD). This prevents the device from sending a "Fragmentation Needed" message back to the source, causing the fragmented packets to drop silently. At the application level, this leads to connection timeouts or hanging sessions where small interactive commands work fine, but large data transfers (like downloading a file or opening a large web page) fail completely.

**77. A host has a link, gets DHCP, but DNS fails. Cause?**

* The assigned DNS server IP address in the DHCP scope configuration is incorrect or points to a non-existent host.
* A firewall along the network path or an explicit access rule on the DNS server is blocking UDP/TCP port 53 traffic from that specific client subnet.
* The DNS server's background service has crashed, or it is configured with an ACL that blocks recursive query requests from that host's network segment.

**78. Someone objects to "port security on every port" — your reply?**
"While managing port security on every access port adds administrative overhead—especially when users move devices or IT hardware is replaced—leaving ports unsecured creates a major risk. An attacker can plug a rogue device or unauthorized access point into any open wall jack and gain direct access to our internal network. If the management overhead of MAC-based port security is too high for our team, we should plan to deploy a centralized **Network Access Control (NAC)** solution using $802.1\text{X}$ authentication to secure our network connections dynamically."

**79. A LAN has a broadcast storm. Cause and mitigation?**

* **Cause:** A physical loop in the network topology (such as two switches connected together by multiple active cables) combined with a missing or misconfigured **Spanning Tree Protocol (STP)**. This causes broadcast frames to circulate endlessly, consuming all available network bandwidth and crashing switch processors.
* **Mitigation:** Locate and disconnect the duplicate physical link causing the loop. To prevent future storms, properly configure Spanning Tree Protocol across all devices, enable features like **BPDU Guard** and **Root Guard** on access interfaces, and apply **Storm Control** thresholds on switch ports to automatically drop excessive broadcast traffic.

**80. What does "BGP prefix hijack" mean and what protections?**

* **Meaning:** Occurs when an unauthorized Autonomous System (AS) broadcasts a routing announcement claiming ownership of IP address ranges (prefixes) that belong to a different organization. This misdirection routes internet traffic destined for the legitimate organization through the attacker's network, enabling traffic interception, eavesdropping, or denial-of-service conditions.
* **Protections:** Implement **Resource Public Key Infrastructure (RPKI)** to cryptographically validate that an Autonomous System is authorized to announce specific IP prefixes. Organizations should also apply strict prefix filtering on edge routers and use specialized third-party monitoring services to detect anomalous routing announcements in real time.

---

### C. Checklist-Style Questions (81–100)

* **[x] OSI 7 layers?** Application, Presentation, Session, Transport, Network, Data Link, Physical.
* **[x] TCP vs UDP?** TCP is connection-oriented, reliable, guarantees packet ordering, and uses flow control; UDP is connectionless, lightweight, fast, and does not guarantee delivery.
* **[x] 3-way handshake?** The connection establishment sequence used by TCP: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`.
* **[x] DHCP DORA?** The four sequential phases of DHCP address assignment: **D**iscover, **O**ffer, **R**equest, **A**cknowledge.
* **[x] NAT vs PAT?** NAT maps private IP addresses to public IP addresses using a 1-to-1 relationship; PAT maps multiple private IPs to a single public IP address by using unique Layer 4 source ports.
* **[x] What is ARP for?** Resolves a known Layer 3 logical IP address into its corresponding Layer 2 physical MAC address on a local network segment.
* **[x] MAC flooding?** An attack that floods a switch's CAM table with fake MAC addresses, forcing it into a fail-open mode where it broadcasts traffic out of all ports like a hub.
* **[x] What are VLANs for?** Logically segmenting a physical switch infrastructure into multiple distinct, isolated Layer 2 broadcast domains.
* **[x] Trunk vs access port?** An access port carries untagged traffic for a single VLAN; a trunk port carries tagged traffic for multiple VLANs simultaneously using $802.1\text{Q}$ headers.
* **[x] Router/switch/hub difference?** A hub repeats raw bit streams out of all ports; a switch intelligently forwards frames within a local network using MAC addresses; a router forwards packets between different networks using IP addresses.
* **[x] DNS recursive vs iterative?** In a recursive query, the resolver handles the entire lookup and returns the final answer to the client; in an iterative query, the resolver queries nameservers step-by-step, following referrals down the DNS hierarchy.
* **[x] DNS port numbers?** Port **53** over UDP for standard name resolution queries; port **53** over TCP for zone transfers and large packet payloads.
* **[x] What is ICMP for?** Used by network devices to transmit operational status data, diagnostic information, and error messages (operating at Layer 3).
* **[x] SSL vs TLS?** SSL is an insecure, deprecated encryption protocol; TLS is its modern, secure successor used to protect network traffic.
* **[x] What makes HTTPS secure?** Uses SSL/TLS protocols to provide data encryption (confidentiality), cryptographic hashing (integrity), and verified digital certificates (authentication).
* **[x] Bandwidth vs throughput?** Bandwidth is the maximum theoretical capacity of a network link; throughput is the actual volume of data successfully transmitted under real-world conditions.
* **[x] What does CIDR /24 mean?** Indicates a subnet mask where the first 24 bits represent the network portion (`255.255.255.0`), leaving 8 bits to support up to 254 usable host addresses.
* **[x] SYN flood attack?** A Denial-of-Service attack that floods a target server with TCP `SYN` packets using spoofed source IPs, consuming its connection memory table with incomplete half-open connections.
* **[x] DNS Tunneling?** A method used by attackers to encapsulate non-DNS payloads (like C2 commands or stolen data) inside standard DNS queries to bypass perimeter firewalls.
* **[x] What is a default gateway for?** The local router interface IP address where host devices send network traffic that is destined for destinations outside their local subnet boundary.
---

## 6. PYTHON
*Source modules: Building Blocks of Python, Libraries, Web Automation, Encryption, Networking with Python, Scapy, Pandas*

### A. General Questions (1–40)

**1. Python is an interpreted language — what does that mean and how does it differ from compiled languages like C++?**

* **Interpreted:** Python code is executed line-by-line by the Python Interpreter (CPython). It is converted into bytecode, which the interpreter then runs on a virtual machine.
* **Compiled (C++):** Source code is translated directly into machine-specific binary (machine code) by a compiler before execution.
* **Difference:** Compiled languages are generally faster at runtime but require a compilation step for every target platform. Interpreted languages are more portable and faster to develop/debug, but they typically have slower execution speeds.

**2. Main Python data types and their mutability?**

* **Mutable:** `list`, `dict`, `set`. (Their content can be changed after creation).
* **Immutable:** `int`, `float`, `str`, `bool`, `tuple`. (Once created, their value cannot be changed).

**3. Difference between list and tuple?**

* **List:** Mutable, defined with `[]`, used for collections of items that may change (e.g., a queue).
* **Tuple:** Immutable, defined with `()`, used for fixed data collections (e.g., coordinates, database records). They are faster and safer for protecting data from accidental changes.

**4. Difference between set and dict?**

* **Set:** A collection of unique, unordered elements (no keys). Used for membership testing and eliminating duplicates.
* **Dict:** A collection of unique key-value pairs. Used for mapping specific keys to specific values.

**5. Difference between mutable and immutable, with examples?**

* **Mutable:** Objects can be modified after assignment. *Example:* `my_list = [1, 2]; my_list[0] = 9` (valid).
* **Immutable:** Objects cannot be modified. Any "change" actually creates a new object in memory. *Example:* `x = "hello"; x = x + " world"` (a new string is created).

**6. Difference between `is` and `==` in Python?**

* `==` checks for **equality** (do the objects have the same value?).
* `is` checks for **identity** (do both variables point to the same location in memory?).

**7. Difference between shallow copy and deep copy?**

* **Shallow Copy:** Creates a new container object, but populates it with *references* to the items found in the original. If you change a nested mutable object in the shallow copy, the original changes too.
* **Deep Copy:** Creates a new container and recursively copies every object found in the original, ensuring the copy is entirely independent.

**8. Difference between list comprehension and for loop?**

* **List Comprehension:** Concise, idiomatic, and generally faster because the loop is pushed into C-level code.
* **For Loop:** More verbose; better if the logic is complex, requires multiple lines, or involves side effects.

**9. What is a decorator and when is it used?**

* A decorator is a function that takes another function and extends its behavior without explicitly modifying it. Used for logging, authentication, performance timing, or caching.

**10. What is a generator and how does it differ from a list?**

* A generator uses `yield` to produce values on-the-fly, one at a time. A list stores all elements in memory at once. Generators are memory-efficient for large or infinite data streams.

**11. Difference between `yield` and `return`?**

* `return` exits a function and returns a single value, destroying the function's local state.
* `yield` pauses the function, returns a value, and saves the function's state, allowing it to resume exactly where it left off.

**12. Exception handling (try/except/finally)?**

* `try`: Code that might raise an error.
* `except`: Logic to run if a specific error occurs.
* `finally`: Logic that **always** runs, regardless of whether an error occurred (used for cleanup).

**13. What is PEP 8?**

* The official Style Guide for Python. It dictates conventions (naming, spacing, indentation) to ensure readability and maintainable code.

**14. Why are virtual environments used?**

* To isolate dependencies for specific projects, preventing "dependency hell" where different projects require different versions of the same library.

**15. Difference between `pip` and `pipx`?**

* `pip` installs packages into the Python environment (often global or virtual).
* `pipx` is used to install and run Python command-line applications in isolated environments, keeping them separate from your library dependencies.

**16. Difference between module and package?**

* **Module:** A single `.py` file containing code.
* **Package:** A folder containing a collection of modules and an `__init__.py` file.

**17. Difference and risk between `import x` and `from x import *`?**

* `import x` keeps the namespace clean (you must use `x.function()`).
* `from x import *` imports everything into the current namespace. **Risk:** Overwriting existing functions (name collisions) and making code difficult to read/debug.

**18. What is the GIL (Global Interpreter Lock)?**

* A mutex that allows only one thread to hold control of the Python interpreter at a time. It prevents race conditions in memory management but limits performance in multi-threaded CPU-bound programs.

**19. Multithreading vs multiprocessing?**

* **Multithreading:** Best for **I/O-bound** tasks (waiting for web requests/files). Limited by the GIL.
* **Multiprocessing:** Best for **CPU-bound** tasks (calculations). Spawns separate processes, each with its own Python interpreter and memory, bypassing the GIL.

**20. What is asyncio used for?**

* Writing concurrent code using the `async`/`await` syntax. It manages many I/O-bound tasks on a single thread efficiently.

**21. What is the `requests` library for?**

* Sending HTTP requests easily to interact with web services and APIs.

**22. `requests` vs `urllib`?**

* `requests` is high-level, human-readable, and feature-rich (auto-decoding, session management). `urllib` is the built-in, low-level standard library—more powerful but verbose.

**23. Selenium vs Playwright?**

* **Selenium:** The long-standing industry standard, supports many languages/browsers, but can be slower and flaky.
* **Playwright:** Modern, faster, supports native auto-waiting and browser contexts; highly recommended for newer web automation projects.

**24. What is BeautifulSoup for?**

* Parsing HTML/XML documents to extract data (web scraping).

**25. Why must robots.txt be respected?**

* Ethical and legal obligation to comply with the site owner's rules regarding which parts of their site are crawlable, preventing server overload and legal issues.

**26. String formatting methods?**

* **f-strings:** `f"{var}"` (Fastest, readable, Python 3.6+).
* **.format():** `"{}".format(var)` (Flexible).
* **%:** `"%s" % var` (Legacy, C-style).

**27. `hashlib` algorithms?**

* Supports SHA-256, SHA-512, MD5 (insecure), SHA-1 (insecure).

**28. Symmetric vs Asymmetric?**

* **Symmetric:** Same key for encryption/decryption (e.g., `cryptography.fernet`). Fast.
* **Asymmetric:** Public key encrypts, private key decrypts (e.g., RSA). Used for key exchange.

**29. Fernet class?**

* Provides "authenticated symmetric encryption" to ensure that data cannot be read *or* tampered with without the secret key.

**30. AES, DES, and RSA?**

* **AES:** Symmetric, industry-standard, very fast.
* **DES:** Symmetric, deprecated/insecure due to small key size.
* **RSA:** Asymmetric, used for key exchange and digital signatures.

**31. TCP server with `socket`?**

* `s = socket.socket(); s.bind(); s.listen(); conn, addr = s.accept()`.

**32. Scapy vs socket?**

* `socket` is for building network applications (like a web server). `Scapy` is for packet crafting and sniffing, allowing you to manipulate every bit of a packet (Ethernet, IP, TCP headers).

**33. Sniffing vs injection (Scapy)?**

* **Sniffing:** Passive capture of network traffic passing by.
* **Injection:** Active creation and transmission of custom packets onto the wire.

**34. Pandas Series vs DataFrame?**

* **Series:** 1D array-like object.
* **DataFrame:** 2D, table-like structure (collection of Series).

**35. File read options?**

* `read_csv`, `read_excel`, `read_json`. Common options: `sep`, `encoding`, `header`, `index_col`.

**36. `loc` vs `iloc`?**

* `loc`: Label-based selection (names of columns/rows).
* `iloc`: Integer position-based selection (index numbers).

**37. Pandas `groupby`?**

* Splits data into groups based on some criteria, applies a function (sum, mean), and combines the results.

**38. Handle NaN?**

* `df.dropna()` (remove), `df.fillna(value)` (replace).

**39. JSON/dict conversion?**

* `json.dumps(dict)` (to string), `json.loads(string)` (to dict).

**40. Logging vs print?**

* `logging` allows severity levels (INFO/ERROR), multiple outputs (file/console), and timestamps, making it production-ready. `print` is for debugging only.

---

### B. Scenario-Based Questions (41–80)

* **41-45 (Security/Files/APIs):** Use `re` for IP patterns, `collections.Counter` for frequency. APIs: `requests.Session()` is key for persistence. `eval()` is catastrophic due to arbitrary command execution (use `ast.literal_eval`).
* **46-50 (Pandas/Debug/Scapy):** `drop_duplicates` for CSVs. Multi-threaded scrapers fail if they are CPU-bound due to the GIL; use `asyncio` for I/O-bound scrapers. Scapy is chosen for pen-testing because it offers full control over raw packet structures (flags, TTLs, etc).
* **51-55 (Performance/Security):** Passwords must use **bcrypt** or **argon2** (with salts). Never use `verify=False` in requests because it disables certificate validation, inviting Man-in-the-Middle attacks.
* **56-60 (Pen-testing/Pandas/Integrity):** `pickle` is dangerous because it can execute arbitrary code upon `unpickle()`. For 5M rows in Pandas, use **Parquet** format and avoid `apply()` in favor of vectorized operations.
* **61-65 (Automation/Profiling):** `tracemalloc` identifies memory leaks. E2E encryption requires TLS + DH key exchange.
* **66-70 (Networking/Structure/Sanitization):** Always use `os.path.basename` to prevent path traversal (e.g., `../../etc/passwd`).
* **71-75 (Supply Chain/Requests):** `pip-audit` detects vulnerabilities in dependencies. Use `proxies` dictionary in `requests` for routing.
* **76-80 (Logs/Testing/Secret Mgmt):** Never log passwords; use log formatters or redact sensitive keys. Use `pytest` with fixtures for robust testing.

---

### C. Checklist-Style Questions (81–100)

* **List/Tuple:** Mutable vs Immutable.
* **Set/Dict:** Unique elements vs Key-Value pairs.
* **Mutable/Immutable:** In-place change vs New object creation.
* **is/==:** Identity (memory) vs Equality (value).
* **Shallow/Deep Copy:** Reference vs Recursive independence.
* **yield/return:** State preservation vs function termination.
* **GIL:** Single-thread Python bytecode limitation.
* **Virtualenv:** Dependency isolation.
* **Decorator:** Function that wraps another function.
* **Generator:** Lazy evaluation (iterating vs storing).
* **PEP 8:** Python style guide.
* **Requests:** HTTP API interaction.
* **Selenium/Playwright:** Browser automation.
* **BeautifulSoup:** HTML/XML parsing.
* **Symmetric/Asymmetric:** Shared key vs Public/Private pair.
* **AES/RSA:** Symmetric speed vs Asymmetric security.
* **Scapy:** Packet manipulation/sniffing.
* **Series/DataFrame:** 1D vs 2D structure.
* **loc/iloc:** Label vs Index.
* **Logging:** Severity levels and file handling.
---

## 7. SECURITY GATEWAYS
*Source modules: Security Gateways Modules 1–6 (firewalls, NGFW, IDS/IPS, VPN, NAT, AAA, DLP, web/email gateways)*

### A. General Questions (1–40)

1. What is a Security Gateway and how does it differ from a classic firewall?

2. Difference between stateless and stateful firewalls?

3. How does a stateful firewall implement connection tracking?

4. What additional features does an NGFW have?

5. Difference between L7 application firewall and L4 firewall?

6. What is a WAF and which attacks does it block?

7. Technical and operational difference between IDS and IPS?

8. Signature-based vs anomaly-based IDS?

9. What do false positive and false negative mean in IDS context?

10. Inline IPS vs SPAN-port IDS deployment difference?

11. Difference between NAT and PAT (NAT overload)?

12. Source NAT vs destination NAT difference?

13. What is a DMZ and why is it used?

14. Proxy server vs reverse proxy difference?

15. Explicit vs transparent proxy?

16. Explain each of AAA (Authentication, Authorization, Accounting).

17. Difference between RADIUS and TACACS+?

18. Authentication factors (know/have/are/somewhere/are doing)?

19. How does the Zero Trust model affect gateways?

20. What is a VPN and what types exist (site-to-site, remote access)?

21. Difference between IPsec transport and tunnel modes?

22. Difference between SSL VPN and IPsec VPN?

23. What do IKE Phase 1 and Phase 2 do in IPsec?

24. What is Diffie-Hellman used for?

25. Difference between AH and ESP in IPsec?

26. How does a DLP system work?

27. Difference between Network, Endpoint, and Cloud DLP?

28. What protection functions does an email gateway provide?

29. What are SPF, DKIM, and DMARC for?

30. Difference between spam filter and email sandbox?

31. Difference between URL filtering and content filtering?

32. How does TLS inspection (SSL bump) work and what issues does it create?

33. How does category-based filtering work in a web proxy?

34. What is a honeypot and what types exist?

35. Honeynet vs honeypot difference?

36. Antivirus vs EDR difference?

37. What is XDR (Extended Detection and Response)?

38. Difference between bot protection and DDoS protection?

39. Talk about Geo-IP filtering — benefits and limits?

40. How is a Threat Intelligence feed integrated into gateways?

### B. Scenario-Based Questions (41–80)

41. With a firewall in place, why can attacks still happen?
    *Concepts: wrong rules, allowed malicious traffic, insider, encrypted traffic, app-layer bypass, supply chain*

42. Explain stateless vs stateful firewall with a real traffic example.
    *Concepts: return traffic, connection table, asymmetric routing, simpler ACL trade-off*

43. Choosing IDS vs IPS — which criteria?
    *Concepts: inline risk, false positive cost, traffic criticality, latency, learning period*

44. Choosing SSL VPN vs IPsec VPN — which criteria?
    *Concepts: remote users vs site-to-site, ease of deployment, performance, compatibility*

45. You want to deploy TLS inspection but users complain about banking sites. How to proceed?
    *Concepts: bypass list, privacy, certificate distribution, performance, legal implications*

46. The company says "we have VPN, internal network is safe". Your reply?
    *Concepts: lateral movement, compromised endpoint, Zero Trust, segmentation, ZTNA*

47. An email gateway flags 10% of emails as dangerous. How do you assess if this is high?
    *Concepts: baseline, sender reputation, peer industry, FP rate, retro analysis*

48. HR director complains about DLP (can't send CVs). How to balance?
    *Concepts: rule tuning, business override, sensitive info types, audit trail, exception process*

49. SPF and DKIM are on but spoofing continues. How does DMARC help?
    *Concepts: alignment, policy (none/quarantine/reject), reports, gradual enforcement*

50. A new NGFW is bought with default config. First 5 steps?
    *Concepts: change default admin, segment management, baseline rules, logging, signature update*

51. Designing a honeypot — which rules to follow?
    *Concepts: isolation, no real data, monitoring, legal/policy, segment from prod*

52. An admin says "we have MFA, no need for an RDP gateway". Your reaction?
    *Concepts: defense in depth, session control, recording, geo-control, brute force visibility*

53. WAF false positives are stopping production. Your approach?
    *Concepts: detection-only mode, tuning, exclusion by path/method, gradual enforcement*

54. After deploying a new IPS rule, the network slowed down. Cause?
    *Concepts: deep inspection cost, throughput cap, asic offload, rule efficiency*

55. The company says "no internet outage" but the new firewall rule blocks traffic. How to balance?
    *Concepts: change window, rollback, staged rollout, monitor mode, communication*

56. A site-to-site VPN drops at Phase 2. Diagnostic sequence?
    *Concepts: proposal mismatch, peer logs, ACL alignment, NAT-T, PSK*

57. During a DDoS, can an NGFW handle it? Which technologies are more effective?
    *Concepts: scrubbing centers, anycast, rate limiting, ISP coordination, hybrid mitigation*

58. A teammate says "VPN logs aren't important". Your reply?
    *Concepts: incident timeline, compliance, breach attribution, retention policy*

59. Cloud firewall (FWaaS) vs on-prem — what operational changes does it require?
    *Concepts: scaling model, telemetry, identity-based rules, vendor lock-in, IaC*

60. The company says "DLP has too many FPs, let's disable it". Counter-arguments?
    *Concepts: tuning roadmap, classification, business owners, regulatory exposure, audit mode*

61. Setting up a new S2S VPN with a partner — what security requirements should be agreed first?
    *Concepts: encryption suite, key rotation, allowed prefixes, monitoring, decommission*

62. A teammate asks "why is AAA Authorization important if Authentication exists?". Reply?
    *Concepts: granular access, command authorization, role-based, post-login restrictions*

63. "Force all outbound traffic via proxy" — how do you design?
    *Concepts: PAC/WPAD, transparent proxy, bypass list, auth, monitoring, encryption inspection*

64. In firewall logs, what's the difference between "drop" and "deny"?
    *Concepts: silent drop, sent RST/ICMP, attacker fingerprinting*

65. A teammate added an "any-any allow" rule. How do you remediate?
    *Concepts: rule audit, business owner, replace with explicit, test, change record*

66. Deploying a new IPS signature — what rules?
    *Concepts: detection-first, test segment, false-positive analysis, performance, rollback*

67. An admin says "block China geo and attacks will stop". Your reply?
    *Concepts: proxy/VPN bypass, false sense of security, business impact, layered defense*

68. An SMTP relay is left open. Risk and cleanup?
    *Concepts: spam abuse, blocklist (RBL), authenticate-only relay, monitoring*

69. "We have AV, why EDR?" — your reply?
    *Concepts: behavioral detection, telemetry, threat hunting, response actions, FP context*

70. The company wants to reduce VPN and move to ZTNA. Migration plan?
    *Concepts: app inventory, identity-aware proxy, pilot users, monitoring, decommission*

71. A vendor says "5000 FW rules won't slow you down". Your reply?
    *Concepts: lookup performance, ASIC vs CPU, rule order, hit count, audit*

72. Why is a DNS gateway (DNS Firewall, RPZ) important?
    *Concepts: pre-emptive block, malware C2, phishing domain, low-cost defense*

73. A teammate asks "why is AAA Accounting important?". Reply?
    *Concepts: audit trail, compliance, forensic timeline, billing/usage*

74. "Block Telegram via IPS" — what risks?
    *Concepts: encrypted ports/443, business apps using same SNI, false positives, alternative DNS-block*

75. Zero Trust's answer to "user authenticated once, always allowed in"?
    *Concepts: continuous verification, device posture, context, session re-eval*

76. SD-WAN vs classic VPN in a hub-and-spoke design?
    *Concepts: app-aware steering, multiple links, centralized policy, performance*

77. A rule "any IP, port 443, allow" exists. How to consolidate?
    *Concepts: dst-IP/URL, app identification, justification, owner, sunset date*

78. How is "BEC" detected by an email gateway?
    *Concepts: VIP impersonation, lookalike domains, header anomaly, AI-based*

79. How to avoid "shadow rules" during WAF tuning?
    *Concepts: rule ordering, audit hits, deprecation, monitoring without enforcement*

80. The company says "let's drop the perimeter, identity-only". Your reaction?
    *Concepts: realistic phasing, hybrid model, monitoring, attack surface, dependencies*

### C. Checklist-Style Questions (81–100)

- [ ] Stateless vs stateful firewall?
- [ ] NGFW features?
- [ ] IDS vs IPS?
- [ ] False positive vs false negative?
- [ ] NAT vs PAT?
- [ ] What is a DMZ?
- [ ] Proxy vs reverse proxy?
- [ ] AAA model?
- [ ] RADIUS vs TACACS+?
- [ ] Zero Trust?
- [ ] IPsec transport vs tunnel?
- [ ] SSL VPN vs IPsec VPN?
- [ ] What is Diffie-Hellman for?
- [ ] AH vs ESP?
- [ ] How does DLP work?
- [ ] SPF/DKIM/DMARC?
- [ ] What does a WAF protect?
- [ ] What is a honeypot?
- [ ] Antivirus vs EDR?
- [ ] What is XDR?

---

*End of Essentials Track — 7 subjects × 100 questions = 700 questions total*
