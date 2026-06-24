# Domain 5 — Identity and Access Management (IAM)

**Exam Weight: 13%**

---

## 5.1 Core IAM Concepts

### Identity vs. Authentication vs. Authorization

| Concept | Question | Example |
|---------|---------|---------|
| **Identification** | Who are you? | Enter username |
| **Authentication** | Prove it | Enter password, biometric |
| **Authorization** | What can you do? | Access control policy |
| **Accountability** | What did you do? | Audit logs |

### AAA Framework
- **Authentication** — verify identity
- **Authorization** — grant appropriate access
- **Accounting (Auditing)** — record all actions

---

## 5.2 Authentication Factors

### Five Factor Categories

| Factor | Type | Examples | Weakness |
|--------|------|---------|---------|
| **Something you know** | Knowledge | Password, PIN, passphrase, security questions | Forgotten, stolen, guessed |
| **Something you have** | Possession | Smart card, hardware token (RSA SecurID), OTP app | Lost, stolen |
| **Something you are** | Inherence | Fingerprint, retina, iris, voice, face | Cannot be changed if compromised |
| **Somewhere you are** | Location | Geolocation, IP range, GPS | VPN bypass possible |
| **Something you do** | Behavior | Keystroke dynamics, signature dynamics | Variable; affected by illness |

### Multi-Factor Authentication (MFA)
Requires **two or more different factor categories** (not two of the same type).

> **Exam Tip:** Password + security question = single factor (both are "something you know"). Password + fingerprint = MFA (two different factors).

> **Scenario — MFA Factor Confusion (the #1 IAM trap):**
> A bank adds a "second factor" for login: after the password, users must answer "What was the name of your first pet?" Both are **something you know** — this is NOT MFA. An attacker who socially engineers or looks up someone's social media can likely guess both. True MFA requires categories to be different.
>
> **Valid MFA combinations:**
> - Password (know) + SMS code (have) ✓
> - PIN (know) + fingerprint (are) ✓
> - Smart card (have) + PIN (know) ✓
> - Fingerprint (are) + facial recognition (are) ✗ — both are "something you are" — same category, NOT MFA
>
> **"Somewhere you are" trap:** Geolocation is a factor, but it's easily bypassed with a VPN. Most organizations use it as a **risk signal** for adaptive authentication rather than a true second factor.

### Step-Up Authentication
Dynamic elevation of authentication requirements based on risk level of the requested resource.

---

## 5.3 Biometrics

### Performance Metrics

| Metric | Definition | Ideal |
|--------|-----------|-------|
| **FAR** (False Accept Rate) | Unauthorized user accepted as legitimate | As low as possible |
| **FRR** (False Reject Rate) | Legitimate user rejected | As low as possible |
| **CER** (Crossover Error Rate) | Point where FAR = FRR | Lowest CER = most accurate system |

> **FAR and FRR trade-off:** Increasing sensitivity reduces FAR but increases FRR (and vice versa). CER is the sweet spot where they intersect — **lower CER = better biometric system**.

> **Scenario — FAR vs. FRR Trade-off in Real Life:**
> A nuclear facility uses a fingerprint scanner to control access to a critical area. Setting the sensitivity very HIGH (strict matching required):
> - **FRR increases** — legitimate workers with slightly dirty or calloused fingers get rejected frequently. Workers get frustrated and start propping the door open (compensating control bypassed). Life safety risk.
> - **FAR decreases** — almost no unauthorized person gets through.
>
> Setting sensitivity very LOW (loose matching):
> - **FAR increases** — a partial fingerprint from an unauthorized person might match. Security risk.
> - **FRR decreases** — legitimate employees almost never get rejected.
>
> **The CER** is the threshold setting where FAR = FRR. A system with CER = 1% is more accurate than one with CER = 5% — at their respective optimal settings, the first system has lower equal-error rates.
>
> **Exam trick question:** "Which biometric system is most accurate?" → the one with the **lowest CER**, regardless of which type it is. Don't assume "retina scan is always best" — ask for the CER.

### Biometric Types (Accuracy, roughly most to least accurate)
1. Retina scan (most accurate, most invasive)
2. Iris scan
3. Fingerprint
4. Hand geometry
5. Voice recognition (least accurate for physical access)
6. Signature dynamics

---

## 5.4 Access Control Models

### Mandatory Access Control (MAC)
- Labels on subjects and objects
- Central authority (OS, security kernel) enforces access
- Users cannot change permissions
- Used in government/military classified environments
- Bell-LaPadula and Biba models implement MAC

### Discretionary Access Control (DAC)
- Owner of resource controls access
- Owner can grant/revoke access at their discretion
- Implemented via ACLs (Access Control Lists)
- Used in Windows/NTFS, Linux file permissions
- Most flexible; least secure

### Role-Based Access Control (RBAC)
- Access based on role (job function), not individual identity
- Users are assigned to roles; roles are assigned permissions
- Simplifies administration in large organizations
- Most common model in enterprise systems

### Attribute-Based Access Control (ABAC)
- Access decisions based on attributes of subject, object, environment, and action
- Highly flexible; supports complex policies ("allow if user is in HR AND resource is labeled HR AND it's business hours")
- Also called Policy-Based Access Control (PBAC)

### Rule-Based Access Control
- Access determined by rules (typically in firewall ACLs)
- Example: "Allow traffic from 10.0.0.0/8 to port 443"

### Comparison

| Model | Control By | Flexibility | Common Use |
|-------|-----------|------------|-----------|
| MAC | Central authority | Low | Government, military |
| DAC | Resource owner | High | Windows, Linux file systems |
| RBAC | Roles | Medium-High | Enterprise IT |
| ABAC | Attributes/policies | Highest | Cloud, API access |

> **Classic Exam Scenario — First Contact with Aliens:**
> Humans make contact with aliens and want to learn about them without revealing human weaknesses. Which access model is best?
>
> - **MAC** is too rigid — the strict label-based system doesn't allow the dynamic, situation-dependent information sharing that first contact requires.
> - **ABAC (or RBAC)** is the right answer — it allows **flexibility** (share some things but not others, based on context) while maintaining **strict boundaries** (no access to classified human military data). Attributes like "topic = astronomy" (allowed) vs. "topic = defense systems" (denied) make ABAC ideal for nuanced, contextual access control across organizational (or species) boundaries.
>
> **Why this matters:** The exam often uses unusual scenarios to test whether you understand the *properties* of each model, not just the name. ABAC = maximum flexibility + fine-grained control. MAC = maximum rigidity. DAC = maximum owner flexibility but minimum central control.

---

## 5.5 Authentication Protocols

### Kerberos
Network authentication protocol using **tickets** to avoid transmitting passwords.

**Components:**
- **KDC (Key Distribution Center)** — contains AS and TGS
- **AS (Authentication Service)** — issues TGT after initial login
- **TGS (Ticket Granting Service)** — issues service tickets
- **TGT (Ticket Granting Ticket)** — used to request service tickets
- **Service Ticket (ST)** — used to access specific services

**Kerberos Authentication Flow:**
```
1. Client → AS: Request TGT (with timestamp, encrypted with password hash)
2. AS → Client: TGT (encrypted with KDC secret key)
3. Client → TGS: Request service ticket (presents TGT)
4. TGS → Client: Service ticket (encrypted with service's secret key)
5. Client → Server: Present service ticket
6. Server → Client: Access granted / mutual auth
```

**Kerberos Attacks:**
- **Pass the Ticket** — steal and reuse valid tickets
- **Golden Ticket** — forge TGT using KRBTGT account hash (unlimited access)
- **Silver Ticket** — forge service ticket using service account hash
- **Kerberoasting** — request service tickets for offline password cracking

### RADIUS vs. TACACS+

| Feature | RADIUS | TACACS+ |
|---------|--------|---------|
| Transport | UDP | TCP |
| Encryption | Only password | Full packet |
| Protocol | Open | Cisco proprietary |
| AAA separation | Combined | Separate (more granular) |
| Use case | Network access (Wi-Fi, VPN) | Device administration (routers, switches) |

> **Exam Tip:** TACACS+ encrypts the **entire** authentication packet (not just the password). RADIUS only encrypts the password. TACACS+ uses TCP (more reliable); RADIUS uses UDP.

> **Scenario — RADIUS vs. TACACS+ in Context:**
> A company has two needs:
> 1. **Wi-Fi authentication** for 500 employees (802.1X EAP): Use **RADIUS**. It's the standard for network access authentication. Speed matters; UDP is fine; full encryption of the entire packet isn't critical since the EAP tunnel already encrypts credentials.
> 2. **Network device administration** — admins SSHing into 50 routers and switches with privilege levels: Use **TACACS+**. It separately handles authentication (who are you), authorization (what commands can you run at what privilege level), and accounting (log every command typed). This granularity is essential for device management. Full packet encryption protects admin commands in transit.
>
> **The key distinction for the exam:** RADIUS = *accessing* the network (users, VPN, Wi-Fi). TACACS+ = *administering* network devices (routers, switches, firewalls).

### LDAP (Lightweight Directory Access Protocol)
- Directory services (Active Directory, OpenLDAP)
- Port 389 (cleartext), Port 636 (LDAPS — encrypted)
- Used for authentication and authorization lookups
- X.500 is the underlying directory standard

---

## 5.6 Single Sign-On (SSO) and Federation

### SSO
User authenticates once and gains access to multiple systems without re-authenticating.

**Benefits:** Better user experience, reduced password fatigue, centralized authentication control.
**Risk:** Single point of failure — one compromised credential grants access to all systems.

> **Scenario — SSO Risk: One Credential, All Access:**
> A company deploys SSO. An employee's laptop is compromised by a phishing attack that steals their SSO session cookie. The attacker now has seamless access to: email, ERP system, HR system, payroll, file shares, CRM — everything the employee could access — without needing any individual password. Before SSO, they would have needed to crack each system's password separately. This is why SSO implementations MUST require MFA as the authentication step — one strong factor protecting the single credential protects everything.

### Federation
Extension of SSO across organizational boundaries. Allows users from one organization to access resources in another using their home organization credentials.

### SAML (Security Assertion Markup Language)
**XML-based** standard for exchanging **authentication and authorization** data between Identity Provider (IdP) and Service Provider (SP). SAML is the primary technology enabling **Federated Identity** — allowing a user authenticated at Organization A to access resources at Organization B without a second login.

**SAML Flow:**
```
User → SP: Access resource
SP → User: Redirect to IdP
User → IdP: Authenticate
IdP → User: SAML Assertion (signed XML)
User → SP: Present SAML Assertion
SP → User: Access granted
```

> **Scenario — SAML in Enterprise Federated SSO:**
> A company (Organization A) uses Okta as their Identity Provider. They sign a contract with a SaaS vendor (ServiceNow = Service Provider). A lawyer at Organization A navigates to ServiceNow. ServiceNow doesn't know who she is — it redirects her to Okta (their IdP). She authenticates to Okta (MFA). Okta generates a signed SAML Assertion: "This is Jane Doe, jane@orgA.com, she belongs to role 'Legal-Team'." Jane presents this XML token to ServiceNow. ServiceNow trusts the Okta signature (established in advance via metadata exchange) and grants access without ever receiving Jane's password.
>
> **OAuth 2.0 vs. SAML distinction (most confused pair):**
> - SAML: "Who is this person?" → Enterprise SSO, XML-based
> - OAuth 2.0: "Can this app act on behalf of this user?" → API authorization, token-based
> - Example: Logging into Spotify with your Google account uses **OAuth 2.0** — Spotify never sees your Google password; it gets a token saying "this user authorized Spotify to see their profile." Your Google account is the Authorization Server, Spotify is the Client.
> - If a question describes logging into an enterprise app via your company's Active Directory — think **SAML**. If it describes a mobile app getting permission to access your calendar or contacts — think **OAuth 2.0**.

### OAuth 2.0
Authorization framework — allows third-party apps to access resources **on behalf of users** without sharing passwords. Used for API authorization.

**Key Roles:**
- **Resource Owner** — the user
- **Client** — the third-party app
- **Authorization Server** — issues tokens (IdP)
- **Resource Server** — hosts the protected resource

**Tokens:**
- **Access Token** — grants access to resources (short-lived)
- **Refresh Token** — used to get new access tokens (long-lived)

### OpenID Connect (OIDC)
Authentication layer built on top of OAuth 2.0. Adds **identity** to OAuth's **authorization**. Issues an **ID Token (JWT)** that proves who the user is.

### Comparison

| Protocol | Purpose | Token/Assertion |
|----------|---------|----------------|
| **SAML** | Federated SSO (enterprise, web) | XML Assertion |
| **OAuth 2.0** | API authorization | Access Token |
| **OIDC** | Authentication + authorization | ID Token (JWT) |
| **Kerberos** | Network authentication | Kerberos Tickets |

---

## 5.7 Identity Lifecycle Management

### Account Provisioning
- Automated provisioning via HR system integration (identity governance platforms)
- Follow least privilege principle — grant only what is needed for the role
- Document all access grants

### Transfers and Role Changes
- Promptly remove access from old role
- Grant access for new role (avoid **privilege accumulation** / access creep)

### Account Reviews (Access Recertification)
- Regular reviews (quarterly, annually) to verify access is still appropriate
- Managers certify their direct reports' access
- Removes access creep over time

### Access Creep
Gradual accumulation of access rights beyond what is needed, typically due to job changes without access removal. Mitigated by regular access reviews and SoD enforcement.

> **Scenario — Access Creep in Practice:**
> Dave joined the company in IT Help Desk (needs: password reset tool, ticketing system). After 2 years he moved to Network Engineering (needs: switch/router admin). After 3 years he's now in Cloud Architecture (needs: AWS console, Terraform). Each move added new access — but no one ever removed the old access. Dave now has Help Desk admin, network device admin, and cloud admin simultaneously. He's essentially a super-admin through accumulated permissions, even though no one ever explicitly granted him that level of access. A quarterly access review would have caught and revoked each previous role's permissions at each job change.
>
> **Kerberos attack scenarios:**
> - **Pass the Ticket:** Attacker compromises a machine, dumps the TGT from memory using Mimikatz, and uses it on another machine without knowing the password. The TGT is valid until it expires (default 10 hours). Fix: short ticket lifetimes, monitoring for ticket reuse from different IPs.
> - **Golden Ticket:** Attacker dumps the KRBTGT account's NTLM hash (only possible if domain admin is compromised). They can now forge TGTs for ANY account — even non-existent ones — with ANY privileges, valid for 10 years. This is the "skeleton key" to all Active Directory resources. Fix: rotate KRBTGT account password twice (to invalidate old tickets) immediately after any domain admin compromise.
> - **Kerberoasting:** Attacker requests service tickets for accounts with SPNs (Service Principal Names — SQL Server, Exchange, IIS, etc.). These tickets are encrypted with the service account's password hash. The attacker takes the ticket offline and brute-forces it. Fix: use long, random passwords for service accounts; monitor for unusual service ticket requests.

### Deprovisioning
- Remove all access immediately upon termination
- Disable accounts before deleting (preserve audit trail)
- Revoke certificates, API keys, and physical access badges
- Check for orphaned accounts (accounts with no linked active user)

---

## 5.8 Privileged Access Management (PAM)

PAM specifically addresses the security of administrative and privileged accounts.

| Control | Description |
|---------|-------------|
| **Credential Vaulting** | Store and manage privileged passwords centrally; rotate automatically |
| **Just-in-Time (JIT) Access** | Grant admin rights only when needed; auto-revoke after time limit |
| **Privileged Session Management** | Record, monitor, and control all privileged sessions |
| **Dual Control** | Require two people to access critical systems |
| **Break-glass Accounts** | Emergency accounts with monitored, time-limited use |

### Shared Account Password Management (SAPM)
Manage passwords for shared accounts (root, administrator, service accounts) through a vault that:
- Rotates passwords automatically
- Checks out credentials to authorized users
- Records who used what and when

---

## 5.9 Identity as a Service (IDaaS)

Cloud-based identity and access management:
- **Examples:** Okta, Microsoft Entra ID (Azure AD), Ping Identity
- Provides SSO, MFA, federation, provisioning
- Considerations: data sovereignty, availability dependency on provider

---

## 5.10 Identity Governance and Administration (IGA)

### Separation of Duties in IAM
| Scenario | SoD Control |
|----------|------------|
| Accounts payable | Person who creates payment cannot also approve it |
| System administration | Person who creates accounts cannot also audit them |
| Developer | Cannot push directly to production |

### Segregation of Duties Matrix (Incompatible Roles)
Maintain a documented matrix of role combinations that violate SoD — used in access reviews to detect toxic combinations.

### Account Types and Their Risks
| Account Type | Risk | Control |
|-------------|------|---------|
| **Shared accounts** | No accountability; audit trail broken | Eliminate; use individual accounts with elevation |
| **Service accounts** | Often over-privileged; password rarely changed | PAM vault; regular rotation; monitor activity |
| **Orphaned accounts** | No owner; no monitoring | Regular account reviews; HR-IT termination integration |
| **Privileged accounts** | Highest-risk; target for attackers | MFA; JIT access; session recording |
| **Guest accounts** | Broad access with unknown identities | Time-limited; minimal access; log all activity |

---

## 5.11 Authentication Enhancements

### Password Best Practices (NIST SP 800-63B — Current)
| NIST Recommendation | Old Myth |
|--------------------|----------|
| Focus on **length** (12+ characters) | Force frequent rotation (weakens security) |
| Check against **breached password lists** | Require complex character types |
| Allow **passphrases** | Force arbitrary composition rules |
| **No periodic resets** unless compromise suspected | Mandatory 90-day rotation |
| **No security questions** | Security questions as reset factor |

> **Exam Tip:** NIST SP 800-63B **no longer recommends** mandatory periodic password changes unless compromise is suspected. This is a common exam surprise question.

> **Scenario — Why Forced Rotation Backfires:**
> A company forces password changes every 90 days. Employees, tired of forgetting passwords, start using predictable patterns: "Summer2024!" → "Fall2024!" → "Winter2025!" → "Spring2025!". An attacker who steals the old hash "Summer2024!" can easily guess the pattern. Forced rotation actually made the passwords MORE predictable, not less. NIST's guidance says: use long passphrases (12+ chars), check against breach lists, and only reset if compromise is suspected. Let the password live until it's exposed.

> **Scenario — MAC vs. DAC vs. RBAC — Which One?**
> Three companies, three different access control needs:
>
> **US Intelligence Agency (MAC):** Data is labeled TOP SECRET, SECRET, CONFIDENTIAL. Analyst with SECRET clearance cannot read TOP SECRET files regardless of who gives permission. No individual can override the classification system — it's enforced by the OS/security kernel. Even the VP can't grant a SECRET-cleared analyst access to TOP SECRET data.
>
> **Small startup (DAC):** The founder puts files in a shared folder and says "only my three co-founders can see this." She can change permissions herself at any time — she's the resource owner. Flexible, user-controlled. Fine for small organizations with low risk. If she accidentally shares the folder with everyone, the data is exposed — there's no central enforcement.
>
> **Large enterprise (RBAC):** 5,000 employees. Creating individual permissions for each person would be unmanageable. Instead: "Finance Manager role = access to SAP Finance module, expense approval tool, and monthly reports." New finance manager hired → assign the role → all permissions applied instantly. Employee promoted → remove old role, add new one. Scales perfectly.
>
> **ABAC for modern cloud:** "Allow access IF user.department = 'HR' AND resource.sensitivity = 'HR-only' AND request.time between 08:00-18:00 AND device.managed = true." No single role covers all conditions — ABAC's policy engine evaluates attributes dynamically.

### Passwordless Authentication
- **FIDO2 / WebAuthn** — hardware security keys or biometrics replace passwords entirely
- **Push notifications** — mobile app approves login (Microsoft Authenticator, Duo)
- Eliminates phishing risk of credential theft

### Context-Aware / Adaptive Authentication
Adjusts authentication requirements based on risk signals:
- Login from new country → require MFA
- Login outside business hours → step-up authentication
- Device not enrolled in MDM → deny or quarantine
- Impossible travel (login from two distant locations) → block

---

## 5.12 Federation Deep Dive

### Trust Models in Federation
| Model | Description | Example |
|-------|-------------|---------|
| **Direct Trust** | Two parties establish trust bilaterally | Company A trusts Company B's IdP |
| **Broker Trust** | Central broker mediates trust | Google as IdP for many apps |
| **Web of Trust** | Peer-to-peer trust (PGP model) | PGP key signing |
| **PKI Hierarchy** | Certificate Authority chain | X.509 certificates |

### SAML vs. OAuth 2.0 vs. OIDC — Decision Guide
| Use Case | Best Protocol |
|----------|-------------|
| Enterprise SSO between organizations | SAML |
| App accessing user's data on another service | OAuth 2.0 |
| App needs to know who the user is (identity) | OIDC |
| Mobile app authentication | OIDC |
| Legacy enterprise applications | SAML |

---

## 5.13 Need to Know vs. Least Privilege — Critical Distinction

These two principles are frequently confused on the exam but are fundamentally different:

| Principle | Controls | Applied To | Layer |
|-----------|---------|-----------|-------|
| **Least Privilege** | Minimum system permissions needed to do the job | System rights, roles, access levels | Technical enforcement |
| **Need to Know** | Must have a demonstrated business need for specific data, even within clearance level | Specific data items or files | Policy enforcement |

- **Clearance ≠ Access:** Having TOP SECRET clearance does NOT mean you can access all TOP SECRET documents. You must also satisfy need-to-know for each specific dataset.
- **Least privilege** limits HOW MUCH access (read vs. read/write, one system vs. many).
- **Need to know** limits WHICH SPECIFIC DATA even within an already-privileged scope.

> **Tricky Scenario:** An analyst has SECRET clearance and access to the "Signals Intelligence" compartment. Their friend in the adjacent office is working on a "Human Intelligence" program, also at SECRET level. Can the analyst access the HUMINT files since they're both at SECRET?
> **Answer: No.** Different compartments require separate need-to-know authorization. Clearance level is necessary but insufficient — each compartment/project requires specific authorization. This is why the US intelligence community uses **compartmented information** (SCI — Sensitive Compartmented Information), adding a third layer beyond clearance: access to specific "compartments" or "caveats."

---

## 5.13 Exam Tips Summary

- **MFA** requires factors from **different categories** (not two passwords).
- **CER (Crossover Error Rate)** — lower is better for biometrics.
- **MAC** = labels, central authority, most restrictive.
- **DAC** = owner controls access, most flexible, used in OS file systems.
- **RBAC** = enterprise standard; reduces admin overhead.
- **Kerberos** uses tickets; **Golden Ticket** attack = compromise of KRBTGT hash.
- **TACACS+** = full encryption, TCP, device administration. **RADIUS** = password-only encryption, UDP, network access.
- **SAML** = federated SSO. **OAuth 2.0** = API authorization. **OIDC** = authentication on top of OAuth.
- **Access creep** is mitigated by periodic access reviews/recertification.
- **Orphaned accounts** are accounts with no active linked user — high risk; must be detected and disabled.
- **NIST SP 800-63B** no longer recommends mandatory periodic password rotation.
- **Shared accounts** break accountability and the audit trail — avoid them.
- **FIDO2** = passwordless using hardware tokens or biometrics; eliminates phishing risk.
- **Impossible travel** detection = adaptive/context-aware authentication signal.
- Disable accounts before deleting to preserve forensic audit trail.
