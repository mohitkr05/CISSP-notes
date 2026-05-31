# Domain 2 — Asset Security: Practice Questions

---

**Q1.** An organization is disposing of old hard drives that contained classified financial records. According to NIST SP 800-88, which sanitization method ensures the data cannot be recovered even with advanced forensic tools?

- A) Clear
- B) Overwrite
- C) Purge
- D) Destroy

**Answer: D**
**Destroy** (physical destruction via shredding, incineration, or disintegration) provides the highest level of assurance. Clear and overwrite can be defeated by advanced forensics. Purge is effective but destroy is the most definitive method.

---

**Q2.** A cloud service provider stores customer data on encrypted storage volumes. When a customer terminates their contract, the provider destroys the encryption keys instead of the physical media. Which disposal method does this represent?

- A) Degaussing
- B) Purging
- C) Crypto-shredding
- D) Clearing

**Answer: C**
**Crypto-shredding** renders data unreadable by destroying the encryption key. Since the provider cannot physically destroy shared cloud infrastructure, this is the appropriate approach for cloud environments.

---

**Q3.** Under GDPR, which role determines the purpose and means of processing personal data?

- A) Data processor
- B) Data subject
- C) Data controller
- D) Data custodian

**Answer: C**
The **data controller** decides WHY and HOW personal data is processed. The data processor acts on behalf of the controller. The data subject is the individual whose data is collected.

---

**Q4.** Which of the following is the BEST example of Privacy by Design?

- A) Adding encryption to a system after a data breach
- B) Including privacy impact assessments during the design phase
- C) Publishing a privacy policy on the company website
- D) Encrypting backups when requested by the compliance team

**Answer: B**
**Privacy by Design** means privacy is embedded proactively from the start of system design — not added after the fact. Conducting PIAs during design embodies this principle.

---

**Q5.** A healthcare company maintains patient records for 10 years as required by regulation, then permanently deletes them. Which GDPR principle does this practice support?

- A) Purpose limitation
- B) Data minimization
- C) Storage limitation
- D) Accuracy

**Answer: C**
**Storage limitation** requires that data not be kept longer than necessary for its purpose. Deleting records after the required retention period directly supports this principle.

---

**Q6.** An IT administrator is responsible for backing up and encrypting the company's customer database. What role does this person hold with respect to the data?

- A) Data owner
- B) Data controller
- C) Data custodian
- D) Data steward

**Answer: C**
The **data custodian** is responsible for the technical implementation of security controls — including backups and encryption — as directed by the data owner. The custodian implements; the owner decides policy.

---

**Q7.** Which data state is MOST difficult to protect against unauthorized access or exfiltration?

- A) Data at rest
- B) Data in transit
- C) Data in use
- D) Data archived

**Answer: C**
**Data in use** resides in memory and CPU registers while being processed — it is temporarily unencrypted and exposed. Controls are limited; specialized technologies like secure enclaves (Intel SGX) are needed.

---

**Q8.** An organization wants to ensure old magnetic backup tapes are completely sanitized before reuse. Which method is MOST appropriate?

- A) Overwriting with zeros
- B) Degaussing
- C) Crypto-shredding
- D) Incineration

**Answer: B**
**Degaussing** uses a strong magnetic field to erase data from magnetic media like tapes and traditional hard drives. It is highly effective for magnetic media and allows reuse (though the media may need re-certification after degaussing).

---

**Q9.** Which of the following is NOT an effective method for sanitizing a solid-state drive (SSD)?

- A) Crypto-shredding
- B) Physical shredding
- C) Degaussing
- D) Manufacturer secure erase commands

**Answer: C**
**Degaussing does NOT work on SSDs** because SSDs use flash memory (not magnetic storage). The magnetic field has no effect on flash chips. Physical shredding, crypto-shredding, and manufacturer-provided secure erase are all appropriate for SSDs.

---

**Q10.** A company collects customer email addresses to send order confirmations, but also uses them for marketing campaigns without customer consent. Which GDPR principle is being violated?

- A) Accuracy
- B) Data minimization
- C) Purpose limitation
- D) Storage limitation

**Answer: C**
**Purpose limitation** requires that data be collected only for specified, explicit, and legitimate purposes and not processed further in a way incompatible with those purposes. Using emails for marketing (a different purpose) without consent violates this principle.

---

**Q11.** A document is labeled "TOP SECRET" in headers and footers. This is an example of:

- A) Data classification
- B) Data labeling
- C) Data marking
- D) Data handling

**Answer: C**
**Marking** refers to human-readable indicators (headers, footers, cover sheets) applied to documents. **Labeling** refers to machine-readable metadata tags used by automated DLP and classification systems.

---

**Q12.** Which of the following BEST describes the concept of data minimization?

- A) Encrypting all data to reduce its exposure footprint
- B) Retaining data for the minimum required time
- C) Collecting only the data that is necessary for the stated purpose
- D) Limiting access to data to the minimum number of users

**Answer: C**
**Data minimization** (a GDPR principle) means organizations should only collect and process personal data that is adequate, relevant, and limited to what is necessary for the stated purpose.

---

**Q13.** An employee receives an email classified as "Proprietary" and forwards it to a personal email account. Which security control would MOST directly prevent this?

- A) Awareness training
- B) Network firewall
- C) Data Loss Prevention (DLP) system
- D) Digital Rights Management (DRM)

**Answer: C**
A **DLP system** monitors and blocks the transmission of sensitive data based on content inspection and classification labels. It is the most direct technical control to prevent data exfiltration via email.

---

**Q14.** According to Privacy by Design, privacy protections should be:

- A) Reactive and post-incident
- B) Optional based on data classification
- C) Proactive and built into systems by default
- D) Applied only to sensitive personal information

**Answer: C**
A core principle of **Privacy by Design** is that privacy protections are proactive (not reactive), preventive (not remedial), and set as the **default** — not requiring users to opt in.

---

**Q15.** A regulated organization must preserve all email communications related to an ongoing lawsuit beyond the standard 3-year retention policy. This is called a:

- A) Data retention extension
- B) Legal hold
- C) Data preservation request
- D) Litigation lock

**Answer: B**
A **legal hold** (also called a litigation hold) is a directive to preserve all potentially relevant information beyond normal retention schedules when litigation is anticipated or in progress.

---

**Q16.** Which of the following BEST describes the role of a data steward?

- A) Defines security controls for sensitive data
- B) Processes data on behalf of the data controller
- C) Ensures data quality, accuracy, and governance compliance
- D) Has ultimate accountability for classification decisions

**Answer: C**
A **data steward** manages data quality, metadata, and governance — ensuring that data is accurate, consistent, and compliant with governance policies. Unlike data owners, stewards focus on quality and governance rather than classification authority.

---

**Q17.** An organization is migrating its data center to a cloud provider. Which asset management concern is MOST important to address first?

- A) Ensuring physical access to cloud servers
- B) Inventorying all data assets and their classification before migration
- C) Installing endpoint agents on cloud VMs
- D) Implementing multi-factor authentication for cloud accounts

**Answer: B**
Before migrating, the organization must **inventory and classify all data assets** to understand what is moving, what controls are required, and whether cloud storage meets applicable regulatory requirements for each classification level.

---

**Q18.** What is the PRIMARY purpose of a Certificate of Destruction?

- A) To document the data classification of destroyed media
- B) To provide legal evidence that media was properly sanitized or destroyed
- C) To authorize third parties to dispose of sensitive media
- D) To record the chain of custody during data transfer

**Answer: B**
A **Certificate of Destruction** is a formal document (usually from a certified disposal vendor) that serves as legal evidence that sensitive media was properly sanitized or destroyed — essential for regulatory compliance (HIPAA, PCI-DSS).

---

**Q19.** A data subject requests that an organization delete all personal data it holds about them. Under GDPR, this is known as the:

- A) Right to restrict processing
- B) Right to data portability
- C) Right to erasure
- D) Right to object

**Answer: C**
The **right to erasure** ("right to be forgotten") under GDPR Article 17 allows data subjects to request deletion of their personal data under certain circumstances, such as when it is no longer necessary for the purpose it was collected.

---

**Q20.** An organization processes credit card data and must comply with PCI-DSS. Which classification level would be MOST appropriate for stored cardholder data?

- A) Public
- B) Sensitive
- C) Private
- D) Confidential / Proprietary

**Answer: D**
Cardholder data is the most sensitive commercial classification — **Confidential or Proprietary**. PCI-DSS requirements (encryption at rest, strict access controls, audit logging) align with the highest commercial classification tier.
