# Domain 6 — Security Assessment and Testing

**Exam Weight: 12%**

---

## 6.1 Security Assessment Approaches

### Vulnerability Assessment
Identifies, quantifies, and prioritizes weaknesses in systems. Less invasive than penetration testing — finds vulnerabilities but does not exploit them.

**Process:**
```
Asset Discovery → Scanning → Analysis → Prioritization → Reporting → Remediation → Verification
```

**Types:**
- **Authenticated scan** — scanner has credentials; deeper visibility; fewer false positives
- **Unauthenticated scan** — no credentials; simulates external attacker perspective; more false positives
- **Agent-based** — installed on host; always-on, continuous visibility
- **Network-based** — remote scanner; discovers network-visible vulnerabilities

**Common Tools:** Nessus, Qualys, OpenVAS, Rapid7 InsightVM

### Penetration Testing
Simulates a real attack to discover how far an attacker can penetrate. Goes beyond finding vulnerabilities to actually exploiting them to demonstrate impact.

#### Knowledge Levels

| Type | Tester Knowledge | Perspective |
|------|----------------|-------------|
| **Black Box** | No prior knowledge | External attacker |
| **White Box** | Full knowledge (source code, architecture) | Internal developer/auditor |
| **Gray Box** | Partial knowledge (credentials, architecture docs) | Privileged insider or partner |

#### Penetration Testing Phases

| Phase | Activities |
|-------|-----------|
| **Planning / Rules of Engagement** | Define scope, targets, methods, legal authorization |
| **Reconnaissance** | OSINT, passive information gathering |
| **Scanning / Enumeration** | Port scans, service detection, vulnerability scans |
| **Exploitation** | Attempt to exploit discovered vulnerabilities |
| **Post-Exploitation** | Lateral movement, privilege escalation, persistence |
| **Reporting** | Document findings, evidence, risk ratings, remediation |

> **Exam Tip:** A penetration test **must always have written authorization** from the system owner before it begins. Unauthorized testing is illegal — even if well-intentioned.

> **Scenario — Pentest Authorization Trap (classic exam scenario):**
> A security consultant is hired by Company A to test their network. While testing, they discover that the network connects to Company B's systems (a partner organization). The consultant finds a critical vulnerability in Company B's server through the Company A connection. Should they exploit it to demonstrate the full attack chain?
>
> **NO.** The authorization only covers Company A's systems. Company B was never part of the scope. Accessing Company B's systems — even to demonstrate a real attack path — is unauthorized access under the CFAA. The correct action: stop at the boundary, document the finding, notify the client that additional authorization from Company B is needed before proceeding.
>
> **Black/White/Gray Box selection:**
> - If you want to simulate an **external attacker** with no insider knowledge → **Black box**
> - If you want to find **as many vulnerabilities as possible** (code audit + infrastructure test) → **White box**
> - If you want to simulate a **compromised insider** or a **partner with some access** → **Gray box** (most realistic for modern threat models — assumes attackers who have already established a foothold)

### Red Team / Blue Team / Purple Team

| Team | Role |
|------|------|
| **Red Team** | Simulates attackers; offensive operations; finds security gaps |
| **Blue Team** | Defends; detects and responds to attacks; SOC operations |
| **Purple Team** | Collaborative exercise; red and blue work together to improve both offense and defense |

---

## 6.2 Software Security Testing

### Static vs. Dynamic vs. Interactive

| Type | When | How | Advantage |
|------|------|-----|----------|
| **SAST** (Static) | Development/build | Analyzes source code | Early detection; no running app needed |
| **DAST** (Dynamic) | Testing/staging | Tests running app | Finds runtime issues; no source needed |
| **IAST** (Interactive) | Testing/production | Instrumented runtime | Most accurate; combines both |
| **SCA** (Composition) | Build | Analyzes dependencies | Finds known CVEs in libraries |
| **Fuzz testing** | Testing | Random/malformed input | Finds unexpected input handling |

> **Scenario — SAST vs. DAST: What Each Finds and Misses:**
> A developer writes code with a SQL injection vulnerability: `query = "SELECT * FROM users WHERE id = " + userInput`. 
>
> **SAST** scans the source code at commit time — finds this string concatenation pattern immediately, flags it as a potential injection point, and fails the CI build. No need to deploy the app. However, SAST can't tell you if a JWT validation issue only triggers under specific runtime conditions.
>
> **DAST** tests the running app by sending `' OR '1'='1` to every input field. If the app returns database errors or unexpected results, DAST flags it. But DAST doesn't have the source code — it can't tell you which line of code caused it.
>
> **The practical combination:** SAST catches obvious code patterns early; DAST catches what actually manifests in the running app; IAST instruments the running app and traces exactly which lines execute during a DAST test — combining the visibility of both.
>
> **SCA specific scenario:** A developer pulls in the open-source `log4j` library version 2.14.1 (the one with the infamous Log4Shell RCE vulnerability). SCA tools (like Snyk or OWASP Dependency-Check) scan the pom.xml/package.json and immediately flag: "log4j 2.14.1 has CRITICAL CVE-2021-44228 — upgrade to 2.17.1." This happens before the app is even deployed.

### Code Review
- **Manual code review** — highest quality; finds logic flaws; slow
- **Automated code review (SAST)** — fast; finds known patterns; high false positive rate
- **Pair programming** — two developers; implicit security review

### Interface Testing
Tests the interfaces between system components:
- **API testing** — validate inputs, outputs, error handling, authentication
- **UI testing** — validate input sanitization, output encoding
- **Network interface testing** — validate protocol implementations

---

## 6.3 Audit Types and Compliance

### Internal Audit
- Conducted by the organization's own audit team
- Objective: verify compliance with internal policies
- Less independent than external; more frequent
- Findings go to internal management

### External Audit
- Conducted by independent third party
- Objective: independent verification for regulatory compliance or customer assurance
- More credible to external stakeholders

### Third-Party Audit
- Assessing a vendor or supplier's security controls
- Supports supply chain risk management
- Right-to-audit clauses in contracts enable this

---

## 6.4 SOC Reports (Service Organization Controls)

SOC reports are independent auditor assessments of service organization controls.

### SOC 1
- Scope: **Financial reporting controls** (relevant to financial auditors)
- Audience: User entity auditors
- Examples: Payroll processors, bank transaction processors

### SOC 2
- Scope: **Security, availability, processing integrity, confidentiality, privacy** (Trust Services Criteria)
- Audience: Customers, business partners (typically restricted)
- Most relevant for cloud providers, SaaS companies

#### SOC 2 Type I vs. Type II

| Type | Description | Covers |
|------|-------------|--------|
| **Type I** | Point-in-time assessment ("paperwork") | Are controls **designed** appropriately? |
| **Type II** | Period of time assessment (6+ months) | Are controls **operating effectively** over time? |

> **Exam Tip:** **Type II is stronger** than Type I — it proves controls actually work over a sustained period, not just on a single audit day. When evaluating vendor security, always prefer **Type II** reports.
>
> **Key Distinction Memory Hook:**
> - **Type I** = "design review" = a snapshot in time = a photo of the controls on audit day
> - **Type II** = "operating effectiveness" = a video of the controls over 6+ months = far more reliable
>
> A vendor who only provides Type I has only proven they *had* the controls on one day. Type II proves the controls consistently operated throughout the period.

> **Scenario — SOC Report Selection in Vendor Due Diligence:**
> Your company is evaluating a cloud payroll provider. The vendor offers you a SOC 2 Type I report from last month.
>
> **Should you accept this?** Type I only proves the controls were *designed* appropriately on one day in October. It doesn't prove they've been operating consistently. A company could put perfect controls in place for the audit day and then disable monitoring for the rest of the year. **Ask for a SOC 2 Type II** covering at least 6 months — that proves the controls were operating effectively throughout the period.
>
> **SOC 1 vs. SOC 2 trap:** The vendor is a SaaS company, not a payroll processor. A customer asks for a report about how the vendor handles security. They should provide **SOC 2** (covers security, availability, confidentiality). If they only have a SOC 1, that report covers financial reporting controls — irrelevant to security assurance.
>
> **SOC 3** = same audit as SOC 2 but formatted to be publicly shared (e.g., posted on the company's website as a "trust seal"). It lacks the detailed findings of SOC 2 — it's a marketing-grade assurance document, not a technical audit report. Never substitute a SOC 3 for due diligence purposes.

### SOC 3
- Same criteria as SOC 2 but **publicly available** (can be shared on website)
- Less detail than SOC 2

---

## 6.5 Security Metrics and KPIs

### Key Metrics to Track

| Metric | Description |
|--------|-------------|
| **MTTD** (Mean Time to Detect) | Average time from breach to detection |
| **MTTR** (Mean Time to Respond/Recover) | Average time to respond to/recover from an incident |
| **Vulnerability Density** | Number of vulnerabilities per 1000 lines of code |
| **Patch Compliance Rate** | % of systems patched within policy window |
| **False Positive Rate** | % of alerts that are not real incidents |
| **Coverage Metrics** | % of assets scanned, % of code covered by SAST |
| **Training Completion Rate** | % of employees completing security training |
| **Phishing Simulation Click Rate** | % of employees who click phishing simulation emails |

### KPI vs. KRI

| Type | Definition | Example |
|------|-----------|---------|
| **KPI** (Key Performance Indicator) | Measures effectiveness of security controls | Patch compliance rate: 95% |
| **KRI** (Key Risk Indicator) | Early warning indicator of increasing risk | # of unpatched critical CVEs: 12 |

> **Scenario — KPI vs. KRI in a Board Presentation:**
> A CISO presents to the board:
>
> **KPIs** (how well are our controls working?):
> - Patch compliance rate: 97% of systems patched within SLA ✓
> - Mean Time to Detect (MTTD): 4 hours average (down from 12 hours last quarter) ✓
> - Phishing simulation click rate: 3% (down from 11% last quarter) ✓
>
> **KRIs** (what's rising on the horizon?):
> - Number of internet-facing systems with CVSS 9.0+ vulnerabilities: 14 (up from 3 last month) — flag this
> - Third-party vendors without SOC 2 reports: 8 (increased due to new contracts) — flag this
> - Privileged accounts without MFA: 5 (all new service accounts) — flag this
>
> KPIs tell you how well you're doing **today**. KRIs tell you where your exposure is **growing** — they are early warning indicators that something bad may happen if not addressed. Both are needed for a complete security posture picture.

---

## 6.6 Log Review and Audit Trails

### What to Log

| Event Category | Examples |
|---------------|---------|
| Authentication events | Logins, logouts, failed attempts, lockouts |
| Privileged account activity | Admin logins, sudo/RunAs, config changes |
| Account management | Account creation, deletion, password resets |
| Object access | File access, database queries, email access |
| Network activity | Firewall blocks, DNS queries, proxy logs |
| System events | Service starts/stops, reboots, crashes |

### Audit Trail Requirements
- **Integrity** — logs must not be modifiable by subjects they audit
- **Completeness** — all relevant events captured
- **Availability** — accessible for review and investigation
- **Retention** — kept for required period (regulatory, legal)
- **Timestamps** — accurate NTP synchronization essential

### Log Analysis Techniques
- **Clipping levels** — baseline normal behavior; alert on deviations
- **Keystroke monitoring** — capture and log user keystrokes (requires legal/policy authorization)
- **Synthetic transactions** — simulate user activity to test and monitor system availability and performance

---

## 6.7 Synthetic Transactions

Automated scripts that simulate real user transactions to:
- Verify system availability
- Detect performance degradation
- Validate that critical business transactions work correctly
- Provide continuous baseline for anomaly detection

Example: A script that logs in, places a $1 test order, and cancels it every 5 minutes to verify the e-commerce system is operational.

---

## 6.8 Risk Assessment Process

```
Asset Identification → Threat Identification → Vulnerability Identification →
Likelihood Assessment → Impact Assessment → Risk Calculation → Mitigation Planning
```

### Privacy Impact Assessment (PIA)
Required before deploying systems that collect or process personal data:
- Identifies privacy risks in new or changed systems
- Required by GDPR (Data Protection Impact Assessment — DPIA)
- Documents controls to mitigate privacy risks

### Security Impact Analysis
Evaluates how a proposed change to a system affects its security posture:
- Part of the change management process
- Required before deploying patches or configuration changes
- Determines if a change requires a new security assessment

---

## 6.9 Compliance-Based Testing

### Specific Regulatory Testing Requirements
| Regulation | Testing Requirement |
|-----------|-------------------|
| **PCI-DSS** | Quarterly vulnerability scans (ASV); annual penetration test |
| **HIPAA** | Periodic technical and non-technical evaluations |
| **SOX** | Annual controls testing by external auditor |
| **GDPR** | DPIA before high-risk processing; no mandatory pen test |
| **NIST RMF** | Assessment of controls at each authorization step |
| **ISO 27001** | Regular internal and external audits against the standard |

### Vulnerability Scanner Limitations
- Scanners report **potential** vulnerabilities based on version/signatures
- **False positives** — scanner flags an issue that doesn't actually exist
- **False negatives** — scanner misses a real vulnerability
- Authenticated scans reduce both — more visibility, fewer assumptions
- Scanners cannot assess business logic flaws — manual pen test required

> **Scenario — False Positive vs. False Negative Consequences:**
> A scanner reports that a web server is running Apache 2.2.0 (vulnerable to a known RCE exploit). Your team spends 2 days investigating — turns out the vendor applied a backported patch and the version string is misleading. The server was never actually vulnerable. This is a **false positive** — wasted analyst time is the cost.
>
> A scanner completely misses a custom admin portal running on port 8443 that has no authentication. It wasn't in the scanner's standard port range and had no version banner to fingerprint. An attacker finds it within hours of a breach. This is a **false negative** — the real cost: a missed critical vulnerability.
>
> **Why authenticated scans matter:** An unauthenticated scanner sees your web server from the outside — like looking at a building's facade. It can see what ports are open but can't see inside. An authenticated scanner logs in and inspects installed software, patch levels, configuration files, and running services from inside — like a building inspector with a key. Far more accurate, far fewer false positives and negatives.
>
> **Business logic flaws — why scanners miss them:** A scanner can find SQL injection because it's a known pattern. But if your e-commerce site lets you change the price of an item in the checkout POST request (IDOR/parameter tampering), no scanner will detect this — it requires a human tester who understands what the application is *supposed* to do and then tests whether the application *enforces* it.

---

## 6.10 Assessment Report Structure

### Penetration Test Report Components
1. **Executive Summary** — business risk language; no technical jargon; for CISO/CEO
2. **Scope and Methodology** — what was tested, how, rules of engagement
3. **Findings** — each vulnerability with: description, evidence, CVSS score, risk rating
4. **Remediation Recommendations** — specific, actionable
5. **Attack Narrative** — story of how the test progressed
6. **Appendices** — raw tool output, screenshots, proof-of-concept code

### CVSS Scoring Components
| Metric Group | Components |
|-------------|-----------|
| **Base** | Attack vector, attack complexity, privileges required, user interaction, confidentiality/integrity/availability impact |
| **Temporal** | Exploit code maturity, remediation level, report confidence |
| **Environmental** | Organization-specific modifiers |

---

## 6.11 Output Interpretation — What Results Mean

### Vulnerability Assessment Findings
| Finding | Action |
|---------|--------|
| Critical (9.0-10.0) | Patch immediately; consider emergency change |
| High (7.0-8.9) | Patch within 7-14 days |
| Medium (4.0-6.9) | Patch within 30-90 days |
| Low (0.1-3.9) | Best effort patching |
| Informational | No action required; awareness only |

### Penetration Test Result Interpretation
- A clean pen test does NOT mean no vulnerabilities — it means the tester found no exploitable path within scope/time constraints
- **Scope creep** — expanding test beyond authorized boundaries is unauthorized access
- **Re-testing** — verify remediation fixed the finding

---

## 6.12 Threat Modeling Methodologies — Assessment Context

Threat modeling belongs in the **design phase** of the SDLC — finding threats before code is written. Key frameworks:

| Framework | Developer | Focus | Best For |
|-----------|----------|-------|---------|
| **STRIDE** | Microsoft | 6 threat categories mapped to CIA+AN | Application design |
| **PASTA** | vSpRINT | 7-stage risk-centric process | Enterprise risk-aligned dev |
| **DREAD** | Microsoft | Quantitative risk scoring (1-10) | Prioritizing findings |
| **TRIKE** | Community | Threat-risk-centric; uses requirement models | Defensive requirements |
| **VAST** | ThreatModeler | Visual, Agile, and scalable | DevSecOps pipelines |
| **Attack Trees** | Schneier | Hierarchical goal-based attack decomposition | Complex system analysis |

> **Exam rule:** When a question mentions threat modeling as an organizational process for a new enterprise system → **SABSA** (architecture-level). When it's specifically about application threat modeling in the SDLC → **STRIDE or PASTA**. STRIDE is the most tested.

### Penetration Test Scope Types

| Scope | Description | Exam Use |
|-------|-------------|---------|
| **Full-scope** | All systems, all methods, no restrictions | Most comprehensive |
| **Partial/targeted** | Only specific systems or application layers | Common for compliance |
| **Network pen test** | Infrastructure, firewalls, routing | External/internal network |
| **Web application pen test** | OWASP Top 10 focus | App-specific |
| **Social engineering pen test** | Phishing, vishing, physical | Human factor testing |
| **Red team engagement** | Simulated adversary; includes physical + cyber | Full attack chain realism |

> **Tricky Scenario:** A company passes their annual PCI-DSS penetration test with zero critical findings. The CISO declares the environment "secure." Is this correct?
> **Answer: No.** A penetration test only covers what was in scope, using the techniques the tester employed, in the time allotted. Zero findings means "no exploitable paths were found within these constraints" — not "the environment is secure." Additionally, PCI-DSS requires an ASV scan (quarterly) AND an annual pen test — passing one doesn't validate everything. New vulnerabilities emerge daily.

---

## 6.12 Exam Tips Summary

- **Penetration testing requires written authorization** — always, no exceptions.
- **Black box** = no knowledge. **White box** = full knowledge. **Gray box** = partial.
- **SOC 2 Type II** is stronger than Type I (period of time vs. point in time).
- **DAST** finds runtime issues without source code. **SAST** finds source code issues without running the app.
- **MTTD** and **MTTR** are the key metrics for measuring incident detection and response performance.
- **KPI** measures performance. **KRI** is an early warning indicator of risk.
- Log integrity is critical — logs must be protected from modification by audited subjects.
- **PIA/DPIA** is required by GDPR before processing personal data in new systems.
- **Authenticated scans** give more complete and accurate results than unauthenticated.
- **Red Team** = attack. **Blue Team** = defend. **Purple Team** = both collaborate.
- **PCI-DSS** requires quarterly ASV scans AND annual penetration testing.
- **CVSS** base score assesses severity; environmental score adjusts for your specific context.
- A clean vulnerability scan ≠ secure. Scanners miss business logic flaws and zero-days.
- **SOC 1** = financial controls. **SOC 2** = security/availability/etc. **SOC 3** = publicly shareable summary.
