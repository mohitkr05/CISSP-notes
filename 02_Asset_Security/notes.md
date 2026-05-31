# Domain 2 — Asset Security

**Exam Weight: 10%**

---

## 2.1 Data Classification

Classification ensures data is handled appropriately based on its sensitivity and value.

### Government Classification Levels (US)

| Level | Description |
|-------|-------------|
| **Top Secret** | Unauthorized disclosure could cause exceptionally grave damage to national security |
| **Secret** | Unauthorized disclosure could cause serious damage |
| **Confidential** | Unauthorized disclosure could cause damage |
| **Unclassified** | Not sensitive; publicly releasable |

### Commercial Classification Levels (common)

| Level | Description |
|-------|-------------|
| **Confidential / Proprietary** | Trade secrets, competitive information |
| **Private** | Personal employee information, HR data |
| **Sensitive** | Requires special handling (financial, legal) |
| **Public** | No harm if disclosed |

### Classification Criteria
Data classification should be based on:
- **Value** to the organization
- **Age** and relevance (older data may be reclassified downward)
- **Useful life** of the information
- **Personal association** (PII triggers privacy requirements)

> **Exam Tip:** The **data owner** classifies data. The **data custodian** implements the security controls for that classification. These are different roles.

> **Scenario — Owner vs. Custodian vs. User in Practice:**
> A hospital's Chief Medical Officer (CMO) decides that patient diagnosis records are "Confidential/PHI" and that only treating physicians may access them — that's the **data owner** role. The IT team then implements database encryption, access control lists, and audit logging to enforce that policy — that's the **data custodian** role. A nurse who logs in to view a patient's chart before administering medication is the **data user**. If the nurse copies the chart to a USB drive without authorization, she violated the owner's policy — the custodian's controls (USB blocking) should have prevented it.
>
> **Exam trap:** When asked "who is responsible for ensuring patient data is encrypted?" the answer is the **data custodian** (IT implements it), NOT the data owner (who only defines the requirement). When asked "who decides what classification patient data gets?" the answer is the **data owner**.

### Reclassification
Data can be **downgraded** (declassified) when it no longer requires the original protection level. All reclassification must be documented.

---

## 2.2 Data Ownership Roles

| Role | Responsibility | Example |
|------|---------------|---------|
| **Data Owner** | Assigns classification, defines access policy, accountable for data | VP of Finance for financial data |
| **Data Custodian** | Implements controls, backups, encryption, patch management | IT administrator |
| **Data User** | Accesses data according to owner's policy | Employee using the ERP system |
| **Data Steward** | Data quality, governance, metadata management | Data governance team |
| **Data Processor** (GDPR) | Processes personal data on behalf of the controller | SaaS vendor |
| **Data Controller** (GDPR) | Determines purposes and means of processing | The organization itself |
| **System Owner** | Responsible for a system that stores/processes data | Application owner |

---

## 2.3 Data States

Data exists in three states, each requiring different security controls:

### Data at Rest
Data stored on physical media (disk, tape, database).

**Controls:**
- **Full Disk Encryption (FDE)** — BitLocker, FileVault
- **Database encryption** — Transparent Data Encryption (TDE)
- **File/folder encryption** — EFS, AES-256
- Physical security of media

### Data in Transit
Data moving across a network.

**Controls:**
- **TLS 1.2/1.3** — HTTPS, SMTPS, IMAPS
- **VPN (IPSec, SSL VPN)**
- **SFTP / SCP** — secure file transfer
- Avoid unencrypted protocols (FTP, Telnet, HTTP)

### Data in Use
Data actively being processed in memory/CPU.

**Controls:**
- **Memory encryption** — AMD SEV, Intel TDX
- **Secure enclaves** — Intel SGX
- **DLP (Data Loss Prevention)** tools
- Screen locks and session timeouts

> **Exam Tip:** Data in use is the hardest state to protect. Controls here are limited and emerging. Expect scenario questions about DLP and memory protection.

> **Scenario — The Three Data States:**
> A healthcare analyst receives an encrypted email attachment (data in transit — TLS protects it). She saves it to her encrypted laptop drive (data at rest — FDE protects it). She opens the file in Excel and starts editing patient data (data in use — the data is now decrypted in RAM, visible on her screen). At this moment, a screenshot malware on her laptop can capture the data. Data at rest and in transit are well-protected; data in use is the exposure window.
>
> **Why this matters for the exam:** Questions about "an employee opening a file" or "data being processed" are pointing to **data in use** vulnerabilities. The correct controls to mention are DLP tools, screen privacy filters, session timeouts, and memory encryption (SGX/SEV). You cannot encrypt data in RAM with a file encryption tool.

---

## 2.4 Data Handling and Lifecycle

### Data Lifecycle Stages
```
Create → Store → Use → Share → Archive → Destroy
```

### Marking and Labeling
- **Marking** — human-readable labels (e.g., "CONFIDENTIAL" header/footer)
- **Labeling** — machine-readable tags for automated classification/DLP

### Handling Procedures by Classification
Higher classification = more restrictive handling:
- Transmission restrictions (encrypted channels only)
- Storage requirements (encrypted, access-controlled)
- Printing controls (secure print, shredding)
- Sharing restrictions (NDA required)

### Data Retention
- Retention policies define how long data must be kept (legal, operational, regulatory requirements)
- GDPR: storage limitation principle — no longer than necessary
- Legal holds: preserve data relevant to litigation beyond normal retention

---

## 2.5 Data Privacy

### Key Privacy Concepts

| Concept | Definition |
|---------|-----------|
| **PII** (Personally Identifiable Information) | Information that can identify an individual (name, SSN, email) |
| **PHI** (Protected Health Information) | Health data protected under HIPAA |
| **SPI** (Sensitive Personal Information) | Subset of PII requiring extra protection (biometrics, race, religion) |

### GDPR Core Principles (7)
1. **Lawfulness, fairness, transparency**
2. **Purpose limitation** — collected for specified, explicit purposes
3. **Data minimization** — only collect what is necessary
4. **Accuracy** — keep data accurate and up to date
5. **Storage limitation** — no longer than necessary
6. **Integrity and confidentiality** — appropriate security
7. **Accountability** — controller is responsible for compliance

### GDPR Data Subject Rights
- Right to **access** their data
- Right to **rectification** (correction)
- Right to **erasure** ("right to be forgotten")
- Right to **restrict processing**
- Right to **data portability**
- Right to **object** to processing
- Rights related to **automated decision-making**

### Privacy by Design (Ann Cavoukian — 7 Principles)
1. Proactive not reactive; preventative not remedial
2. Privacy as the default setting
3. Privacy embedded into design
4. Full functionality — positive-sum, not zero-sum
5. End-to-end security — full lifecycle protection
6. Visibility and transparency
7. Respect for user privacy

> **Exam Tip:** Privacy by Design means building privacy in from the start, not adding it as an afterthought.

---

## 2.6 Asset Management

### Hardware Asset Lifecycle
```
Procurement → Deployment → Operation → Maintenance → End-of-Life → Disposal
```

Key controls:
- Maintain an inventory (CMDB — Configuration Management Database)
- Tag assets (barcodes, RFID)
- Track location and custodian
- License compliance (software)

### Software Asset Management
- **License tracking** — avoid compliance violations (BSA audits)
- **Vulnerability management** — patch unmanaged software
- **CMDB** — central repository for all IT assets and relationships

### Cloud Assets
- Shadow IT risk — employees using unauthorized cloud services
- Cloud security posture management (CSPM)
- Data residency requirements (where data is stored)

---

## 2.7 Secure Disposal and Media Sanitization

Reference: **NIST SP 800-88 — Guidelines for Media Sanitization**

### Sanitization Methods (increasing effectiveness)

| Method | Description | Use Case |
|--------|-------------|---------|
| **Clear** | Overwrite with non-sensitive data; defeated by advanced forensics | Low-sensitivity media reuse |
| **Purge** | Degaussing, cryptographic erase, secure overwrite | Moderate-sensitivity; media reuse possible |
| **Destroy** | Physical destruction (shred, incinerate, disintegrate) | High-sensitivity; no reuse |

### Specific Techniques

| Technique | Description | Media Type |
|-----------|-------------|-----------|
| **Degaussing** | Magnetic field erases data | Magnetic media (HDD, tape) |
| **Overwriting** | Write patterns over data (e.g., DoD 5220.22-M) | HDDs (not SSDs) |
| **Shredding** | Physical destruction | Paper, optical media, SSDs |
| **Incineration** | Burning | Any media |
| **Crypto-shredding** | Destroy encryption key (data useless without key) | Encrypted storage (cloud, SSDs) |

> **Exam Tip:** **Crypto-shredding** is the preferred method for cloud storage and SSDs where physical destruction is impractical. Destroying the key renders all encrypted data unreadable.

> **Exam Tip:** **Degaussing does NOT work on SSDs or optical media** — it only affects magnetic storage.

> **Scenario — Choosing the Right Sanitization Method:**
> A company is disposing of three types of media:
> 1. **Old magnetic tape backups** containing patient records: Use **Degaussing** (destroys the magnetic field) then **physical shredding** for assurance. Overwriting is also acceptable but degaussing is faster for tapes.
> 2. **SSDs from decommissioned laptops** with confidential sales data: Cannot degauss (no magnetic medium). Use **Crypto-shredding** (if the drive was encrypted — destroy the key, data becomes useless) or **physical shredding** if re-use is not needed.
> 3. **Cloud S3 buckets** containing PHI that are being decommissioned: Use **Crypto-shredding** — you can't physically destroy the hardware, but if all data was encrypted with a key you control, deleting the key sanitizes the data effectively.
>
> **The exam trap:** A question mentions decommissioning SSDs and someone proposes "overwrite with DoD 5220.22-M 7-pass wipe." This is WRONG for SSDs — the wear-leveling algorithms in SSDs don't guarantee all blocks are overwritten. The correct answer is **crypto-shredding** or **physical destruction**.

### Certificate of Destruction
Document confirming proper disposal — required for compliance (HIPAA, PCI-DSS, etc.). Third-party destruction vendors should provide this.

---

## 2.8 Data Protection Techniques

### Tokenization vs. Encryption vs. Masking

| Technique | How It Works | Reversible? | Use Case |
|-----------|-------------|------------|---------|
| **Encryption** | Transforms data using a key | Yes (with key) | Protecting data in transit/rest |
| **Tokenization** | Replaces sensitive value with a non-sensitive token | Only via token vault | PCI-DSS (credit card numbers) |
| **Data Masking** | Obscures real data with fake data | No (original data preserved separately) | Dev/test environments |
| **Anonymization** | Irreversibly removes identity linkage | No | Analytics, research |
| **Pseudonymization** | Replaces identity with pseudonym; key held separately | Yes (with key) | GDPR-compliant processing |

> **Exam Tip:** **Tokenization** is preferred for PCI-DSS because tokens are valueless outside the vault — even if stolen, no PAN data is exposed. **Pseudonymization** does not satisfy GDPR anonymization — the data is still personal data because it can be re-identified.

> **Scenario — Tokenization vs. Encryption vs. Masking (the most-tested distinction):**
>
> **Tokenization in PCI-DSS:** A customer pays online. Their real credit card number (4111-1111-1111-1111) is captured, sent to a token vault, and replaced with a random token (8675-3090-XXXX-1234). The e-commerce app stores and processes only the token. If the e-commerce database is breached, attackers get tokens that are worthless — they can't be reversed without the vault.
>
> **Encryption in transit:** That same credit card number is sent over HTTPS (TLS). The number is mathematically transformed with a key. If someone intercepts the ciphertext and gets the key, they can recover the card number. Encryption IS reversible with the key — tokenization has no mathematical relationship to the original.
>
> **Data Masking in dev/test:** A developer needs real-looking test data. The real DB value "4111-1111-1111-1111" becomes "XXXX-XXXX-XXXX-1111" in the test database. The original value is preserved in production — masking just hides it for non-production use. Not reversible from the masked copy alone.
>
> **Pseudonymization (GDPR context):** A hospital replaces patient name "John Smith" with Patient-ID "P-88742" in research datasets. The mapping table (P-88742 = John Smith) is locked away. Researchers work with P-88742. Because re-identification IS possible (if you get the mapping table), GDPR still treats this as personal data. It reduces risk but doesn't eliminate GDPR obligations.
>
> **Anonymization:** Truly removes all identity linkage — no mapping table exists. GDPR no longer applies because it can never be re-identified. Much harder to achieve in practice.

### DRM (Digital Rights Management)
- Persistent controls embedded in files — enforce usage restrictions after distribution
- Controls: view-only, print-disable, copy-disable, expiry dates
- Used for: documents, media, software licensing
- Enterprise version: IRM (Information Rights Management) — Microsoft AIP/Purview

---

## 2.9 GDPR — Exam-Critical Details

### Breach Notification Requirements
| Requirement | Detail |
|-------------|--------|
| **Notify supervisory authority** | Within **72 hours** of becoming aware of breach |
| **Notify data subjects** | If breach is likely to result in high risk to their rights |
| **Processor → Controller** | Processor must notify controller **without undue delay** |

### Legal Bases for Processing Personal Data (GDPR Article 6)
1. **Consent** — explicit, informed, freely given
2. **Contract** — processing necessary to fulfill a contract
3. **Legal obligation** — required by law
4. **Vital interests** — life or death situation
5. **Public task** — official government function
6. **Legitimate interests** — balanced against individual rights

> **Exam Tip:** Consent under GDPR must be **freely given, specific, informed, and unambiguous**. Pre-ticked boxes are NOT valid consent.

> **Scenario — GDPR Breach Notification Chain:**
> A US-based SaaS company (Data Processor) stores EU citizen data for a French company (Data Controller). The SaaS company suffers a breach at 9 AM Monday. Here's what must happen:
> 1. **SaaS company → French company (Controller):** Must notify the Controller "without undue delay" — practically as soon as they confirm it's a breach, even if details are incomplete.
> 2. **French company → French Data Protection Authority (CNIL):** Must notify within **72 hours** of the Controller becoming aware. If the SaaS notifies at 9 AM Monday, the French company's 72-hour clock starts then — they must notify CNIL by 9 AM Thursday.
> 3. **French company → Affected EU citizens:** Only required if the breach is "likely to result in a high risk to the rights and freedoms" of individuals (e.g., exposed passwords, financial data). Not always required.
>
> **Exam trap:** The 72-hour clock runs from when the **Controller** becomes aware, not from when the breach occurred. If the breach happened 5 days ago but the Controller only found out today, the 72-hour window starts today.
>
> **GDPR vs. HIPAA notification trap:** GDPR = 72 hours to the supervisory authority. HIPAA = 60 days to individuals, 60 days to HHS. HIPAA is more generous on time but still mandatory. These numbers come up in every exam.

### GDPR vs. HIPAA Comparison
| Feature | GDPR | HIPAA |
|---------|------|-------|
| Jurisdiction | EU/EEA (and global for EU citizens) | USA |
| Scope | All personal data | Protected Health Information (PHI) |
| Breach notification | 72 hours to authority | 60 days to individuals; 60 days to HHS |
| Right to erasure | Yes | Limited |
| Penalties | Up to 4% global revenue or €20M | Up to $1.9M/year per category |

---

## 2.10 Data Collection Limitation

### Scoping Principles
| Principle | Description |
|-----------|-------------|
| **Data minimization** | Collect only what is strictly necessary for the stated purpose |
| **Purpose limitation** | Use data only for the purpose it was collected |
| **Consent management** | Obtain, record, and honor user consent preferences |
| **Children's data** | Extra protections required (COPPA: under 13, GDPR: under 16 in most EU states) |

---

## 2.11 Exam Tips Summary

- The **data owner** makes classification decisions; the **custodian** implements them.
- **Data in use** is the most difficult state to protect.
- **Crypto-shredding** is best for cloud and SSD disposal.
- **GDPR** applies to data of EU citizens globally — it is jurisdiction-independent.
- **Data minimization** = collect only what you need.
- Higher classification = more restrictive handling at every stage of the lifecycle.
- When in doubt on disposal: if the data is highly sensitive, choose **physical destruction**.
- **Tokenization** ≠ encryption: tokens have no mathematical relationship to original data.
- **Pseudonymization** ≠ anonymization under GDPR — pseudonymized data is still personal data.
- GDPR breach notification: **72 hours** to the supervisory authority.
- **Data Controller** = decides why and how data is processed. **Data Processor** = processes on controller's behalf. The controller bears primary responsibility.
