# Domain 1 — Security and Risk Management: Practice Questions

---

**Q1.** A company's asset is valued at $500,000. A vulnerability could result in a 40% loss of the asset. The threat is expected to occur once every two years. What is the Annualized Loss Expectancy (ALE)?

- A) $200,000
- B) $100,000
- C) $50,000
- D) $400,000

**Answer: B**
SLE = AV × EF = $500,000 × 0.40 = $200,000. ARO = 0.5 (once every two years). ALE = SLE × ARO = $200,000 × 0.5 = **$100,000**.

---

**Q2.** Which of the following BEST demonstrates due care?

- A) Researching which firewall to purchase before buying
- B) Continuously monitoring network activity and patching systems
- C) Performing a risk assessment before deploying a new system
- D) Reviewing vendor security certifications before signing a contract

**Answer: B**
Due care is the ongoing effort to do the right thing — monitoring and patching are continuous actions. Options A, C, and D represent due **diligence** (research and investigation before acting).

---

**Q3.** An organization decides to purchase cyber liability insurance to address the risk of a data breach. Which risk response strategy is being used?

- A) Risk mitigation
- B) Risk avoidance
- C) Risk acceptance
- D) Risk transfer

**Answer: D**
Cyber insurance shifts the financial consequences of a risk to a third party (the insurer). This is **risk transfer**.

---

**Q4.** Which role is responsible for determining the sensitivity level of data and assigning a classification label?

- A) Data custodian
- B) Data user
- C) Data owner
- D) Security administrator

**Answer: C**
The **data owner** (typically a business unit manager) is responsible for classifying data and determining who should have access. The custodian implements the controls the owner specifies.

---

**Q5.** During a BIA, the team determines that the accounting system must be restored within 24 hours or the company will face irreversible financial harm. Which metric does this describe?

- A) RPO
- B) WRT
- C) MTD
- D) RTO

**Answer: D**
The **RTO (Recovery Time Objective)** is the target time within which a system must be restored after an outage. The 24-hour threshold for recovery is the RTO. MTD is the maximum acceptable downtime before irreversible harm — if given as the outer boundary, it would be MTD; here the question describes the restoration target, which is RTO.

---

**Q6.** A bank implements a policy requiring two employees to always be present when accessing the vault. Which control does this represent?

- A) Separation of duties
- B) Dual control
- C) Two-person integrity
- D) Least privilege

**Answer: C**
**Two-person integrity (TPI)** requires two people to be physically present simultaneously. **Dual control** requires two people to provide separate inputs/credentials. This scenario — two employees physically present — is TPI.

---

**Q7.** Which of the following BCP tests causes the MOST disruption to normal operations?

- A) Tabletop exercise
- B) Parallel test
- C) Full interruption test
- D) Simulation

**Answer: C**
A **full interruption test** shuts down the primary site and requires operations to run entirely from the alternate site. It is the most realistic and most disruptive test type.

---

**Q8.** According to the (ISC)² Code of Ethics, which canon takes the highest priority?

- A) Advance and protect the profession
- B) Act honorably, honestly, justly, responsibly, and legally
- C) Provide diligent and competent service to principals
- D) Protect society, the common good, necessary public trust and confidence, and the infrastructure

**Answer: D**
The canons are listed in priority order. Protecting **society** is canon #1 and always takes precedence over obligations to employers, clients, or the profession.

---

**Q9.** Which framework uses the five core functions: Identify, Protect, Detect, Respond, and Recover?

- A) COBIT
- B) ISO 27001
- C) NIST Cybersecurity Framework (CSF)
- D) ITIL

**Answer: C**
The **NIST CSF** organizes cybersecurity activities into five core functions: Identify, Protect, Detect, Respond, and Recover. COBIT focuses on IT governance; ISO 27001 on ISMS; ITIL on service management.

---

**Q10.** A company terminates an employee who had administrative access to critical systems. What is the MOST important immediate action?

- A) Conduct an exit interview
- B) Revoke all system access
- C) Change the employee's password
- D) Perform a security audit of their activity

**Answer: B**
Immediate **revocation of access** is the most critical step to prevent a disgruntled or departing employee from causing damage. Everything else (exit interview, audit) can follow, but access must be cut first.

---

**Q11.** Which of the following is an example of a qualitative risk analysis technique?

- A) Annualized Loss Expectancy calculation
- B) Single Loss Expectancy formula
- C) Delphi Technique
- D) Return on Security Investment analysis

**Answer: C**
The **Delphi Technique** uses anonymous expert consensus — it is qualitative (opinion-based). ALE, SLE, and ROSI are all quantitative (formula-based).

---

**Q12.** An organization wants to implement a control to detect if a single employee is committing ongoing fraud. Which control is MOST effective?

- A) Separation of duties
- B) Background checks
- C) Mandatory vacations
- D) Least privilege

**Answer: C**
**Mandatory vacations** force someone else to cover the employee's duties, which often exposes ongoing fraud schemes. Background checks are preventive/pre-hiring. Separation of duties prevents fraud but doesn't detect ongoing schemes by a single person.

---

**Q13.** Trade secret protection differs from patent protection primarily because:

- A) Trade secrets are registered with a government authority
- B) Trade secrets can last indefinitely as long as secrecy is maintained
- C) Trade secrets must be disclosed after 20 years
- D) Trade secrets apply only to software inventions

**Answer: B**
**Trade secrets** have no registration requirement and no fixed expiration — protection lasts as long as the information is kept secret. Patents last 20 years from filing and require public disclosure.

---

**Q14.** A CISO is evaluating the cost-effectiveness of a new security control. The ALE before the control is $150,000, the ALE after is $60,000, and the annual cost of the control is $50,000. Should the control be implemented?

- A) Yes, because ALE is reduced
- B) No, because the control costs more than the reduction
- C) Yes, because the net savings is $40,000 per year
- D) No, because residual risk remains

**Answer: C**
Safeguard Value = ALE(before) − ALE(after) − Annual Cost = $150,000 − $60,000 − $50,000 = **$40,000 net savings**. The control is cost-effective.

---

**Q15.** Which recovery site option provides the FASTEST recovery time but is also the MOST expensive to maintain?

- A) Cold site
- B) Warm site
- C) Hot site
- D) Reciprocal agreement

**Answer: C**
A **hot site** is fully equipped with hardware, software, and real-time data replication. It allows recovery in minutes to hours, but requires maintaining a fully operational duplicate facility — the highest cost.

---

**Q16.** An employee is given only the permissions required to perform their specific job duties and nothing more. Which security principle does this represent?

- A) Need to know
- B) Least privilege
- C) Separation of duties
- D) Job rotation

**Answer: B**
**Least privilege** means granting the minimum access rights necessary. **Need to know** is a related concept specifically about information access (knowing only what's required for a task). Least privilege applies more broadly to system permissions.

---

**Q17.** GDPR requires organizations to notify supervisory authorities of a personal data breach within:

- A) 24 hours
- B) 48 hours
- C) 72 hours
- D) 7 days

**Answer: C**
**GDPR Article 33** mandates notification to the relevant supervisory authority within **72 hours** of becoming aware of the breach (where feasible).

---

**Q18.** Which of the following BEST describes the relationship between policies, standards, guidelines, and procedures?

- A) All four are mandatory and legally binding
- B) Policies are mandatory; standards are mandatory; guidelines are optional; procedures are step-by-step
- C) Guidelines are mandatory; procedures are optional
- D) Standards replace policies in most organizations

**Answer: B**
**Policies** — mandatory high-level direction. **Standards** — mandatory specific requirements. **Guidelines** — recommended but not mandatory. **Procedures** — mandatory step-by-step instructions. Both policies and procedures are mandatory; guidelines are flexible.

---

**Q19.** A company determines that a risk is too costly to mitigate and the potential impact is within acceptable bounds. The risk is formally documented and accepted by management. This is an example of:

- A) Risk transference
- B) Risk avoidance
- C) Residual risk acceptance
- D) Risk mitigation

**Answer: C**
When management formally acknowledges and accepts a risk — especially the **residual risk** remaining after controls — this is **risk acceptance**. It requires formal sign-off from senior management.

---

**Q20.** Which of the following is the PRIMARY purpose of a Business Impact Analysis (BIA)?

- A) To identify the root cause of a disaster
- B) To determine the financial and operational impact of disruptions to critical business functions
- C) To test the effectiveness of the disaster recovery plan
- D) To assign recovery priorities to IT assets

**Answer: B**
The **BIA** identifies critical business functions, analyzes the impact of their disruption (financial, reputational, regulatory), and defines recovery objectives (RTO, RPO, MTD). It is the foundation for all continuity and recovery planning.
