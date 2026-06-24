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

---

**Q21.** Which security model specifically addresses **commercial integrity** using an "Access Triple" of Subject, Program, and Object?

- A) Bell-LaPadula
- B) Biba
- C) Clark-Wilson
- D) Brewer-Nash

**Answer: C**
**Clark-Wilson** enforces commercial integrity through well-formed transactions. Access is mediated via Transformation Procedures (TPs) — users cannot directly manipulate Constrained Data Items (CDIs); they must go through approved programs. This creates the Subject → TP → CDI access triple and enforces separation of duties.

---

**Q22.** A customer specifying security requirements for a product they want evaluated under Common Criteria would produce which document?

- A) Security Target (ST)
- B) Evaluation Assurance Level (EAL)
- C) Protection Profile (PP)
- D) System Security Plan (SSP)

**Answer: C**
A **Protection Profile (PP)** is a document specifying the security requirements for a class of products — written from the customer's perspective ("what we want"). The vendor responds with a **Security Target (ST)** — their specific claims about how their product meets those requirements.

---

**Q23.** An AI security framework requires that autonomous AI agents must pause and seek human authorization before taking high-risk irreversible actions. What control concept does this represent?

- A) Least privilege for AI agents
- B) Mandatory human-in-the-loop override
- C) AI model sandboxing
- D) Differential privacy enforcement

**Answer: B**
**Mandatory human-in-the-loop (HITL) override** is an architectural control requiring that AI systems halt and obtain explicit human approval before executing actions with significant consequences (e.g., deleting data, sending communications, initiating financial transactions). This is a key 2026 AI governance control.

---

**Q24.** An attacker crafts a malicious input that causes an AI model to ignore its system prompt instructions and follow the attacker's embedded commands instead. What type of attack is this?

- A) Adversarial example attack
- B) Model inversion attack
- C) Prompt injection
- D) Data poisoning

**Answer: C**
**Prompt injection** exploits the fact that LLMs process instructions and data in the same input channel. By embedding adversarial instructions in user input (or external content the model reads), an attacker can override or bypass the system's intended behavior — analogous to SQL injection for AI systems.

---

**Q25.** Which is the highest EAL level typically achieved by commercial security products?

- A) EAL2
- B) EAL4
- C) EAL6
- D) EAL7

**Answer: B**
**EAL4 (Methodically designed, tested, and reviewed)** is the highest EAL level routinely achieved by commercial products. EAL5–EAL7 require formal mathematical verification that is extremely costly and is typically reserved for government/military systems. Most enterprise firewalls, OS components, and crypto modules are certified at EAL4.

---

**Q26.** Two AI agents communicate with each other as part of an automated pipeline. One agent's output is used as input to the next. What security principle should govern how Agent B treats Agent A's output?

- A) Treat it as trusted since both agents are internal
- B) Treat it as untrusted external input requiring validation
- C) Apply Bell-LaPadula's no-read-up rule between agents
- D) Encrypt the channel between agents and consider it secure

**Answer: B**
In multi-agent architectures, each agent's output must be treated as **untrusted input** by the receiving agent — regardless of whether it originated internally. A compromised or hallucinating Agent A could inject malicious content. This is the AI equivalent of treating all inter-process communication with the same scrutiny as external user input.

---

**Q27.** An attacker parks a vehicle outside a government facility and uses specialized antennas to capture electromagnetic emissions from computer monitors inside, reconstructing displayed data. What type of attack is this?

- A) Covert storage channel
- B) Side-channel timing attack
- C) TEMPEST / Van Eck phreaking
- D) Covert timing channel

**Answer: C**
**TEMPEST / Van Eck phreaking** exploits electromagnetic emanations from computing equipment to reconstruct data without any physical or network access. Countermeasures include TEMPEST-certified hardware, Faraday cage shielding, and switching from CRT to LCD displays (lower emissions).

---

**Q28.** A system checks a file's permissions before opening it. Between the permission check and the file open operation, an attacker replaces the file with a different one. What vulnerability is exploited?

- A) Covert storage channel
- B) Time of Check / Time of Use (TOC/TOU)
- C) Salami attack
- D) Maintenance hook

**Answer: B**
**TOC/TOU (Time of Check / Time of Use)** is a race condition where the state of a resource changes between when it is checked and when it is used. The attacker exploits the window between the permission check and the actual file operation. Prevention: atomic operations, file locking.

---

**Q29.** Which cloud hypervisor type runs directly on hardware (bare-metal) and is considered more secure than its alternative?

- A) Type 1 — bare-metal hypervisor (VMware ESXi, Hyper-V)
- B) Type 2 — hosted hypervisor (VirtualBox, VMware Workstation)
- C) Type 3 — containerized hypervisor
- D) Type 0 — firmware-based hypervisor

**Answer: A**
**Type 1 (bare-metal) hypervisors** run directly on hardware with no host OS layer — the attack surface is smaller because there is no underlying OS to compromise. **Type 2 (hosted) hypervisors** run on top of a general-purpose OS, which adds attack surface: compromising the host OS can compromise all VMs.

---

**Q30.** In a cloud shared responsibility model, who is responsible for patching the operating system on an IaaS (Infrastructure as a Service) virtual machine?

- A) The cloud provider
- B) The customer
- C) It is shared equally between provider and customer
- D) A third-party managed service provider by default

**Answer: B**
In **IaaS**, the customer is responsible for everything above the hypervisor — including the **OS, middleware, applications, and data**. The cloud provider manages the physical hardware, network, and hypervisor. Failing to patch a customer-deployed OS on an EC2 instance is the customer's responsibility and liability.
