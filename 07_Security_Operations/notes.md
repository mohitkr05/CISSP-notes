# Domain 7 — Security Operations

**Exam Weight: 13%**

---

## 7.1 Incident Response

### NIST SP 800-61 Incident Response Lifecycle

```
Preparation → Identification → Containment → Eradication → Recovery → Lessons Learned
```

| Phase | Key Activities |
|-------|---------------|
| **Preparation** | Develop IR plan, train team, deploy tools (SIEM, IDS), establish CIRT |
| **Identification** | Detect and confirm incident; classify severity; notify stakeholders |
| **Containment** | Short-term (isolate affected systems) and long-term (patch, harden) |
| **Eradication** | Remove malware, close vulnerabilities, reset compromised credentials |
| **Recovery** | Restore systems from clean backups; verify functionality; monitor closely |
| **Lessons Learned** | Post-incident review (PIR); update plans; document improvements |

### Incident Classification

| Category | Description | Examples |
|----------|-------------|---------|
| **Denial of Service** | Service availability disruption | DDoS, resource exhaustion |
| **Malicious Code** | Malware execution | Ransomware, trojans, rootkits |
| **Unauthorized Access** | Access without authorization | Privilege escalation, account compromise |
| **Inappropriate Usage** | Policy violations | Employee misuse, data exfiltration |
| **Scans/Probes** | Reconnaissance | Port scans, vulnerability scanning |

### CIRT / CSIRT (Computer Security Incident Response Team)
Roles: incident handler, forensic analyst, legal liaison, communications lead, management.

> **Exam Tip:** The first priority in incident response is always **containment** — stopping the spread — not evidence collection. However, if forensic investigation is required, **preservation of evidence** may come before eradication.

> **Scenario — IR Phase Sequencing (the tricky exam question):**
> At 2 AM, the SOC detects ransomware actively encrypting files across 50 servers. The CISO calls you. What's the priority order?
>
> 1. **Identification** — is this a real incident or a false positive? (It's real — files are encrypting)
> 2. **Containment** — isolate affected systems immediately. Disconnect from the network. Stop the spread before it hits more servers. This is FIRST.
> 3. **Evidence Preservation** — BUT: if litigation or law enforcement involvement is likely, take RAM images and disk snapshots BEFORE you shut anything down. Shutting down destroys volatile evidence.
> 4. **Eradication** — remove the ransomware, identify the entry point (phishing email, unpatched RDP?)
> 5. **Recovery** — restore from the last clean backup, verify integrity, bring systems back online
> 6. **Lessons Learned** — post-incident review within 2 weeks. What failed? What worked?
>
> **The containment vs. evidence tension:** The exam will present scenarios where you must choose. If the question says "law enforcement is involved" or "litigation is expected" → lean toward evidence preservation. If the question says "ransomware is spreading" or "systems are being destroyed in real time" → containment first, collect evidence from what you can save.

> **Scenario — Identification Phase in Practice:**
> A Tier 1 SOC analyst sees an alert: "User downloaded 4.7 GB to a USB drive at 11:55 PM." Is this an incident?
> - Identification phase: investigate before declaring an incident. Check: is this user's role consistent with bulk downloads? Is this their normal behavior? Was it authorized (approved backup? business travel?). Call the user's manager.
> - If the user is a terminated employee whose account wasn't disabled → **Unauthorized Access** incident — escalate immediately.
> - If it's a developer who regularly archives code to external drives per approved policy → **False positive / non-incident**. Document and close.
> The phase name "Identification" means: confirm it IS an incident, NOT just that you detected an anomaly.

---

## 7.2 Digital Forensics

### Forensic Process (RFC 3227 / NIST)
```
Identification → Preservation → Collection → Examination → Analysis → Presentation
```

### Order of Volatility (collect most volatile first)
1. **CPU registers and cache** — lost immediately on power loss
2. **RAM (main memory)** — lost on power off; contains running processes, encryption keys
3. **Network state** — active connections, ARP tables
4. **Running processes** — process list, open files, sockets
5. **Disk storage** — persistent but can be overwritten
6. **Remote logging / monitoring data** — external systems
7. **Archival media** — backup tapes, cold storage

> **Exam Tip:** Always capture **RAM first** before imaging the disk. RAM contains decrypted keys, running processes, and volatile evidence that disappears on power-off.

> **Scenario — Order of Volatility in a Ransomware Investigation:**
> Investigators arrive at a server that is actively running ransomware. The clock is ticking — evidence is disappearing in real time and the malware is still running. What do they collect and in what order?
>
> 1. **CPU registers/cache** — already gone by the time you get there (microseconds)
> 2. **RAM** — use a tool like WinPmem or Volatility to dump all physical memory RIGHT NOW. This contains: the running ransomware process, the encryption keys currently in use (could decrypt the files!), network connection tables, recently typed passwords
> 3. **Network state** — `netstat -an` to capture active connections (the C2 server IP!), ARP tables, routing tables
> 4. **Running processes** — `tasklist /v`, open files, loaded DLLs
> 5. **Disk image** — forensic bit-for-bit copy with a write blocker (slower but preserves everything on disk including deleted files and slack space)
> 6. **Log files** — already on disk, less urgent
>
> **The encryption key in RAM:** Ransomware must keep the encryption key in RAM while it's actively encrypting files. If you image RAM during an active encryption, forensic analysts may be able to extract the key and decrypt the victim files WITHOUT paying the ransom. This has worked in real cases (CryptoLocker recovery). Powering off the machine first destroys this opportunity permanently.

### Evidence Types

| Type | Description |
|------|-------------|
| **Real evidence** | Physical objects (drives, devices) |
| **Documentary evidence** | Documents, logs, records |
| **Testimonial evidence** | Witness statements |
| **Direct evidence** | Directly proves a fact |
| **Circumstantial evidence** | Infers a fact from related evidence |
| **Hearsay evidence** | Second-hand; generally not admissible |
| **Best evidence rule** | Original documents preferred over copies |

### Chain of Custody
Documented record of who handled evidence, when, and what was done. Critical for legal admissibility. Every transfer must be logged.

### Forensic Principles
- **Locard's Exchange Principle** — every contact leaves a trace
- **Write blockers** — prevent modification of evidence during collection
- **Hash verification** — MD5/SHA-256 before and after collection to prove integrity
- **Bit-for-bit image** — forensic copy includes deleted files and slack space

---

## 7.3 Logging and Monitoring

### SIEM (Security Information and Event Management)
Aggregates and correlates logs from multiple sources to detect threats in real time.

**SIEM capabilities:**
- Log aggregation and normalization
- Correlation rules and alerting
- Real-time dashboards
- Long-term log retention
- Compliance reporting (PCI-DSS, HIPAA)

### Log Management Best Practices
- **Centralized logging** — ship logs to central SIEM; local logs can be tampered
- **Log integrity** — WORM storage; timestamps with NTP synchronization
- **Retention** — retain logs per regulatory requirements (HIPAA: 6 years; PCI-DSS: 1 year minimum)
- **Review** — automated correlation rules; regular manual review

### IDS vs. IPS

| Feature | IDS | IPS |
|---------|-----|-----|
| **Action** | Detects and alerts | Detects and blocks |
| **Placement** | Out-of-band (tap) | Inline (in traffic path) |
| **Risk** | Misses attacks | False positives can block legitimate traffic |

### IDS Detection Methods

| Method | How It Works | Strength | Weakness |
|--------|-------------|---------|---------|
| **Signature-based** | Match known attack patterns | Low false positives | Misses zero-days; needs updates |
| **Anomaly-based** | Detects deviation from baseline | Detects unknown attacks | High false positives |
| **Heuristic** | Rule-based behavior analysis | Balanced | Can miss novel attacks |

> **Scenario — IDS Detection Methods: Which One Catches What?**
>
> **Signature-based:** An attacker runs a known Metasploit exploit (EternalBlue / MS17-010). The IDS has the signature for this exact exploit in its database. Alert fires immediately, correctly. 0 false positives because it's a known match. A NEW, unknown exploit? The IDS is silent — it doesn't know what to look for.
>
> **Anomaly-based:** The baseline says this server receives 50 connections/hour. An attacker starts a port scan — suddenly 10,000 connection attempts per minute. The IDS has no signature for this particular scan tool but flags it as "significant deviation from baseline." Alert fires. Now a legitimate load test is also flagged because it looks like a DDoS to the anomaly engine — that's the false positive problem.
>
> **The IPS inline risk:** An IPS sits in the traffic path and can DROP packets. This is powerful but dangerous. A misconfigured IPS signature can block legitimate HTTP traffic and take down a website. IDS just alerts — worst case you miss something. IPS blocks — worst case you cause an outage. This is why IPS rules are tuned carefully before being set to "block" mode.
>
> **HIDS vs. NIDS scenario:** A malicious insider installs a keylogger directly on a workstation. There's no unusual network traffic (the keylogger logs locally and uploads in small encrypted bursts). **NIDS** misses it completely — nothing unusual in the network traffic. **HIDS** (monitoring process creation, file system changes, registry modifications on the endpoint) detects the keylogger process and its attempts to persist via registry run keys. Endpoint-level threats require endpoint-level detection.

**HIDS** (Host IDS) — monitors individual system activity (file changes, registry, processes).
**NIDS** (Network IDS) — monitors network traffic at a tap or span port.

### Threat Intelligence

| Term | Description |
|------|-------------|
| **IOC** | Indicator of Compromise (IP, hash, domain) |
| **TTPs** | Tactics, Techniques, Procedures (adversary behavior) |
| **STIX** | Structured Threat Information Expression (format) |
| **TAXII** | Trusted Automated Exchange of Intelligence Information (protocol) |
| **MITRE ATT&CK** | Framework of known adversary tactics and techniques |

---

## 7.4 Change Management

### Change Control Process
```
Request → Review → Approval → Test → Implement → Document → Review
```

| Step | Description |
|------|-------------|
| **RFC** (Request for Change) | Formal request documenting the change |
| **CAB** (Change Advisory Board) | Reviews and approves changes |
| **Emergency Change** | Fast-tracked for critical security patches |
| **Rollback Plan** | Must be defined before any change is approved |

### Configuration Management
- **CMDB** — tracks all IT assets, their configurations, and relationships
- **Baseline configuration** — approved starting state for all systems
- **Configuration drift** — deviation from baseline (detected by continuous monitoring)

### Patch Management Process
```
Inventory → Scan → Prioritize → Test → Deploy → Verify → Report
```

- **Critical patches** — deploy within 24-48 hours
- **High patches** — deploy within 7-30 days
- **Test** all patches in a non-production environment first

---

## 7.5 Disaster Recovery

### Backup Types

| Type | What is Backed Up | Restore Complexity | Speed |
|------|------------------|-------------------|-------|
| **Full** | All data | Simple (one backup set) | Slowest |
| **Incremental** | Changes since last backup of any type | Complex (full + all incrementals) | Fastest |
| **Differential** | Changes since last full backup | Moderate (full + latest differential) | Medium |

> **Exam Tip:**
> - **Incremental** backup = fastest backup, slowest restore (need all incrementals)
> - **Differential** backup = slower backup than incremental, faster restore (only need full + latest diff)

> **Scenario — Backup Strategy Comparison (very commonly tested):**
> Full backup happens Sunday night. Then:
>
> **Incremental strategy:**
> - Mon backup: changes since Sunday (small, fast)
> - Tue backup: changes since Monday (small, fast)
> - Wed backup: changes since Tuesday (small, fast)
> - Disaster happens Thursday: restore Sunday full + Monday + Tuesday + Wednesday incrementals = 4 backup sets needed, applied in sequence. Slow restore.
>
> **Differential strategy:**
> - Mon backup: changes since Sunday (medium)
> - Tue backup: changes since Sunday (larger — includes Monday's changes too)
> - Wed backup: changes since Sunday (even larger — includes Mon + Tue changes)
> - Disaster happens Thursday: restore Sunday full + Wednesday differential only = 2 backup sets. Fast restore.
>
> **Which to choose?**
> - Limited backup window, can afford slower restore? → Incremental (daily backup is tiny and fast)
> - Tight RTO, can afford slower backup and more storage? → Differential (restore in 2 steps)
> - Best of both worlds, don't care about backup time? → Full daily (most expensive in storage and time, but single-set restore)
>
> **3-2-1 Rule in practice:** A law firm keeps 3 copies of client files: the live file server (Copy 1), an on-site NAS backup (Copy 2, different media), and a nightly encrypted cloud backup to Azure (Copy 3, offsite). Ransomware encrypts the server and the NAS (both on the same network). The cloud backup is untouched — they restore from Copy 3. Air-gapping goes further: the backup is physically disconnected from the network, making ransomware propagation impossible.

### Backup Storage

| Type | Description |
|------|-------------|
| **Onsite** | Fast recovery; vulnerable to same disaster |
| **Offsite** | Slower recovery; protected from site disaster |
| **3-2-1 Rule** | 3 copies, 2 different media types, 1 offsite |
| **Air-gapped** | Offline/disconnected; protects against ransomware |

### Recovery Objectives
- **RTO** — maximum time to restore system
- **RPO** — maximum acceptable data loss (in time)
- **MTD/MAD** — maximum tolerable downtime
- **WRT** — work recovery time (post-restore verification)

### DR Testing (see also Domain 1)
Tabletop → Walkthrough → Simulation → Parallel → Full Interruption

---

## 7.6 Vulnerability Management

### Process
```
Asset Discovery → Vulnerability Scanning → Risk Assessment → Prioritization → Remediation → Verification
```

### CVSS (Common Vulnerability Scoring System)
- Score 0–10; Base, Temporal, Environmental metrics
- Critical: 9.0–10.0 | High: 7.0–8.9 | Medium: 4.0–6.9 | Low: 0.1–3.9

### Patch Prioritization Factors
- CVSS score
- Asset criticality
- Exploitability (is exploit in the wild?)
- Business impact
- Compensating controls in place

### Zero-Day Response
A zero-day is a vulnerability with no available patch. Mitigations:
- **Network segmentation** — limit lateral movement
- **WAF / IPS rules** — virtual patching
- **Disable affected features** if possible
- **Increase monitoring** for exploitation indicators

---

## 7.7 Data Loss Prevention (DLP)

DLP systems monitor and control movement of sensitive data.

| Type | Scope |
|------|-------|
| **Endpoint DLP** | Controls data on devices (USB, print, copy-paste) |
| **Network DLP** | Inspects data leaving via network (email, web uploads) |
| **Cloud DLP** | Monitors data in cloud storage and SaaS applications |
| **Storage DLP** | Scans data at rest for misclassified/exposed sensitive data |

**DLP Actions:** Alert, Block, Quarantine, Encrypt, Log

---

## 7.8 Third-Party and Supply Chain Security

### Vendor Risk Management

| Control | Purpose |
|---------|---------|
| **Vendor risk assessment** | Evaluate security posture before onboarding |
| **SLAs** | Define security requirements and commitments |
| **Right to audit** | Contractual right to audit vendor security |
| **SOC 2 Type II reports** | Independent auditor's report on vendor controls |
| **Penetration test results** | Verify vendor's security posture |

### Supply Chain Attacks
- Hardware tampering (counterfeit components)
- Software supply chain compromise (malicious packages, build system compromise)
- Managed service provider (MSP) compromise (e.g., SolarWinds)

**Mitigations:**
- Trusted supplier lists
- Hardware verification (TPM attestation)
- Software bill of materials (SBOM)
- Code signing verification

> **Scenario — SolarWinds: A Supply Chain Attack Case Study:**
> In 2020, attackers (later attributed to SVR, Russian intelligence) compromised the SolarWinds build server. They injected malicious code into the legitimate Orion software update package. When SolarWinds pushed the update to 18,000+ customers (including US government agencies), every customer installed trojanized software signed with SolarWinds' legitimate certificate.
>
> Why it worked:
> - The update was signed by SolarWinds — certificate validation passed
> - The malware was dormant for 12-14 days (evaded sandboxes that test for immediate malicious behavior)
> - Traffic to the C2 domain looked like normal Orion traffic
>
> Mitigation lessons:
> - **SBOM:** If customers had inventories of every library/component in Orion, they could have cross-referenced against the known-good build
> - **Build environment isolation:** The build server should have been air-gapped and monitored for unauthorized changes
> - **Code signing ≠ secure code:** Signature only proves who signed it, not that the content is safe
> - **Anomaly detection:** The malware's C2 beacon used subdomain patterns (avsvmcloud.com) that unusual DNS monitoring could have flagged

---

## 7.9 Privileged Account Management (PAM)

Privileged accounts (admin, root, service accounts) are primary targets.

| Control | Description |
|---------|-------------|
| **Just-in-time (JIT) access** | Grant elevated access only when needed, for limited time |
| **Session recording** | Record all privileged sessions for audit |
| **Password vaulting** | Store and rotate privileged credentials automatically |
| **Least privilege** | Minimum permissions for administrative tasks |
| **MFA for privileged access** | Required for all admin accounts |
| **Service account rotation** | Regular rotation of service account credentials |

---

## 7.10 Security Operations Center (SOC)

### SOC Tiers
| Tier | Responsibility |
|------|--------------|
| **Tier 1** | Alert triage; first look; escalates unusual findings |
| **Tier 2** | Incident investigation; deep analysis; threat hunting |
| **Tier 3** | Advanced analysis; malware reverse engineering; threat hunting |
| **Management** | Metrics, reporting, staffing, tool management |

### SOC Operating Models
| Model | Description | Pros/Cons |
|-------|-------------|-----------|
| **In-house SOC** | Organization owns and staffs entirely | Full control; high cost |
| **MSSP (Managed Security Services Provider)** | Third party monitors 24/7 | Cost-effective; less contextual knowledge |
| **Hybrid SOC** | In-house + MSSP for coverage gaps | Balance of control and cost |
| **Virtual SOC** | No physical facility; staff work remotely | Flexible; coordination challenges |

---

## 7.11 Incident Response — Advanced

### Evidence Handling — Legal Admissibility Requirements
For evidence to be admissible in court it must be:
1. **Authentic** — provably collected from the actual source
2. **Accurate** — hash verified; not tampered with
3. **Complete** — full picture, not cherry-picked
4. **Convincing** — jury/judge can understand it
5. **Admissible** — obtained legally; no illegal search/seizure

### Incident Response Playbooks
Pre-defined response procedures for specific incident types:
- Ransomware playbook
- Data breach playbook
- Account compromise playbook
- DDoS response playbook

Benefits: Faster response, consistent quality, less reliance on memory during high-stress events.

### Containment Strategies Comparison
| Approach | When to Use | Risk |
|----------|------------|------|
| **Isolate from network** | Stop spread; preserve evidence | May alert attacker |
| **Shut down** | Prevent further damage | Destroys volatile evidence (RAM) |
| **Monitor without touching** | Gather intelligence on attacker TTPs | Attacker continues operating |
| **Decoy/honeypot redirect** | Gather intelligence covertly | Complex; requires planning |

> **Exam Tip:** If forensic investigation is needed, **monitor without touching** or **image before isolating** to capture volatile data (RAM). Shutting down destroys encrypted keys and running process data.

> **Scenario — Containment Strategy Decision:**
> A company discovers an attacker is inside their network and actively accessing HR records. The security team must decide:
>
> **Option A — Isolate immediately:** Cut the attacker off. Stop data theft NOW. But the attacker knows they're detected and activates a "scorched earth" script that wipes logs and destroys evidence on all compromised systems. Legal team has no forensic evidence for the civil case.
>
> **Option B — Monitor without touching:** Let the attacker continue for 24 hours while capturing their every action (packets, keystrokes via UEBA, process executions). Gather intelligence on their TTPs, identify all compromised systems, let them "lead you" to other implants. Risk: more data is exfiltrated during the 24 hours.
>
> **Option C — Honeynet redirect:** Quietly redirect the attacker's sessions to a honeypot environment that looks identical to production but contains fake data. They think they're still in production. Gather intelligence with zero real data at risk.
>
> **Exam guidance:** The answer depends on what the question emphasizes. "Legal evidence is critical" → Option B or C. "Patient safety data is being exfiltrated right now" → Option A immediately. "The question asks about the FIRST step in IR" → always **containment** first (Option A), then forensics.

> **Scenario — Change Management Rollback (CAB Exam Scenario):**
> A patch is deployed to the production payroll server on Friday evening per approved change. Within 20 minutes, payroll processing crashes. The CAB approval required a rollback plan. The rollback plan: revert to the pre-patch VM snapshot taken 30 minutes before deployment. Team executes the rollback — payroll is restored in 45 minutes. Without the pre-approved rollback plan, the team would have been guessing, taking hours longer.
>
> **Exam trap:** "An emergency patch for a critical zero-day must be deployed immediately — there's no time for CAB approval." Even emergency changes require some approval process — an expedited "emergency CAB" with available change manager + CISO sign-off. Skipping authorization entirely violates change management policy. The CISSP answer is always: get *some* form of documented approval, even if expedited.

---

## 7.12 Threat Hunting

Proactive search for threats that evade automated detection (unlike reactive SIEM alerting).

**Process:**
```
Hypothesis → Investigate → Identify Indicators → Refine Detection Rules → Improve Defenses
```

**Data sources for hunting:**
- SIEM log data
- EDR (Endpoint Detection and Response) telemetry
- Network traffic analysis (NTA)
- Threat intelligence (MITRE ATT&CK TTPs)

### MITRE ATT&CK Framework
- Knowledge base of real adversary tactics and techniques
- 14 tactics (Reconnaissance → Impact)
- Hundreds of specific techniques per tactic
- Used to map detections, identify gaps, and prioritize hunting hypotheses

---

## 7.13 Identity Monitoring Operations

### Privileged Activity Monitoring
- All admin commands logged with full context
- Privileged sessions recorded (video + keystroke)
- Alerts on anomalous privileged activity (unusual hours, unusual targets)
- Break-glass account access triggers immediate alert

### User and Entity Behavior Analytics (UEBA)
- Establishes behavioral baseline per user and entity
- Alerts on deviations: accessing new data, unusual login times, large data downloads
- Detects insider threats and compromised accounts that evade signature-based detection

---

## 7.14 Physical Operations Security

### Data Center Physical Security Layers
```
Site perimeter → Building perimeter → Data center floor → Server cage → Individual rack
```

### Media Handling in Operations
| Activity | Control |
|----------|---------|
| Removable media (USB drives) | Disable USB ports; track all removable media |
| Laptop hard drives | Full disk encryption (FDE); remote wipe capability |
| Tape backup transport | Encryption; locked courier case; chain of custody |
| Printer/copier hard drives | Sanitize before disposal; encryption enabled |

---

## 7.15 Exam Tips Summary

- **Order of volatility** — capture RAM before disk.
- **Chain of custody** — document every handoff of evidence.
- **Incident response first priority** — containment (stop the bleeding).
- **3-2-1 backup rule** — 3 copies, 2 media, 1 offsite.
- **Incremental** = fastest backup, slowest restore. **Differential** = slower backup, faster restore.
- **Signature-based IDS** misses zero-days; **anomaly-based** has more false positives.
- **SIEM** correlates logs; **IDS** detects; **IPS** blocks.
- Change management requires a **rollback plan** before any change is approved.
- For vendor access: always require **right to audit** in contracts.
- **UEBA** detects insider threats and compromised accounts by behavioral deviation.
- **MITRE ATT&CK** maps adversary TTPs — used for threat hunting and detection gap analysis.
- Before shutting down a compromised system, **image RAM** to capture decryption keys and volatile evidence.
- Evidence admissibility: authentic + accurate + complete + convincing + admissible (legally obtained).
- **Locard's Exchange Principle** — every contact leaves a trace; foundation of forensic investigation.
