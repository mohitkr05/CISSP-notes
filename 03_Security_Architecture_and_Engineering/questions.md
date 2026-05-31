# Domain 3 — Security Architecture and Engineering: Practice Questions

---

**Q1.** A SECRET-cleared user attempts to read a TOP SECRET document. According to the Bell-LaPadula model, this action should be:

- A) Allowed, because the user is cleared
- B) Denied, because of the "no write down" property
- C) Denied, because of the "no read up" property
- D) Allowed, because integrity is maintained

**Answer: C**
The Bell-LaPadula **simple security property (no read up)** prevents subjects from reading objects at a higher classification level. A SECRET-cleared user cannot read a TOP SECRET document.

---

**Q2.** Which security model is MOST appropriate for preventing conflicts of interest in investment banking?

- A) Bell-LaPadula
- B) Biba
- C) Clark-Wilson
- D) Brewer-Nash (Chinese Wall)

**Answer: D**
The **Brewer-Nash (Chinese Wall)** model was specifically designed to prevent conflicts of interest. Once a user accesses data from one company, they are blocked from accessing data from competing organizations.

---

**Q3.** A developer accidentally leaves a debug login backdoor in production code. What type of vulnerability is this?

- A) Covert timing channel
- B) Maintenance hook
- C) TOC/TOU vulnerability
- D) Salami attack

**Answer: B**
A **maintenance hook** (or backdoor) is undocumented access functionality left by developers, sometimes intentionally for testing, that bypasses normal authentication.

---

**Q4.** Which block cipher mode produces identical ciphertext for identical plaintext blocks and should NEVER be used?

- A) CBC
- B) CTR
- C) GCM
- D) ECB

**Answer: D**
**ECB (Electronic Codebook)** encrypts each block independently, so identical plaintext blocks produce identical ciphertext blocks. This reveals patterns and is cryptographically insecure.

---

**Q5.** An organization needs to implement encryption that provides both confidentiality and authentication for network traffic. Which cipher mode is BEST suited?

- A) ECB
- B) CBC
- C) GCM
- D) OFB

**Answer: C**
**GCM (Galois/Counter Mode)** provides **Authenticated Encryption with Associated Data (AEAD)** — both confidentiality (CTR-based encryption) and integrity/authentication (GHASH). It is used in TLS 1.3 and is the preferred mode for modern systems.

---

**Q6.** What is the PRIMARY purpose of the Diffie-Hellman algorithm?

- A) Encrypting large volumes of data
- B) Creating digital signatures
- C) Securely exchanging cryptographic keys over an insecure channel
- D) Generating one-time passwords

**Answer: C**
**Diffie-Hellman** is a **key exchange** protocol. It allows two parties to establish a shared secret key over an insecure channel without transmitting the key itself. It does not encrypt data or create signatures.

---

**Q7.** A digital certificate with EAL4 assurance was evaluated under which standard?

- A) TCSEC (Orange Book)
- B) Common Criteria (ISO/IEC 15408)
- C) FIPS 140-3
- D) ISO 27001

**Answer: B**
**EAL (Evaluation Assurance Levels)** are defined by the **Common Criteria (ISO/IEC 15408)** standard. EAL4 is the highest level typically achieved by commercial products and means "methodically designed, tested, and reviewed."

---

**Q8.** According to the Biba integrity model, a user at a HIGH integrity level trying to read data at a LOW integrity level should be:

- A) Allowed — integrity is maintained when reading lower data
- B) Denied — no read down property prevents reading lower integrity data
- C) Allowed — Biba only restricts writing, not reading
- D) Denied — no write up property prevents this

**Answer: B**
**Biba's simple integrity property (no read down)** prevents subjects from reading objects at a lower integrity level, to prevent contamination by untrustworthy data. No read down, no write up.

---

**Q9.** A fire breaks out in a server room. Which fire suppression agent should NOT be used?

- A) CO2
- B) FM-200
- C) Inergen
- D) Water

**Answer: D**
**Water** should never be used on electrical fires (Class C). It conducts electricity and can damage equipment. CO2, FM-200, and Inergen are all appropriate suppressants for electrical/data center environments.

---

**Q10.** Which of the following BEST describes a mantrap?

- A) A motion-sensing alarm system at facility entrances
- B) A double-door entry system that requires the first door to close before the second opens
- C) A turnstile that counts people entering a secure area
- D) A CCTV system focused on entrance monitoring

**Answer: B**
A **mantrap** (also called an airlock) is a physical access control using two interlocked doors — only one can be open at a time. It is the primary technical control to prevent tailgating (piggybacking).

---

**Q11.** Which hashing algorithm is currently considered broken and should NOT be used for security-critical applications?

- A) SHA-256
- B) SHA-3
- C) MD5
- D) HMAC-SHA256

**Answer: C**
**MD5** has known collision vulnerabilities — two different inputs can produce the same hash. It is considered cryptographically broken for integrity and digital signature purposes. SHA-256 and SHA-3 are current standards.

---

**Q12.** What principle does the Clark-Wilson model primarily enforce?

- A) Mandatory access control based on classification labels
- B) Integrity through well-formed transactions and separation of duties
- C) Confidentiality by preventing reads at higher classification levels
- D) Conflict of interest prevention

**Answer: B**
**Clark-Wilson** focuses on **integrity** in commercial environments through well-formed transactions (TPs modify CDIs), separation of duties, and auditing. It is the model for commercial financial systems.

---

**Q13.** An attacker monitors timing variations in encryption operations to infer the encryption key. This is an example of:

- A) Known plaintext attack
- B) Side-channel attack
- C) Birthday attack
- D) Replay attack

**Answer: B**
A **side-channel attack** exploits information gained from the physical implementation of a cryptographic system (timing, power consumption, electromagnetic emissions) rather than weaknesses in the algorithm itself. Timing attacks are a type of side-channel attack.

---

**Q14.** An organization stores private keys in specialized hardware to protect them from software-based attacks. What is this hardware called?

- A) TPM
- B) HSM (Hardware Security Module)
- C) Secure Enclave
- D) Smartcard

**Answer: B**
An **HSM (Hardware Security Module)** is a dedicated cryptographic processor designed to generate, store, and protect cryptographic keys. It enforces strict access controls and resists physical and logical attacks. HSMs are used by CAs, payment processors, and financial institutions.

---

**Q15.** Which of the following BEST describes the "fail secure" design principle?

- A) A system that maintains availability even during a failure
- B) A system that denies access by default when it fails
- C) A system that logs all failures for forensic analysis
- D) A system that automatically recovers after a failure

**Answer: B**
**Fail secure** means that when a system fails, it defaults to a **secure (denied/locked) state** rather than an open state. Example: an electronic door lock that locks (not opens) when power is cut. (vs. fail safe, which might open for life safety).

---

**Q16.** A company analyzes its symmetric encryption algorithm's performance and finds it processes 10GB of data per second. An asymmetric algorithm for the same task achieves only 0.1GB per second. This demonstrates which property?

- A) Asymmetric encryption is more secure
- B) Symmetric encryption is faster for bulk data encryption
- C) Asymmetric encryption provides better key management
- D) Symmetric encryption is less secure due to key sharing

**Answer: B**
**Symmetric encryption** (AES, 3DES) is significantly faster than asymmetric (RSA, ECC) for bulk data encryption. This is why hybrid cryptography is used: asymmetric algorithms exchange keys, then symmetric algorithms encrypt the actual data.

---

**Q17.** What is the role of an OCSP responder in PKI?

- A) Issues digital certificates to end entities
- B) Provides real-time certificate revocation status
- C) Manages the Certificate Revocation List
- D) Generates key pairs for certificate requests

**Answer: B**
**OCSP (Online Certificate Status Protocol)** provides real-time, on-demand certificate status — valid, revoked, or unknown. It is more efficient than downloading an entire CRL, especially for large PKI deployments.

---

**Q18.** According to the principle of complete mediation, which of the following statements is correct?

- A) Access decisions are cached after the first check to improve performance
- B) Every access request must be checked against the access control policy, every time
- C) Only high-privilege users require access mediation
- D) Mediation applies only to external network requests

**Answer: B**
**Complete mediation** requires that every access attempt be validated against the access control policy — not just the first time. Caching access decisions (common in OS file systems) can violate this principle.

---

**Q19.** Which emanations control standard is used by the US government to protect against electronic eavesdropping of computer equipment?

- A) FIPS 140-3
- B) TEMPEST
- C) Common Criteria
- D) ISO/IEC 27001

**Answer: B**
**TEMPEST** is a US government standard covering electromagnetic shielding requirements for equipment to prevent eavesdropping by capturing electromagnetic emanations. A Faraday cage is a physical implementation of TEMPEST principles.

---

**Q20.** An employee with access to a financial system rounds down all transactions to the nearest cent and transfers the fractional cents to a separate account. Over time, this accumulates to a significant amount. What type of attack is this?

- A) TOC/TOU attack
- B) Covert channel attack
- C) Salami attack
- D) Aggregation attack

**Answer: C**
A **salami attack** involves making many small, individually unnoticeable fraudulent actions that accumulate into significant theft. Classic example: rounding transactions and diverting fractions to a slush account.
