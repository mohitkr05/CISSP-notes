# Domain 6 — Security Assessment and Testing: Practice Questions

---

**Q1.** A security firm is hired to test an organization's defenses. The testers are given no information about the environment and must discover everything themselves. What type of penetration test is this?

- A) White box
- B) Gray box
- C) Black box
- D) Crystal box

**Answer: C**
**Black box** penetration testing provides the testers with no prior knowledge of the target environment. They must perform reconnaissance and enumeration to map the target, simulating an external attacker's perspective.

---

**Q2.** Before conducting a penetration test, what document is MOST critical to obtain?

- A) A network diagram of the target environment
- B) Written authorization from the system owner
- C) A list of known vulnerabilities in the target systems
- D) The organization's incident response plan

**Answer: B**
**Written authorization** (rules of engagement, scope document, authorization letter) from the appropriate system owner is legally required before any penetration test begins. Testing without authorization is a crime regardless of intent.

---

**Q3.** A cloud service provider has undergone an independent audit that assessed whether their security controls were operating effectively over a 12-month period. What type of report would document this audit?

- A) SOC 1 Type I
- B) SOC 2 Type I
- C) SOC 2 Type II
- D) SOC 3

**Answer: C**
**SOC 2 Type II** covers the Trust Services Criteria (security, availability, etc.) assessed over a **period of time** (typically 6–12 months), demonstrating that controls are not only designed appropriately but operating effectively over time.

---

**Q4.** An organization discovers that attackers breached their systems 47 days before the breach was detected. Which security metric does this 47-day period represent?

- A) MTTR (Mean Time to Respond)
- B) MTTD (Mean Time to Detect)
- C) RTO (Recovery Time Objective)
- D) ALE (Annualized Loss Expectancy)

**Answer: B**
**MTTD (Mean Time to Detect)** measures the average time from when an attack occurs to when it is discovered. A shorter MTTD means threats are identified more quickly, limiting the attacker's dwell time.

---

**Q5.** Which assessment type identifies security weaknesses but stops short of actually exploiting them?

- A) Penetration test
- B) Red team exercise
- C) Vulnerability assessment
- D) War games

**Answer: C**
A **vulnerability assessment** scans for and identifies weaknesses but does not exploit them. A penetration test goes further by actually exploiting vulnerabilities to demonstrate the real impact and depth of potential compromise.

---

**Q6.** A development team runs automated security scanning tools against the source code repository after every code commit. What type of testing is this?

- A) DAST
- B) SAST
- C) IAST
- D) Fuzz testing

**Answer: B**
**SAST (Static Application Security Testing)** analyzes source code without executing it. Integrating SAST into the CI/CD pipeline at the commit or build stage is a core DevSecOps practice.

---

**Q7.** An organization wants to verify that their security monitoring systems can detect a real attack. They hire a team to perform a covert attack simulation without notifying the blue team. What is this called?

- A) Purple team exercise
- B) Tabletop exercise
- C) Red team exercise
- D) Security audit

**Answer: C**
A **red team exercise** simulates a real attacker (adversary simulation) without the defenders (blue team) being notified. This tests whether the blue team can detect and respond to a realistic attack — as opposed to a purple team exercise where both teams collaborate openly.

---

**Q8.** A company reviews SOC 2 Type I vs. Type II reports from a potential cloud vendor. Why is Type II preferred?

- A) Type II covers financial controls while Type I covers security
- B) Type II is publicly available while Type I is restricted
- C) Type II demonstrates controls operated effectively over time, not just at a point in time
- D) Type II requires fewer audit resources and is more cost-effective

**Answer: C**
**SOC 2 Type II** is preferred because it demonstrates controls were **actually operating effectively** over an extended period (e.g., 12 months). Type I only proves controls were properly designed at a single point in time — providing weaker assurance.

---

**Q9.** A security team wants to measure the effectiveness of their patch management program. Which metric is MOST appropriate?

- A) Mean Time to Detect (MTTD)
- B) Patch compliance rate (% of systems patched within policy window)
- C) False positive rate of IDS alerts
- D) Number of security incidents per quarter

**Answer: B**
**Patch compliance rate** directly measures how effectively the patch management program is working — what percentage of systems are patched within the policy-defined window. It is the most direct KPI for patch management effectiveness.

---

**Q10.** During a penetration test, the tester escalates from a standard user account to domain administrator. What phase of the penetration testing methodology does this represent?

- A) Reconnaissance
- B) Scanning
- C) Exploitation
- D) Post-exploitation (privilege escalation)

**Answer: D**
**Privilege escalation** during a penetration test occurs in the **post-exploitation** phase — after initial access has been gained. The tester moves from limited access toward the most privileged accounts to demonstrate the full scope of compromise.

---

**Q11.** An organization must conduct a Data Protection Impact Assessment (DPIA) before deploying a new HR system that processes employee personal data. Which regulation requires this?

- A) HIPAA
- B) PCI-DSS
- C) GDPR
- D) SOX

**Answer: C**
**GDPR Article 35** requires a **DPIA (Data Protection Impact Assessment)** before processing operations that are likely to result in high risks to individuals' rights and freedoms — including systematic monitoring of employees or large-scale processing of sensitive personal data.

---

**Q12.** A phishing simulation reveals that 35% of employees click the simulated phishing link. How should this result be classified?

- A) KRI indicating a high risk of social engineering compromise
- B) KPI demonstrating effective security controls
- C) A false positive requiring recalibration
- D) A penetration testing finding

**Answer: A**
A 35% click rate is a **Key Risk Indicator (KRI)** — an early warning signal of elevated risk. It indicates employees are susceptible to social engineering. A low click rate would be a positive KPI; a high rate signals that additional training is needed.

---

**Q13.** During a code review, a reviewer discovers that user input is directly passed to an `eval()` function in the application. What vulnerability does this represent, and what testing method would MOST likely catch this issue earliest in the SDLC?

- A) XSS vulnerability; DAST
- B) Command injection / RCE vulnerability; SAST
- C) SQL injection; vulnerability scan
- D) Buffer overflow; fuzz testing

**Answer: B**
Passing user input to `eval()` creates a **remote code execution (command injection)** vulnerability. **SAST** catches this by analyzing source code — it would flag the dangerous `eval()` call during the development/build phase, earliest in the SDLC ("shift left").

---

**Q14.** An organization uses automated scripts to simulate customer login, search, and checkout processes every 5 minutes and alert if any step fails. What is this technique called?

- A) Regression testing
- B) Synthetic transaction monitoring
- C) Performance testing
- D) DAST scanning

**Answer: B**
**Synthetic transaction monitoring** uses scripted simulations of real user transactions to continuously verify system availability and performance, and to detect failures before real users are impacted.

---

**Q15.** What is the PRIMARY difference between a security audit and a penetration test?

- A) Penetration tests use automated tools; security audits are always manual
- B) Security audits verify compliance with policies/standards; penetration tests attempt to exploit vulnerabilities
- C) Security audits are conducted externally; penetration tests are always internal
- D) Penetration tests check for policy violations; security audits exploit vulnerabilities

**Answer: B**
A **security audit** evaluates whether systems and processes comply with security policies, standards, and regulations. A **penetration test** actively attempts to exploit vulnerabilities to determine how far an attacker could penetrate. Audits measure compliance; pen tests measure exploitability.

---

**Q16.** A security team tracks the average number of days between when a vulnerability is discovered in production and when it is successfully patched. What metric is this?

- A) MTTD
- B) MTTR (Mean Time to Remediate)
- C) Vulnerability density
- D) Patch window compliance

**Answer: B**
**Mean Time to Remediate (MTTR)** — in the context of vulnerability management — measures the average time from vulnerability discovery to successful remediation. Shorter MTTR indicates a more responsive patching process.

---

**Q17.** A security assessor wants to understand a vendor's security posture without disclosing that an audit is taking place. Which type of assessment enables this?

- A) Announced internal audit
- B) White box penetration test
- C) Covert (unannounced) external assessment
- D) SOC 2 Type II review

**Answer: C**
A **covert (unannounced) assessment** tests the organization's controls and detection capabilities without prior notification. This provides the most realistic view of security posture — announced assessments often lead to temporary improvements that don't reflect day-to-day security.

---

**Q18.** Which of the following BEST describes the purpose of a purple team exercise?

- A) Test physical security controls with unannounced entry attempts
- B) Enable red and blue teams to collaborate and share knowledge to improve both offensive and defensive capabilities
- C) Simulate a nation-state attack on critical infrastructure
- D) Conduct a joint tabletop exercise with multiple organizations

**Answer: B**
A **purple team exercise** is a collaborative engagement where the red team (attackers) and blue team (defenders) work together openly — red team demonstrates techniques, blue team tunes detection and response. The goal is knowledge transfer to improve overall security posture.

---

**Q19.** An authenticated vulnerability scan of a web server returns 12 critical findings. An unauthenticated scan of the same server returns 3 critical findings. What does this difference indicate?

- A) The authenticated scan is less reliable
- B) Authenticated scans provide deeper visibility into vulnerabilities not exposed externally
- C) The web server has been hardened against external attacks
- D) Unauthenticated scans are more accurate for web servers

**Answer: B**
**Authenticated scans** use credentials to access the system and examine internal configurations, installed software versions, and local patches — revealing vulnerabilities invisible to external unauthenticated scans. The 9 additional findings are internal vulnerabilities not exposed to the network.

---

**Q20.** A hospital prepares to deploy a new patient records system. Which assessment is legally required before processing patient data under GDPR?

- A) SOC 2 Type II audit
- B) Penetration test
- C) Data Protection Impact Assessment (DPIA)
- D) HIPAA risk analysis

**Answer: C**
**GDPR requires a DPIA** for high-risk personal data processing activities, including healthcare data at scale. A DPIA identifies privacy risks, documents mitigation measures, and may require consultation with the supervisory authority before processing begins. (Note: HIPAA risk analysis is separately required under US law.)
