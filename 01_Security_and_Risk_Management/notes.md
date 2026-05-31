# Domain 1 — Security and Risk Management

**Exam Weight: 16%** — Highest-weighted domain. Foundational for all other domains.

---

## 1.1 CIA Triad

The three core pillars of information security:

| Pillar | Definition | Controls |
|--------|-----------|---------|
| **Confidentiality** | Preventing unauthorized disclosure of information | Encryption, access controls, classification |
| **Integrity** | Ensuring data is accurate and unaltered | Hashing (MD5, SHA), digital signatures, version control |
| **Availability** | Ensuring systems and data are accessible when needed | Redundancy, RAID, clustering, DRP |

### Beyond the Triad
- **Authenticity** — data is genuine and from the claimed source
- **Non-repudiation** — sender cannot deny sending; achieved via digital signatures
- **Accountability** — actions traced to an individual (audit logs)

> **Exam Tip:** When a question asks about "proving someone sent a message," the answer is **non-repudiation** (digital signatures). When it asks about "who performed an action," the answer is **accountability** (audit logs).

> **Scenario — CIA in Practice:**
> A hospital stores patient records. A nurse reads a record without permission (Confidentiality violation). A billing clerk changes a patient's diagnosis code to increase reimbursement (Integrity violation). Ransomware encrypts the records and staff can't access them (Availability violation). Each violation maps to a different pillar — and a different set of controls is needed.

> **Tricky Distinction:** Availability vs. Integrity — if a backup is corrupted and restores wrong data, that is an **Integrity** failure even though the system is "available." Always ask: is the data accessible (Availability) or is it accurate (Integrity)?

---

## 1.2 Security Governance

### Key Roles

| Role | Responsibility |
|------|----------------|
| **Board of Directors** | Ultimate accountability for security |
| **Senior Management** | Sets policy; owns residual risk |
| **CISO** | Manages security program |
| **Security Steering Committee** | Cross-functional oversight body |
| **Data Owner** | Business unit; assigns classification |
| **Data Custodian** | IT; implements controls set by owner |
| **Data User** | Accesses data per policy |

### Due Care vs. Due Diligence
- **Due Care** — doing what a reasonable person would do; ongoing action ("doing the right thing")
- **Due Diligence** — researching and understanding risks before acting ("knowing the right thing")

> **Mnemonic:** Due **C**are = **C**ontinuous action. Due **D**iligence = **D**iscovery/research first.

> **Scenario — Due Care vs. Due Diligence:**
> A company is evaluating a new cloud vendor. **Due diligence** = reviewing the vendor's SOC 2 report, checking their breach history, and assessing their security policies *before* signing the contract. **Due care** = after signing, continuously monitoring the vendor, running quarterly reviews, and ensuring SLAs are met. If the vendor has a breach and you never reviewed their controls, you failed due diligence. If you reviewed but never followed up, you failed due care.
>
> **Exam trap:** A question might describe a manager who signed a contract with a vendor without checking their security posture — that is a **due diligence** failure, not due care.

### Security Policies Hierarchy
```
Policies (mandatory, high-level)
  └─ Standards (mandatory, specific metrics)
       └─ Guidelines (recommended, flexible)
            └─ Procedures (step-by-step instructions)
```

### Governance Frameworks

| Framework | Focus |
|-----------|-------|
| **COBIT** | IT governance and management |
| **ITIL** | IT service management |
| **ISO/IEC 27001** | ISMS (Information Security Management System) |
| **ISO/IEC 27002** | Code of practice for security controls |
| **NIST CSF** | 5 functions: Identify, Protect, Detect, Respond, Recover |
| **COSO** | Internal control / enterprise risk management |

---

## 1.3 Risk Management

### Risk Concepts

| Term | Definition |
|------|-----------|
| **Threat** | Potential cause of an undesired incident |
| **Vulnerability** | Weakness that can be exploited |
| **Risk** | Probability × Impact (Threat × Vulnerability) |
| **Exposure** | Being subject to loss |
| **Control/Safeguard** | Measure to reduce risk |
| **Residual Risk** | Risk remaining after controls applied |
| **Total Risk** | Risk without any controls |

**Formula:** `Total Risk = Threats × Vulnerabilities × Asset Value`
**Formula:** `Residual Risk = Total Risk − Countermeasure Effect`

> **Scenario — Risk Concepts in One Story:**
> A company's web server (Asset Value = $500K) has an unpatched Apache vulnerability (Vulnerability). A criminal gang is actively exploiting Apache vulnerabilities (Threat). The combination = Risk. They install a WAF (Control/Safeguard) which reduces the probability of exploitation. The risk that remains after the WAF is deployed = Residual Risk. The board formally accepts that residual risk in writing — that is **risk acceptance**.

### Quantitative Risk Analysis

| Term | Formula |
|------|---------|
| **AV** (Asset Value) | Monetary value of asset |
| **EF** (Exposure Factor) | % of asset lost in an incident (0–1) |
| **SLE** (Single Loss Expectancy) | `AV × EF` |
| **ARO** (Annualized Rate of Occurrence) | Expected frequency per year |
| **ALE** (Annualized Loss Expectancy) | `SLE × ARO` |
| **Safeguard Value** | `ALE(before) − ALE(after) − Annual Cost of Safeguard` |

**Example:**
- Asset Value = $200,000
- EF = 25% → SLE = $50,000
- ARO = 0.5 (once every 2 years)
- ALE = $25,000

> **Scenario — Should You Buy the Safeguard?**
> A fire suppression system costs $8,000/year. Without it:
> - Server room AV = $400,000, EF = 50% → SLE = $200,000, ARO = 0.1 → ALE(before) = $20,000
> With it: risk drops, ALE(after) = $2,000
> Safeguard Value = $20,000 − $2,000 − $8,000 = **$10,000 net benefit** → Buy it.
> If the system cost $25,000/year, the Safeguard Value would be negative — it costs more than the risk it prevents → Don't buy it.
>
> **Exam trap:** The formula subtracts the *annual cost* of the safeguard, not a one-time cost. Always use the annual figure.

### Qualitative Risk Analysis
Uses subjective ratings (High/Medium/Low) and scenario-based analysis. Methods:
- **Delphi Technique** — anonymous expert consensus
- **Brainstorming**
- **Questionnaires**
- **Risk Matrix** (Likelihood × Impact grid)

### Risk Response Strategies

| Strategy | Description | Example |
|----------|-------------|---------|
| **Accept** | Acknowledge and do nothing | Low-risk, low-impact items |
| **Avoid** | Eliminate the activity | Stop offering a risky service |
| **Transfer** | Shift risk to third party | Cyber insurance |
| **Mitigate (Reduce)** | Implement controls | Firewalls, patching |

> **Scenario — Choosing the Right Strategy:**
> A company runs a legacy payroll application that can't be patched (EoL vendor). Consider each option:
> - **Accept**: Management signs off — "we'll run it as-is and accept the risk." Fine for very low-impact/low-probability scenarios.
> - **Avoid**: Shut down the payroll application entirely. Not realistic — payroll must run.
> - **Transfer**: Buy cyber insurance to cover breach costs from this app. Risk of exploitation stays, but financial impact shifts.
> - **Mitigate**: Put the legacy app on an isolated VLAN, add a WAF in front, and monitor it heavily.
>
> **Exam trap:** Insurance (Transfer) does NOT eliminate risk — the vulnerability still exists. Risk Transfer moves the financial consequence, not the threat or vulnerability itself.

### Risk Frameworks

| Framework | Description |
|-----------|-------------|
| **NIST RMF (SP 800-37)** | 6-step: Categorize, Select, Implement, Assess, Authorize, Monitor |
| **NIST SP 800-30** | Risk assessment guide |
| **ISO 31000** | Enterprise risk management standard |
| **OCTAVE** | Operationally Critical Threat, Asset, Vulnerability Evaluation |
| **FAIR** | Factor Analysis of Information Risk — quantitative |
| **TRIKE** | Threat modeling methodology |

---

## 1.4 Compliance and Legal

### Key Regulations

| Regulation | Jurisdiction | Scope |
|-----------|-------------|-------|
| **GDPR** | EU | Personal data of EU citizens; 72-hr breach notification |
| **HIPAA** | USA | Healthcare data (PHI) |
| **PCI-DSS** | Global | Payment card data |
| **SOX** | USA | Financial reporting integrity |
| **GLBA** | USA | Financial institutions |
| **FERPA** | USA | Student education records |
| **COPPA** | USA | Children's online privacy (<13) |

### Computer Crime Laws (USA)

| Law | Scope |
|-----|-------|
| **CFAA (Computer Fraud and Abuse Act)** | Unauthorized computer access |
| **ECPA** | Electronic communications privacy |
| **Identity Theft Enforcement Act** | Identity theft |
| **Wiretap Act** | Interception of communications |

### Intellectual Property

| Type | Duration | Protection |
|------|----------|------------|
| **Copyright** | Life + 70 years | Original creative works |
| **Trademark** | Renewable indefinitely | Brand identifiers |
| **Patent** | 20 years | Inventions |
| **Trade Secret** | Indefinite (as long as secret) | Confidential business info |

> **Exam Tip:** Trade secrets have no registration and no expiration — but they lose protection if disclosed.

### Types of Laws
- **Criminal law** — prosecution by government; intent required
- **Civil law** — disputes between parties; no intent required
- **Administrative law** — regulatory compliance; agencies enforce
- **Tort law** — civil wrong causing harm; negligence claims

---

## 1.5 Personnel Security

### Hiring Controls
- **Background checks** — criminal, credit, reference
- **Employment agreement** — job responsibilities, acceptable use
- **NDA (Non-Disclosure Agreement)** — protects confidential information
- **Non-compete clause** — restricts employment with competitors

### Ongoing Controls

| Control | Purpose |
|---------|---------|
| **Separation of Duties** | No single person controls entire process; prevents fraud |
| **Least Privilege** | Only access needed to do the job |
| **Need to Know** | Access restricted to information required for role |
| **Mandatory Vacations** | Detects ongoing fraud (someone else covers the job) |
| **Job Rotation** | Cross-trains staff; exposes fraud over time |
| **Dual Control** | Two people required to perform a single action |
| **Two-Person Integrity (TPI)** | Two people must be present at all times |

> **Scenario — Why Mandatory Vacations Catch Fraud:**
> An accountant has been slightly inflating vendor invoices and pocketing the difference for 2 years. Because *he* processes and approves his own payments (SoD violation), no one noticed. When he takes mandatory vacation, a colleague covers his work, finds the discrepancies, and reports them. The fraud is discovered. This is why mandatory vacations are a *detective* control — they force a break in a fraudulent pattern that depends on one person always being present.
>
> **Tricky Distinction — Least Privilege vs. Need to Know:**
> Both restrict access, but they apply differently. A security guard has *least privilege* — only the minimum system rights to do their job (e.g., can badge into the building but not the server room). A cleared analyst with TOP SECRET clearance still uses *need to know* — even if they have the clearance level, they can only see the specific project files they're assigned to. Clearance is a ceiling; need to know is the actual door you're allowed through.

> **Scenario — Dual Control vs. Two-Person Integrity (TPI):**
> **Dual Control** example: Launching a nuclear missile requires two officers to turn their keys simultaneously. Neither can do it alone — both must actively participate in the *action*.
> **TPI** example: A guard escorts a technician into a secure vault. The guard doesn't need to *do* anything — they just must be *present* at all times. If the technician is alone in the vault for 30 seconds, TPI is violated, even if nothing bad happened.

### Termination Controls
- Immediate access revocation upon departure
- Return of company assets
- Exit interview
- Escorted off premises (hostile termination)

### Security Awareness Training
- **Awareness** — recognize security issues (posters, videos)
- **Training** — skill building for specific tasks
- **Education** — in-depth understanding (certifications, degrees)

> **Exam Tip:** Social engineering is a primary threat mitigated by user awareness training. Phishing simulations are the most effective training method.

---

## 1.6 Business Continuity Planning (BCP)

### BCP vs. DRP

| Plan | Scope | Focus |
|------|-------|-------|
| **BCP** | Organization-wide | Maintaining business functions during disruption |
| **DRP** | IT/Systems focused | Restoring IT systems after a disaster |

> BCP is the umbrella; DRP is a subset of BCP.

### BCP Process (4 Steps)
1. **Project Scope and Planning** — initiation, team, charter
2. **Business Impact Analysis (BIA)** — identify critical functions, dependencies, financial impact
3. **Continuity Planning** — develop strategies; alternate processing
4. **Approval and Implementation** — senior management sign-off, training, testing

### Business Impact Analysis (BIA) Key Terms

| Term | Definition |
|------|-----------|
| **MTD / MAD** | Maximum Tolerable Downtime — longest tolerable outage |
| **RTO** | Recovery Time Objective — target time to restore system |
| **RPO** | Recovery Point Objective — maximum acceptable data loss (in time) |
| **WRT** | Work Recovery Time — time to verify system integrity after recovery |
| **MTTR** | Mean Time to Repair |
| **MTBF** | Mean Time Between Failures |

**Relationship:** `RTO + WRT ≤ MTD`

> **Scenario — RTO, RPO, MTD in Action:**
> An e-commerce site has these BCP targets:
> - **MTD = 8 hours** — the business cannot survive more than 8 hours of downtime before revenue and reputation damage is unacceptable.
> - **RTO = 4 hours** — IT must restore the systems within 4 hours of a disaster declaration.
> - **WRT = 2 hours** — after systems are restored, the team needs 2 hours to verify data integrity, test the checkout flow, and confirm operations are normal.
> - RTO (4h) + WRT (2h) = 6 hours ≤ MTD (8h) → This plan is valid.
>
> - **RPO = 1 hour** — the company backs up every hour. If a disaster hits at 2:55 PM, the backup from 2:00 PM is the last clean copy. They can lose up to 55 minutes of orders — that's acceptable under the 1-hour RPO.
>
> **Exam trap:** If someone asks "how much data can we lose?" → answer is **RPO**. If they ask "how long can we be down?" → **MTD/MAD**. If they ask "how fast must IT restore?" → **RTO**.

### Alternate Site Types

| Site | Description | Cost | RTO |
|------|-------------|------|-----|
| **Hot Site** | Fully equipped, real-time data replication | Highest | Minutes–Hours |
| **Warm Site** | Partially equipped, needs data restoration | Medium | Hours–Days |
| **Cold Site** | Empty facility, must install everything | Lowest | Days–Weeks |
| **Mobile Site** | Portable/trailer-based | Varies | Varies |
| **Reciprocal Agreement** | Mutual aid between organizations | Low/Free | Varies |
| **Cloud** | On-demand provisioning | Pay-per-use | Minutes–Hours |

### BCP/DRP Testing Types (in order of thoroughness)

| Test | Description | Disruption |
|------|-------------|------------|
| **Read-through / Checklist** | Review plan for accuracy | None |
| **Tabletop Exercise** | Walk through scenarios verbally | None |
| **Walkthrough / Structured Walk-through** | Team walks through procedures | None |
| **Simulation** | Realistic scenario practice | Minimal |
| **Parallel Test** | Run alternate site alongside primary | Low |
| **Full Interruption** | Shut down primary, activate alternate | High |

> **Scenario — Which DR Test to Choose:**
> A hospital needs to test its DR plan but cannot afford any patient care disruption. The safest test is a **Tabletop Exercise** — key staff gather in a conference room, and the facilitator says "a ransomware attack has encrypted all systems." Each person talks through their role: who calls whom, what systems get isolated, when the hot site is activated. No actual systems are touched. It exposes gaps in the plan without any risk.
>
> When the hospital is ready for a more rigorous test, they run a **Parallel Test** — the hot site is fully activated and processing synthetic transactions alongside the live primary site. If the hot site works correctly, the plan is validated. No patients are affected because the primary site stays live.
>
> **Full Interruption** would only be done during a scheduled maintenance window — primary systems are actually shut down to prove the hot site can carry the full load. Highest value, highest risk.

---

## 1.7 Ethics

### (ISC)² Code of Ethics

**Canons (in priority order):**
1. Protect society, the common good, necessary public trust and confidence, and the infrastructure.
2. Act honorably, honestly, justly, responsibly, and legally.
3. Provide diligent and competent service to principals.
4. Advance and protect the profession.

> **Exam Tip:** If canons conflict, always prioritize protecting **society** over protecting the organization or client.

> **Scenario — Ethics Canon Conflict:**
> You are a CISSP working for a defense contractor. During a routine audit you discover that your company's software has a vulnerability that could allow a nation-state to take down power grid systems. Your manager tells you to keep it quiet until the next release cycle (3 months away) to avoid bad PR. What do you do?
> Canon 1 (protect society) outranks Canon 3 (serve your principal/employer). You are ethically required to escalate — notify the appropriate authorities or the software vendor — even against your employer's wishes. Protecting society always wins when canons conflict.
>
> **Another classic trap:** A client asks you to perform a penetration test and "look the other way" if you find evidence of fraud against their customers. Canon 2 (act honorably and legally) means you cannot participate in covering up harm to others. Your obligation to society and the law overrides your obligation to this specific client.

### Computer Ethics Institute — Ten Commandments of Computer Ethics
Key prohibitions include: unauthorized access, using computers to harm others, interfering with others' work, stealing, using/copying software without authorization.

---

## 1.8 Threat Modeling and Risk Concepts

### Threat Categories
| Category | Examples |
|----------|---------|
| **Natural threats** | Floods, earthquakes, hurricanes, fires |
| **Human threats — unintentional** | Employee errors, misconfiguration, accidents |
| **Human threats — intentional** | Hackers, insiders, nation-states, terrorists |
| **Technical threats** | Hardware failure, software bugs, power outages |

### Risk Visibility Terms
| Term | Meaning |
|------|---------|
| **Known risk** | Identified and planned for |
| **Unknown risk** | Not yet identified (residual/emergent) |
| **Risk appetite** | Amount of risk an organization is willing to accept |
| **Risk tolerance** | Acceptable variation around risk appetite |
| **Risk threshold** | Level at which risk becomes unacceptable |

### Control Types (Classification by Goal)
| Type | Purpose | Examples |
|------|---------|---------|
| **Preventive** | Stop incidents before they happen | Firewalls, access control, encryption |
| **Detective** | Identify incidents that have occurred | IDS, audit logs, cameras |
| **Corrective** | Fix damage after an incident | Backups, incident response, patches |
| **Deterrent** | Discourage potential attackers | Warning banners, guards, fences |
| **Compensating** | Alternative control when primary is infeasible | MFA when SSO is unavailable |
| **Recovery** | Restore to normal operations | DRP, BCP, hot site |
| **Directive** | Direct/guide behavior | Policies, procedures, awareness training |

> **Exam Tip:** A single control can be multiple types simultaneously. A camera is both detective AND deterrent.

> **Scenario — Control Type Ambiguity (the hardest exam questions):**
> A bank has a policy requiring all employees to take 2 weeks of consecutive vacation per year. What type of control is this?
> - It is **detective** — forces someone else to cover the job and exposes fraud.
> - It is also **preventive** — employees who know someone will cover their work are less likely to commit fraud in the first place.
> - The *primary* exam answer is **detective** in most question contexts.
>
> A login warning banner says "Unauthorized access is prohibited and will be prosecuted." What type?
> - **Deterrent** (discourages attackers) and **Directive** (guides/directs behavior with a policy statement). Not preventive — it doesn't stop the login attempt.
>
> **The Corrective vs. Recovery distinction:** Applying a patch after a breach = **Corrective** (fixes the vulnerability that caused the incident). Restoring from backup after ransomware = **Recovery** (restores operations to normal). Both happen post-incident but serve different purposes.

### Control Implementation Types
| Type | Description | Example |
|------|-------------|---------|
| **Administrative** | Policies, procedures, training | Security policy, background checks |
| **Technical (Logical)** | Software/hardware controls | Firewall, encryption, IDS |
| **Physical** | Physical barriers and mechanisms | Locks, fences, guards |

---

## 1.9 Security Policy Framework — Deeper Detail

### Types of Policies
| Policy Type | Description | Example |
|-------------|-------------|---------|
| **Regulatory** | Required by law/regulation | HIPAA-compliant privacy policy |
| **Advisory** | Strongly recommended | Password complexity advisory |
| **Informative** | Educational, no enforcement | General awareness policy |

### Acceptable Use Policy (AUP)
- Defines permitted and forbidden uses of company systems
- Users must sign before access is granted
- Covers internet usage, email, social media, personal use
- Critical for establishing **authorization** baseline

---

## 1.10 Risk Management — Advanced Concepts

### Threat Intelligence Integration
- **Strategic TI** — high-level trends for executive decisions
- **Tactical TI** — TTPs to guide defensive operations
- **Operational TI** — specific active campaign details
- **Technical TI** — IOCs (IPs, hashes, domains) for SIEM rules

### Risk Register
A formal document tracking:
- Risk ID and description
- Likelihood and impact ratings
- Risk owner
- Mitigation actions and status
- Residual risk after controls

### Security Metrics that Demonstrate Value
| Metric | Demonstrates |
|--------|-------------|
| **Reduction in ALE** | Financial value of security controls |
| **MTTD/MTTR** | Detection and response effectiveness |
| **Patch compliance rate** | Vulnerability exposure reduction |
| **Training completion** | Security culture development |

---

## 1.11 Business Impact Analysis — Deeper

### BIA Process Steps
1. Identify critical business functions
2. Identify resources needed for each function
3. Identify interdependencies
4. Determine MTD/RTO/RPO for each function
5. Identify financial and operational impact of outage
6. Prioritize recovery order

### Recovery Priority Ordering
Recovery should be in order of business criticality:
1. Life safety systems
2. Critical infrastructure (power, networking)
3. Highest-revenue or legally-required systems
4. Core business applications
5. Supporting systems

### BCP vs. COOP vs. Crisis Management
| Plan | Scope |
|------|-------|
| **BCP** | Maintain business during disruption |
| **COOP (Continuity of Operations)** | Government — maintain essential functions |
| **Crisis Management Plan** | Human safety and communications during disaster |
| **DRP** | IT/technology recovery |

---

## 1.12 Key Mnemonics

| Mnemonic | Remembers |
|---------|-----------|
| **CIA** | Confidentiality, Integrity, Availability |
| **"All Risk Must Transfer"** | Accept, Risk-Avoid, Mitigate, Transfer |
| **"Prudent Man Rule"** | Due care standard |
| **DAD** | Disclosure, Alteration, Destruction (CIA attacks) |
| **PAPA** | Privacy, Accuracy, Property, Accessibility (Mason's ethics) |
| **PPT** | People, Process, Technology (balanced security) |
| **DIRT** | Directive, Investigative (detective), Recovery, Termination (preventive) |

---

## 1.13 Exam Tips Summary

- CISSP is a **management** exam — think policy/governance first, technical second.
- Senior management is **always** ultimately responsible for security.
- Risk **acceptance** requires formal sign-off from management.
- **Qualitative** = subjective, uses scenarios; **Quantitative** = numbers, formulas.
- **BIA** comes before DRP/BCP strategy development.
- The most important BCP document to test is the **notification checklist**.
- When a regulation and company policy conflict, **follow the more restrictive** one.
- **Control types**: Preventive → Detective → Corrective is the standard defense-in-depth order.
- Administrative, Technical, Physical: every security program needs all three layers.
- **Risk appetite** is set by senior management — security team does not set it unilaterally.
- NIST RMF steps: **Categorize → Select → Implement → Assess → Authorize → Monitor** (CSIAAM).
- The CISSP exam frequently uses **"MOST important"** — think management/policy before technology.
