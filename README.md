# CISSP Study Notes — 2024 CBK

Comprehensive study notes for the **Certified Information Systems Security Professional (CISSP)** exam, aligned with the 2024 (ISC)² Common Body of Knowledge.

---

## Exam Overview

| Property | Detail |
|----------|--------|
| Total Questions | 125–175 (CAT format) |
| Duration | 4 hours |
| Passing Score | 700 out of 1000 |
| Format | Adaptive (CAT) — multiple choice + advanced innovative items |
| Experience Required | 5 years cumulative paid work in 2+ domains |

---

## Domain Weights (2024 CBK)

| # | Domain | Exam Weight |
|---|--------|-------------|
| 1 | [Security and Risk Management](01_Security_and_Risk_Management/notes.md) | 16% |
| 2 | [Asset Security](02_Asset_Security/notes.md) | 10% |
| 3 | [Security Architecture and Engineering](03_Security_Architecture_and_Engineering/notes.md) | 13% |
| 4 | [Communication and Network Security](04_Communication_and_Network_Security/notes.md) | 13% |
| 5 | [Identity and Access Management (IAM)](05_Identity_and_Access_Management/notes.md) | 13% |
| 6 | [Security Assessment and Testing](06_Security_Assessment_and_Testing/notes.md) | 12% |
| 7 | [Security Operations](07_Security_Operations/notes.md) | 13% |
| 8 | [Software Development Security](08_Software_Development_Security/notes.md) | 10% |

---

## Per-Domain File Guide

Each domain directory contains:

| File | Contents |
|------|----------|
| `mindmap.md` | Mermaid.js mind map of all domain subtopics |
| `notes.md` | Comprehensive notes: concepts, frameworks, standards, mnemonics, exam tips |
| `questions.md` | 15–20 CISSP-style practice questions with detailed answer explanations |

---

## Study Strategy

1. **Start with Domain 1** — it underpins everything else (risk, governance, policy).
2. **Use the mind maps first** — get the big picture before drilling into details.
3. **Do questions after each domain** — reinforce with active recall.
4. **Focus extra time on Domains 1, 3, 4, 5, 7** — they carry the most weight (68% combined).
5. **Think like a manager, not a technician** — CISSP questions test senior-level judgment.

### The CISSP Mindset
> "When in doubt, think about what a senior security manager would do — the answer is usually the most comprehensive, risk-based, governance-aligned option."

---

## Key Frameworks and Standards Quick Reference

| Framework/Standard | Domain(s) | Purpose |
|---|---|---|
| NIST RMF (SP 800-37) | 1, 6 | Risk management framework |
| NIST CSF | 1 | Cybersecurity framework |
| ISO/IEC 27001 | 1 | ISMS standard |
| ISO/IEC 27002 | 1 | Security controls guidance |
| COBIT | 1 | IT governance |
| ITIL | 7 | IT service management |
| OWASP Top 10 | 8 | Web application security |
| PCI-DSS | 1, 4 | Payment card data security |
| HIPAA | 1, 2 | Healthcare data privacy |
| GDPR | 1, 2 | EU data protection regulation |
| SOC 1/2/3 | 6 | Service organization controls |
| Common Criteria (ISO 15408) | 3 | Security product evaluation |
| FIPS 140-2/3 | 3 | Cryptographic module standards |

---

## Critical Acronyms Master List

| Acronym | Full Form |
|---------|-----------|
| CIA | Confidentiality, Integrity, Availability |
| AAA | Authentication, Authorization, Accounting |
| BCP | Business Continuity Plan |
| DRP | Disaster Recovery Plan |
| ALE | Annualized Loss Expectancy |
| ARO | Annualized Rate of Occurrence |
| SLE | Single Loss Expectancy |
| EF | Exposure Factor |
| AV | Asset Value |
| RPO | Recovery Point Objective |
| RTO | Recovery Time Objective |
| MTD | Maximum Tolerable Downtime |
| WRT | Work Recovery Time |
| TCO | Total Cost of Ownership |
| PKI | Public Key Infrastructure |
| CA | Certificate Authority |
| CRL | Certificate Revocation List |
| OCSP | Online Certificate Status Protocol |
| SSO | Single Sign-On |
| MFA | Multi-Factor Authentication |
| RBAC | Role-Based Access Control |
| MAC | Mandatory Access Control |
| DAC | Discretionary Access Control |
| ABAC | Attribute-Based Access Control |
| IDS | Intrusion Detection System |
| IPS | Intrusion Prevention System |
| SIEM | Security Information and Event Management |
| DLP | Data Loss Prevention |
| SDLC | Software Development Life Cycle |
| WAF | Web Application Firewall |
| DMZ | Demilitarized Zone |
| VLAN | Virtual Local Area Network |
| VPN | Virtual Private Network |
| PAM | Privileged Access Management |
| UEBA | User and Entity Behavior Analytics |
| CVSS | Common Vulnerability Scoring System |
| SBOM | Software Bill of Materials |
| IOC | Indicator of Compromise |
| TTPs | Tactics, Techniques, and Procedures |
| STIX | Structured Threat Information Expression |
| TAXII | Trusted Automated Exchange of Intelligence Information |
| FIDO2 | Fast Identity Online v2 (passwordless standard) |
| JIT | Just-in-Time (access) |
| ATO | Authorization to Operate |
| IGA | Identity Governance and Administration |
| NAC | Network Access Control |
| UEBA | User and Entity Behavior Analytics |
| MSSP | Managed Security Services Provider |
| APT | Advanced Persistent Threat |
| DPIA | Data Protection Impact Assessment |
| PIA | Privacy Impact Assessment |
| SoD | Separation of Duties |
| TCB | Trusted Computing Base |
| EAL | Evaluation Assurance Level |
| HMAC | Hash-based Message Authentication Code |
| TLS | Transport Layer Security |
| IPSec | Internet Protocol Security |
| AH | Authentication Header (IPSec) |
| ESP | Encapsulating Security Payload (IPSec) |

---

## High-Frequency Exam Topics — Quick Reference

These topics appear most frequently on the CISSP exam. Review these until automatic.

### Must-Know Formulas
```
SLE = Asset Value (AV) × Exposure Factor (EF)
ALE = SLE × Annualized Rate of Occurrence (ARO)
Safeguard Value = ALE(before) − ALE(after) − Annual Control Cost
RTO + WRT ≤ MTD (Maximum Tolerable Downtime)
```

### Must-Know Model Rules
| Model | Property | Rule |
|-------|----------|------|
| Bell-LaPadula | ss-property | No Read Up |
| Bell-LaPadula | *-property | No Write Down |
| Biba | Simple Integrity | No Read Down |
| Biba | Star Integrity | No Write Up |

### Must-Know Order of Operations
| Topic | Correct Order |
|-------|-------------|
| Incident Response (NIST 800-61) | Preparation → Identification → Containment → Eradication → Recovery → Lessons Learned |
| Forensic Process | Identification → Preservation → Collection → Examination → Analysis → Presentation |
| Order of Volatility | CPU registers → RAM → Network state → Running processes → Disk → Remote logs → Archival |
| BCP Process | Scope → BIA → Strategy → Plan → Implementation → Testing → Maintenance |
| BCP Test Types (least to most disruptive) | Checklist → Tabletop → Walkthrough → Simulation → Parallel → Full Interruption |
| NIST RMF | Categorize → Select → Implement → Assess → Authorize → Monitor |

### Must-Know "Which Is Better" Comparisons
| Comparison | Answer |
|------------|--------|
| SOC 2 Type I vs. Type II | Type II is stronger (period of time) |
| Incremental vs. Differential restore speed | Differential is faster to restore |
| Authenticated vs. Unauthenticated scan | Authenticated gives more complete results |
| FAR vs. FRR vs. CER | Lower CER = more accurate biometric |
| TACACS+ vs. RADIUS for device admin | TACACS+ (full encryption, TCP, granular) |
| WPA2-Enterprise vs. WPA2-Personal | WPA2-Enterprise (802.1X, no shared PSK) |
| SAST vs. DAST — earliest detection | SAST (finds issues without running app) |
| Hot vs. Warm vs. Cold site recovery speed | Hot (fastest) → Warm → Cold (slowest) |
| Black box vs. White box — most realistic | Black box (no prior knowledge = real attacker) |

### Must-Know Management Priorities
- **First in IR**: Containment (stop the spread)
- **First in BCP**: BIA (Business Impact Analysis)
- **Who accepts risk**: Senior management (not the security team)
- **Who classifies data**: Data owner (not custodian)
- **Who is ultimately responsible for security**: Board/Senior Management
- **CISSP ethics canon priority**: Society > Organization > Client > Profession
- **When regulation conflicts with policy**: Follow the more restrictive one

### Must-Know Attack → Defense Mappings
| Attack | Primary Defense |
|--------|----------------|
| SQL Injection | Parameterized queries |
| XSS | Output encoding / CSP |
| CSRF | CSRF tokens / SameSite cookies |
| Buffer overflow | Input validation / ASLR / DEP |
| ARP poisoning | Dynamic ARP Inspection (DAI) |
| MAC flooding | Port security |
| DNS poisoning | DNSSEC |
| Evil twin AP | 802.1X / EAP-TLS with certificate validation |
| Kerberoasting | Strong service account passwords; managed service accounts |
| Golden Ticket | Protect KRBTGT account; regular KRBTGT rotation |
| SYN flood | SYN cookies; rate limiting |
| Ransomware | Air-gapped backups; EDR; least privilege |
| Social engineering | Security awareness training; phishing simulations |
| Tailgating | Mantrap; security guards; training |

---

*Notes aligned with (ISC)² CISSP Exam Outline, 2024 edition.*
