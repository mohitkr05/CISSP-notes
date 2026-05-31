# Domain 7 — Security Operations: Practice Questions

---

**Q1.** During a forensic investigation, an analyst needs to collect evidence from a running system. In what order should the analyst capture evidence based on the order of volatility?

- A) Hard disk → RAM → Network state → CPU registers
- B) CPU registers → RAM → Network state → Hard disk
- C) Network state → Hard disk → RAM → CPU registers
- D) RAM → Hard disk → CPU registers → Network state

**Answer: B**
The order of volatility (most volatile first): CPU registers/cache → RAM → Network state → Running processes → Disk → Remote logs. Capture most volatile first because it disappears soonest.

---

**Q2.** After successfully containing a ransomware incident, the security team removes all traces of the malware and closes the exploited vulnerability. Which incident response phase are they in?

- A) Containment
- B) Recovery
- C) Eradication
- D) Lessons Learned

**Answer: C**
**Eradication** involves removing the malware, closing vulnerabilities, resetting compromised accounts — cleaning the environment so recovery can begin. Recovery follows eradication and involves restoring systems to operation.

---

**Q3.** A company implements a backup strategy where a full backup runs every Sunday, and smaller backups run nightly capturing only changes since the last backup of ANY type. Which backup strategy is this?

- A) Differential
- B) Full + incremental
- C) Full + differential
- D) Continuous data protection

**Answer: B**
**Incremental** backups capture only changes since the last backup of ANY type (full or incremental). This minimizes daily backup size/time but requires a full backup plus all subsequent incrementals for a complete restore.

---

**Q4.** To restore from backup on a Friday using a full backup plus differential strategy, an administrator would need:

- A) Sunday's full backup and Thursday's differential backup
- B) Sunday's full backup and all incremental backups from Monday through Thursday
- C) Thursday's differential backup only
- D) Only Sunday's full backup

**Answer: A**
**Differential** backups capture all changes since the last full backup. To restore on Friday: need Sunday's full backup + Thursday's differential (which contains all changes since Sunday). Only 2 backup sets needed.

---

**Q5.** An IDS generates an alert for a legitimate port scan by the IT team during scheduled maintenance. This is an example of:

- A) True positive
- B) True negative
- C) False positive
- D) False negative

**Answer: C**
A **false positive** occurs when a security system alerts on legitimate activity. The port scan is authorized activity (true negative should be no alert), but the IDS incorrectly flagged it — this is a false positive.

---

**Q6.** Which of the following provides the STRONGEST evidence that a forensic disk image was not tampered with after collection?

- A) Timestamp of the image file
- B) Cryptographic hash (SHA-256) of the original drive and the image
- C) Analyst's signed testimony
- D) Chain of custody documentation

**Answer: B**
A **cryptographic hash** (SHA-256) of both the original drive and the forensic image, taken at the time of collection, mathematically proves the image is an exact, unaltered copy. If the hashes match later, no tampering occurred.

---

**Q7.** An organization's SIEM is generating thousands of alerts daily, most of which are false positives. What is the BEST approach to improve alert quality?

- A) Increase the number of data sources feeding the SIEM
- B) Tune correlation rules and establish behavioral baselines
- C) Replace the SIEM with an IDS
- D) Disable low-priority alerts permanently

**Answer: B**
**Tuning correlation rules** and establishing behavioral baselines reduces false positives by better distinguishing normal from abnormal activity. Alert quality improves without reducing security coverage.

---

**Q8.** During a change advisory board (CAB) meeting, a change request lacks a documented rollback plan. What should happen?

- A) The change should be approved since the risk is low
- B) The change should be rejected or deferred until a rollback plan is documented
- C) The change manager should create a rollback plan on behalf of the requestor
- D) The rollback plan can be created after the change is implemented

**Answer: B**
A **rollback plan** is a mandatory component of change management. Changes without a documented rollback plan should not be approved — if the change fails or causes problems, there must be a defined way to revert to the previous state.

---

**Q9.** Which log management practice is MOST important for ensuring log integrity and admissibility in legal proceedings?

- A) Compressing logs to save storage space
- B) Storing logs on write-once media with cryptographic integrity checks
- C) Reviewing logs daily
- D) Encrypting all log files

**Answer: B**
**Write-once (WORM) media with cryptographic integrity checks** ensures logs cannot be altered after creation — critical for both forensic admissibility and compliance. Tampering becomes detectable.

---

**Q10.** An organization wants to minimize the risk of a rogue privileged administrator abusing credentials. Which control provides the MOST direct protection?

- A) Requiring strong passwords for admin accounts
- B) Implementing just-in-time (JIT) privileged access with session recording
- C) Separating admin and user accounts
- D) Performing quarterly access reviews

**Answer: B**
**Just-in-time access** grants elevated privileges only when needed and for a limited time, minimizing the window of opportunity for abuse. **Session recording** creates an audit trail of all privileged actions — both are provided by PAM solutions.

---

**Q11.** A security analyst discovers that the organization's web application firewall (WAF) was configured to block a critical vulnerability for which no patch yet exists. What is the name for this approach?

- A) Zero-day mitigation
- B) Virtual patching
- C) Compensating control
- D) Risk acceptance

**Answer: B**
**Virtual patching** uses a WAF, IPS, or similar control to block exploitation of a vulnerability without modifying the underlying application code — essential when a vendor patch is unavailable. It is also a type of compensating control.

---

**Q12.** Under NIST SP 800-61, which phase of incident response focuses on determining whether an incident has actually occurred and its scope?

- A) Preparation
- B) Containment
- C) Identification
- D) Eradication

**Answer: C**
The **Identification** phase confirms that an incident has occurred (not a false alarm), determines the type and scope of the incident, classifies severity, and notifies appropriate personnel.

---

**Q13.** An attacker compromises a managed service provider and uses that access to breach multiple client organizations. This is an example of:

- A) A watering hole attack
- B) A supply chain attack
- C) A man-in-the-middle attack
- D) An insider threat

**Answer: B**
This is a **supply chain attack** — targeting a trusted third party (MSP) to gain access to its customers. The SolarWinds attack (2020) is the most prominent example of this technique.

---

**Q14.** A security team discovers that a terminated employee's service account was still active 30 days after their departure and was used for unauthorized access. Which control failure contributed MOST to this incident?

- A) Lack of multi-factor authentication
- B) Inadequate access revocation during offboarding
- C) Absence of a data loss prevention system
- D) Insufficient network segmentation

**Answer: B**
**Access revocation during offboarding** is the control that failed. Service accounts associated with departed employees must be disabled or deleted immediately as part of the termination process.

---

**Q15.** An organization stores three copies of data on two different media types and keeps one copy offsite. What rule does this represent?

- A) 3-2-1 backup rule
- B) RAID-5 redundancy
- C) Geographic data distribution
- D) Tiered storage strategy

**Answer: A**
The **3-2-1 backup rule**: 3 copies of data, 2 different storage media types, 1 stored offsite. This protects against media failure, local disasters, and various threat scenarios.

---

**Q16.** During a forensic investigation, an investigator copies a suspect hard drive using a write-blocking device and generates SHA-256 hashes of both drives. The hashes match. What has been demonstrated?

- A) The drive contains evidence of wrongdoing
- B) The forensic image is an authentic, unaltered copy of the original drive
- C) The write blocker prevented data loss
- D) The original drive has not been tampered with by the suspect

**Answer: B**
Matching SHA-256 hashes of the original drive and the forensic copy demonstrates the **forensic image is an authentic, bit-for-bit copy** with no alterations. This satisfies the best evidence rule and supports chain of custody.

---

**Q17.** A CVSS score of 9.8 is assigned to a newly discovered vulnerability in a critical authentication service. Under a typical vulnerability management policy, how quickly should this be patched?

- A) Within 90 days (standard patch cycle)
- B) Within 30 days
- C) Within 7 days
- D) Within 24-48 hours

**Answer: D**
A CVSS score of 9.8 is **Critical** (9.0–10.0). Critical vulnerabilities in critical systems — especially authentication services — should be addressed within **24-48 hours** given the high exploitation risk.

---

**Q18.** Which type of DLP deployment is BEST suited to prevent an employee from copying sensitive files to a personal USB drive?

- A) Network DLP
- B) Cloud DLP
- C) Endpoint DLP
- D) Storage DLP

**Answer: C**
**Endpoint DLP** controls data on the device itself — including USB ports, print, clipboard, screen capture. It is the only DLP type that can prevent data from being copied to a USB drive before it ever hits the network.

---

**Q19.** During post-incident lessons learned, the team determines the incident could have been detected 48 hours earlier if alert thresholds had been tuned. What is the BEST action item from this finding?

- A) Purchase a new SIEM platform
- B) Tune correlation rules and alert thresholds based on the attack indicators
- C) Add more staff to the SOC
- D) Require all users to undergo security awareness training

**Answer: B**
The specific finding is about alert threshold tuning. The most direct and targeted action item is to **tune the correlation rules and thresholds** based on the IOCs from this incident — this directly addresses the root cause of the detection gap.

---

**Q20.** An organization's DRP specifies an RTO of 4 hours for its core financial system. The BIA determined the MTD is 6 hours. The system was restored in 3.5 hours, but it took an additional 2 hours to verify data integrity before users could access it. Was the MTD met?

- A) Yes — the system was restored within the RTO of 4 hours
- B) No — the total downtime of 5.5 hours exceeded the RTO
- C) Yes — the total downtime of 5.5 hours was within the MTD of 6 hours
- D) No — data integrity verification is not part of the recovery process

**Answer: C**
Total downtime = 3.5 hours (restore) + 2 hours (WRT verification) = **5.5 hours**. This exceeds the RTO of 4 hours but is within the MTD of 6 hours. The RTO was missed, but the MTD was met. (RTO + WRT must be ≤ MTD; 3.5 + 2 = 5.5 ≤ 6 ✓)
