# Domain 3 — Security Architecture and Engineering

**Exam Weight: 13%**

---

## 3.1 Secure Design Principles

| Principle | Description |
|-----------|-------------|
| **Least Privilege** | Grant minimum access rights necessary |
| **Separation of Duties** | No single person controls an entire process |
| **Defense in Depth** | Multiple layers of security controls |
| **Fail Secure / Fail Safe** | On failure, default to secure state (deny access) |
| **Keep It Simple** | Complex systems have more vulnerabilities |
| **Zero Trust** | Never trust, always verify; assume breach |
| **Secure Defaults** | Out-of-the-box configuration is secure |
| **Open Design** | Security should not rely on secrecy of design (vs. security through obscurity) |
| **Complete Mediation** | Every access request is checked against access control policy |
| **Psychological Acceptability** | Security mechanisms should not be so burdensome users bypass them |
| **Economy of Mechanism** | Design should be as simple as possible |
| **Least Common Mechanism** | Minimize shared mechanisms between users |

> **Exam Tip:** "Fail secure" = fails in a secure (locked/denied) state. A door that locks when power fails is fail secure. A door that opens when power fails is fail safe (life safety priority). Context matters.

> **Scenario — Fail Secure vs. Fail Safe — Context Changes Everything:**
> Same door, two different contexts:
> - **Server room door during power failure:** Should **lock** (fail secure). No one gets in. Unauthorized access is the bigger risk than inconvenience.
> - **Emergency exit door in a building fire alarm:** Should **unlock** (fail safe). Life safety is the priority. People must be able to escape.
>
> **Exam trap:** A question might say "a bank vault door fails open when power is cut." Is this fail secure or fail safe? Fail safe — it prioritizes life safety (people trapped in vaults). But is it a good security design? No — an attacker could cut the power to open the vault. This is why real vaults use battery-powered locks (fail secure) that remain locked even during power outages.
>
> **The rule:** When the question is about *information security*, default to **fail secure**. When life safety is explicitly mentioned (fire, emergency), **fail safe** wins.

> **Scenario — Psychological Acceptability in Real Life:**
> A company implements a new VPN that requires a hardware token, a 30-character password, and a second approval from a manager every time employees need to access email from home. Within a week, employees are forwarding work emails to personal Gmail accounts to bypass the friction. This is a **Psychological Acceptability** failure — the security mechanism was so burdensome that users bypassed it entirely, creating a worse outcome than a simpler control would have.
>
> **Zero Trust vs. Complete Mediation distinction:** Zero Trust is an architectural philosophy ("never trust, always verify"). Complete Mediation is a design principle — specifically, every single access request to every single object must be checked against the access control policy, with no cached or assumed permissions.

---

## 3.2 Security Models

Security models formalize security policies mathematically.

### Bell-LaPadula Model (Confidentiality)
Created for the US military. Focus: **prevent unauthorized disclosure**.

| Rule | Description | Mnemonic |
|------|-------------|---------|
| **Simple Security Property** (ss-property) | **No Read Up** — subject cannot read object at higher classification | "No read up" |
| **Star Property** (*-property) | **No Write Down** — subject cannot write to object at lower classification | "No write down" |
| **Strong Tranquility** | Security labels don't change during system operation | |

> "A general cannot read TOP SECRET docs (below clearance) — wait, reverse. A SECRET-cleared user **cannot read** TOP SECRET (no read up) but **can write up** to TOP SECRET, and **cannot write down** to UNCLASSIFIED."

> **Scenario — Bell-LaPadula "No Write Down" Explained:**
> A TOP SECRET-cleared analyst is writing a report summarizing intelligence that is classified TOP SECRET. Bell-LaPadula's **star property** says she CANNOT write this report into an UNCLASSIFIED document (no write down) — that would declassify TOP SECRET information into a lower-sensitivity container. She can, however, write a TOP SECRET document upward to a higher classification if one exists.
>
> **Why "No Write Down"?** Imagine a spy who has TOP SECRET clearance. Without this rule, they could read the TOP SECRET file and then write its contents into a PUBLIC Word document — effectively leaking classified data downward. The no-write-down rule closes that covert channel.
>
> **The counterintuitive part:** A SECRET-cleared user CAN write *up* to TOP SECRET. This sounds wrong but makes sense — writing up doesn't leak information downward. The model is purely concerned with preventing downward disclosure.

### Biba Model (Integrity)
Opposite of Bell-LaPadula. Focus: **prevent unauthorized modification**.

| Rule | Description |
|------|-------------|
| **Simple Integrity Property** | **No Read Down** — subject cannot read object at lower integrity level |
| **Star Integrity Property** | **No Write Up** — subject cannot write to object at higher integrity level |

> **Mnemonic — Bell-LaPadula vs. Biba:**
> - Bell-LaPadula: "No read up, no write down" → **Confidentiality**
> - Biba: "No read down, no write up" → **Integrity**

> **Scenario — Biba "No Read Down / No Write Up" Explained:**
> Think of integrity levels as **trust levels** — High integrity = highly trusted source; Low integrity = untrusted source (like the internet).
>
> **No Read Down:** A nuclear power plant's control system runs at HIGH integrity. An operator cannot read data from a LOW integrity source (a public internet feed) to make decisions on the control system. Reading low-integrity data could corrupt the high-integrity process — like taking random internet advice to adjust reactor pressure.
>
> **No Write Up:** A contractor working on a project at LOW integrity clearance cannot write reports directly into the HIGH integrity executive briefing system. Their data hasn't been validated — allowing it to "write up" would contaminate a trusted system with potentially unreliable data.
>
> **Bell-LaPadula vs. Biba Side by Side:**
> | | Can Read Up? | Can Write Down? |
> |---|---|---|
> | Bell-LaPadula (Confidentiality) | NO | NO |
> | Biba (Integrity) | NO | NO — but in the opposite direction |
>
> They seem similar but protect opposite things: BLP prevents information leakage downward; Biba prevents data corruption from below.

### Clark-Wilson Model (Integrity)
Focuses on well-formed transactions and separation of duties in commercial environments.

| Component | Description |
|-----------|-------------|
| **CDI** (Constrained Data Item) | Sensitive data whose integrity must be protected |
| **UDI** (Unconstrained Data Item) | Input data not yet validated |
| **IVP** (Integrity Verification Procedure) | Validates CDI is in consistent state |
| **TP** (Transformation Procedure) | Authorized program that manipulates CDI |

Access rules: Only TPs can modify CDIs. Users access CDIs only through TPs.

> **Scenario — Clark-Wilson in Banking:**
> A bank's accounting system uses Clark-Wilson principles:
> - **CDI** = account balances (must stay accurate and consistent)
> - **UDI** = a teller's raw input at the counter ("customer wants to withdraw $500")
> - **IVP** = an automated check that verifies account balances sum to zero (credits = debits)
> - **TP** = the "Process Withdrawal" program that validates the request, checks available balance, updates the account, and logs the transaction
>
> A teller cannot directly edit account balance fields — they can only submit withdrawal requests through the approved TP. The TP enforces rules (no overdraft, valid amount, proper authorization) before touching the CDI. This is exactly Clark-Wilson's principle: controlled programs mediating access to sensitive data with **well-formed transactions**.

> **Scenario — Brewer-Nash (Chinese Wall) in Consulting:**
> An analyst at a consulting firm has worked on an engagement for Coca-Cola (FMCG sector). She is now offered an engagement for PepsiCo (same sector — direct competitor). Brewer-Nash says she **cannot** access PepsiCo's confidential data because she already has Coca-Cola's sensitive information. Working on both would create a conflict of interest — knowledge from one could (even unconsciously) influence work for the other.
>
> The model dynamically builds "conflict classes." Once you access data from Company A, the entire competing class becomes off-limits to you — no matter your job title or formal clearance level.

### Brewer-Nash Model (Chinese Wall)
Prevents **conflicts of interest**. A user who has accessed information from one organization cannot access competing organization's data. Used in investment banking and consulting.

### Other Models

| Model | Purpose |
|-------|---------|
| **Graham-Denning** | Defines 8 primitive rights for subjects/objects |
| **Take-Grant** | Models how access rights are transferred between subjects |
| **Lattice Model** | Defines upper/lower bounds of access between subjects and objects |
| **Non-Interference** | Ensures high-security actions do not affect low-security state |

---

## 3.3 Trusted Computing Base (TCB)

**TCB** = everything in a system that is trusted to enforce the security policy (hardware, firmware, software).

| Component | Description |
|-----------|-------------|
| **Security Kernel** | Core of TCB; enforces access control |
| **Reference Monitor** | Abstract concept; mediates all access requests; must be tamperproof, always invoked, verifiable |
| **Trusted Platform Module (TPM)** | Hardware chip; stores cryptographic keys, measures boot integrity |
| **Secure Boot** | Verifies bootloader and OS integrity using TPM |

### TCSEC (Orange Book) Evaluation Levels

| Level | Description |
|-------|-------------|
| D | Minimal protection |
| C1 | Discretionary protection |
| C2 | Controlled access protection (audit trails) |
| B1 | Labeled security (mandatory access control) |
| B2 | Structured protection (formal model) |
| B3 | Security domains (reference monitor) |
| A1 | Verified design (formal verification) |

### Common Criteria (CC) — ISO/IEC 15408

**EAL (Evaluation Assurance Levels):** EAL1 (lowest) to EAL7 (highest formal verification).

| EAL | Description |
|-----|-------------|
| EAL1 | Functionally tested |
| EAL2 | Structurally tested |
| EAL3 | Methodically tested and checked |
| EAL4 | Methodically designed, tested, reviewed (most common for commercial products) |
| EAL5 | Semiformally designed and tested |
| EAL6 | Semiformally verified design and tested |
| EAL7 | Formally verified design and tested |

**PP (Protection Profile)** — defines security requirements for a product class.
**ST (Security Target)** — vendor's specific security claims for their product.

---

## 3.4 Cryptography

### Symmetric Encryption
Uses the **same key** for encryption and decryption. Fast; key distribution is a challenge.

| Algorithm | Key Size | Block Size | Notes |
|-----------|----------|------------|-------|
| **AES** | 128/192/256-bit | 128-bit | Current standard; FIPS 197 |
| **3DES** | 168-bit (effective 112) | 64-bit | Legacy; being phased out |
| **DES** | 56-bit | 64-bit | Broken; do not use |
| **Blowfish** | 32-448 bit | 64-bit | Fast; free |
| **RC4** | Variable | Stream cipher | Broken; deprecated (WEP, TLS 1.0) |
| **Twofish** | 128/192/256-bit | 128-bit | AES finalist |

### Asymmetric Encryption
Uses **key pairs** (public + private). Slow; solves key distribution. Public key encrypts; private key decrypts (and vice versa for signing).

| Algorithm | Use | Key Size |
|-----------|-----|----------|
| **RSA** | Encryption, digital signatures | 2048-bit+ recommended |
| **ECC** | Encryption, signatures (smaller keys) | 256-bit ≈ RSA 3072-bit |
| **Diffie-Hellman** | Key exchange (not encryption) | 2048-bit+ |
| **DSA** | Digital signatures only | 1024-3072 bit |
| **El Gamal** | Encryption, signatures | Large keys |

> **Exam Tip:** **Diffie-Hellman** is used for **key exchange**, not encryption. It allows two parties to agree on a shared secret over an insecure channel without transmitting the key itself.

> **Scenario — Diffie-Hellman Analogy (Paint Mixing):**
> Alice and Bob each have a secret color (private key). They agree on a common public color (yellow — the public parameter). Alice mixes yellow with her secret color → sends the mixture to Bob. Bob mixes yellow with his secret color → sends his mixture to Alice. Each then mixes the received mixture with their own secret color. They both end up with the same final color — their shared secret — without ever sharing their individual secret colors. An eavesdropper can see the public yellow and both sent mixtures, but cannot reconstruct the final color without solving an infeasible math problem (discrete logarithm). This is DH key exchange.
>
> **Why DH ≠ Encryption:** DH establishes a shared key. You then use that shared key with a symmetric algorithm (like AES) to actually encrypt data. DH by itself produces a key; it doesn't encrypt any message.

> **Scenario — Digital Signatures vs. Encryption (the #1 most-confused topic):**
> Alice wants to send Bob a signed message proving she wrote it:
> 1. Alice hashes her message → gets a digest "abc123"
> 2. Alice encrypts "abc123" with her **PRIVATE** key → this is the digital signature
> 3. Bob receives (message + signature)
> 4. Bob decrypts the signature with Alice's **PUBLIC** key → gets "abc123"
> 5. Bob independently hashes the message → gets "abc123"
> 6. They match → message wasn't altered, and only Alice could have signed it (she's the only one with her private key)
>
> **Alice wants to send Bob a CONFIDENTIAL message:**
> 1. Alice encrypts with Bob's **PUBLIC** key
> 2. Only Bob's **PRIVATE** key can decrypt it
>
> **The trick:** Signing uses YOUR OWN private key. Encrypting for someone uses THEIR public key. The exam will mix these up to see if you know the difference.

### Hashing
One-way function. Used for integrity verification, passwords, digital signatures.

| Algorithm | Output Size | Status |
|-----------|------------|--------|
| **MD5** | 128-bit | Broken (collision attacks) |
| **SHA-1** | 160-bit | Weak (deprecated) |
| **SHA-256** | 256-bit | Current standard |
| **SHA-3** | Variable | Next-generation |
| **HMAC** | Varies | Hash + secret key = authenticated integrity |
| **bcrypt** | Variable | Password hashing with salt |

**Birthday Attack** targets hash collisions. Probability of collision is ~50% after `2^(n/2)` attempts (where n = hash output bits).

### Block Cipher Modes of Operation

| Mode | Description | Issue |
|------|-------------|-------|
| **ECB** (Electronic Codebook) | Each block encrypted independently | Same plaintext = same ciphertext; never use |
| **CBC** (Cipher Block Chaining) | Each block XORed with previous ciphertext | Needs IV; sequential |
| **CFB** (Cipher Feedback) | Converts block cipher to stream cipher | Self-synchronizing |
| **OFB** (Output Feedback) | Uses cipher output as keystream | No error propagation |
| **CTR** (Counter) | Uses counter as nonce | Parallelizable; good for streaming |
| **GCM** (Galois/Counter Mode) | CTR + authentication | Provides AEAD (preferred for TLS) |

> **Exam Tip:** **ECB** is the only mode where identical plaintexts produce identical ciphertexts — this is a fatal flaw. Always avoid ECB.

> **Scenario — Why ECB Is Broken (the Linux Tux Penguin example):**
> Encrypt the famous Linux Tux penguin image using AES-ECB. Because ECB encrypts each 128-bit block independently and identically, areas of the image with the same color (large flat sections) produce the same ciphertext blocks. The output still clearly shows the silhouette of the penguin — you can see the shape through the "encrypted" image. This visualizes why ECB reveals patterns in the plaintext.
>
> **CBC** prevents this by XORing each block with the previous ciphertext before encrypting. Even identical plaintext blocks produce completely different ciphertext because they're XORed with different previous ciphertext.
>
> **GCM exam note:** When you see "AEAD" (Authenticated Encryption with Associated Data) on the exam, think **GCM**. It provides both confidentiality (CTR mode) AND integrity/authentication (GHASH) in one operation — no separate HMAC needed. This is why TLS 1.3 uses AES-256-GCM.

### Public Key Infrastructure (PKI)

| Component | Role |
|-----------|------|
| **CA (Certificate Authority)** | Issues and signs digital certificates |
| **RA (Registration Authority)** | Verifies identity before CA issues certificate |
| **CRL (Certificate Revocation List)** | List of revoked certificates; published by CA |
| **OCSP (Online Certificate Status Protocol)** | Real-time certificate status check |
| **Repository / LDAP** | Stores certificates and CRLs |

**X.509 Certificate** contains: subject, issuer, public key, validity period, serial number, signature algorithm, CA signature.

**Certificate Types:**
- **DV** (Domain Validated) — basic, proves domain ownership
- **OV** (Organization Validated) — verifies organization identity
- **EV** (Extended Validation) — highest, shows organization in browser bar
- **Wildcard** — covers all subdomains (*.example.com)
- **SAN/UCC** — multiple domains in one certificate

### Key Management

| Phase | Controls |
|-------|---------|
| **Generation** | Use approved algorithms and key lengths; hardware RNG |
| **Distribution** | Secure channel; asymmetric key wrapping |
| **Storage** | HSM (Hardware Security Module); encrypted key store |
| **Rotation** | Regular replacement; limits exposure window |
| **Revocation** | CRL, OCSP; immediate on compromise |
| **Destruction** | Secure deletion; crypto-shredding |
| **Escrow** | Third-party key recovery for law enforcement or disaster recovery |

**Steganography** — hiding data inside other data (e.g., image files). Not encryption; relies on obscurity.
**Watermarking** — embeds ownership/origin information into media for rights management.

> **Scenario — Steganography vs. Encryption:**
> A corporate spy wants to exfiltrate trade secrets. Option 1: Encrypt the data (obvious — a large encrypted file going out will trigger DLP alerts). Option 2: Hide the text inside the least-significant bits of a cat photo — the photo still looks like a normal photo, DLP sees an image file and lets it through. This is steganography — the *existence* of the secret is hidden, not just the content.
>
> **Watermarking** = the opposite intent. A movie studio embeds an invisible serial number in every copy of a film distributed to reviewers. When a pirated copy leaks, the studio extracts the watermark and identifies exactly which reviewer's copy was leaked. The data isn't secret — its presence is intentional — it's about traceability and rights management, not hiding information.

---

## 3.5 Security Architecture Frameworks

| Framework | Description |
|-----------|-------------|
| **TOGAF** | The Open Group Architecture Framework; enterprise architecture |
| **Zachman** | Framework for enterprise architecture taxonomy |
| **SABSA** | Sherwood Applied Business Security Architecture; risk-driven |
| **DoDAF** | US Dept. of Defense Architecture Framework |

---

## 3.6 Physical Security

### Perimeter Controls (outer to inner)

```
Fencing → Lighting → Guards → CCTV → Gates/Bollards → Building
```

| Control | Purpose |
|---------|---------|
| **Fencing** | Perimeter deterrent; 8ft+ with barbed wire for high security |
| **Bollards** | Vehicle barriers (prevent ramming) |
| **Lighting** | Deters crime; minimum 2 foot-candles at perimeter |
| **CCTV** | Surveillance; detective control |
| **Guards** | Adaptive, can respond to incidents |
| **Dogs** | Highly effective deterrent; 15x better than guards alone |

### Facility Access Controls

| Control | Description |
|---------|-------------|
| **Mantrap (airlock)** | Two-door entry; prevents tailgating; one door must close before the other opens |
| **Turnstiles** | One person per authentication |
| **Badge systems** | Proximity cards, smart cards |
| **Visitor logs** | Record all visitor access |
| **Biometrics** | High-assurance physical access |

> **Tailgating (piggyback)** — unauthorized person follows authorized person through secure door. Mitigation: mantrap, security guards, user training.

### Environmental Controls

| System | Purpose |
|--------|---------|
| **HVAC** | Temperature/humidity control (65-75°F, 40-60% humidity) |
| **UPS (Uninterruptible Power Supply)** | Short-term power continuity |
| **Generator** | Long-term power backup |
| **Fire Detection** | Smoke/heat detectors; early warning |
| **Fire Suppression** | Wet pipe, dry pipe, Halon alternatives (FM-200, CO2) |
| **EMI Shielding (Faraday Cage)** | Protects against electromagnetic interference and eavesdropping |

**Fire Suppression Types:**
- **Class A** — ordinary combustibles: water
- **Class B** — flammable liquids: CO2, dry chemical
- **Class C** — electrical: CO2, dry chemical (NOT water)
- **Class D** — metals: dry powder
- **Class K** — kitchen: wet chemical

> **Exam Tip:** **Halon** is effective but environmentally harmful (ozone-depleting). Replacements: FM-200 (HFC-227ea), CO2, Inergen. **Never use water on electrical fires (Class C).**

> **Scenario — Fire Suppression in a Data Center:**
> A data center fire breaks out in the UPS battery room. The fire is electrical (Class C). The automated sprinkler system (wet pipe, water) should NOT activate — water on live electrical equipment causes electrocution and shorts equipment. The correct suppression system is **dry chemical or CO2** for electrical fires.
>
> For the main server floor, the preferred system is **FM-200 or Inergen** (clean agent) — suppresses fire without leaving residue on equipment and without oxygen displacement hazards of CO2 in occupied spaces. Halon was perfect for this but is banned under the Montreal Protocol due to ozone depletion.
>
> **CO2 caution:** CO2 displaces oxygen — it will suppress the fire AND suffocate any humans in the room. CO2 suppression in occupied spaces requires evacuation alarms and time delays. Never the first choice if people might be present.

---

## 3.7 Common Vulnerabilities in Architectures

| Vulnerability | Description |
|---------------|-------------|
| **Covert Storage Channel** | Unauthorized data transmission via storage mechanisms |
| **Covert Timing Channel** | Unauthorized data transmission by modulating timing of events |
| **TOC/TOU (Time of Check/Time of Use)** | Race condition where state changes between check and use |
| **Emanations** | Electromagnetic signals that can reveal information (TEMPEST countermeasure) |
| **Maintenance Hooks (Backdoors)** | Undocumented system access left by developers |
| **Salami Attack** | Small, unnoticeable fraud increments that accumulate |
| **Aggregation** | Combining low-sensitivity data to create high-sensitivity information |
| **Inference** | Deriving sensitive data from permitted queries |

> **Scenario — Aggregation vs. Inference (database security traps):**
>
> **Aggregation:** A database analyst has access to:
> - Employee names (Unclassified)
> - Employee work locations (Unclassified)
> - Employee schedule/shift times (Unclassified)
> - Employee vehicle descriptions (Unclassified)
>
> Individually, each piece of data is harmless. But combined, an analyst could identify that "John Smith, drives a blue Honda, parks in Lot C, arrives 6 AM" — now targeting a high-value government employee is trivially easy. The **aggregated** data is far more sensitive than any individual piece.
>
> **Inference:** A researcher queries a medical database: "How many patients over 60 in ZIP code 12345 have HIV?" The system returns "1." From this answer, the researcher infers the identity of that one patient (if they know who the only person over 60 with HIV in that ZIP code is). No single query was unauthorized — but the answer revealed protected information. Solution: **cell suppression** (don't return results below a minimum count) and **query restriction**.
>
> **TOC/TOU (Time of Check/Time of Use) Scenario:**
> A bank's ATM checks your account balance ($500) and approves a $400 withdrawal (Time of Check). Between the check and the actual debit posting (Time of Use), a race condition allows a second simultaneous ATM transaction for $300 to also pass the check (balance was still $500). Result: you withdraw $700 total from a $500 account. ATM vendors prevent this with **atomic transactions** and database locks.

---

## 3.8 Cryptographic Attacks

| Attack | Description | Countermeasure |
|--------|-------------|---------------|
| **Brute Force** | Try all possible keys | Longer key length; slow hash functions |
| **Dictionary Attack** | Try common words/passwords | Salting; long passphrases |
| **Birthday Attack** | Find hash collisions | Longer hash output (SHA-256+) |
| **Rainbow Table** | Pre-computed hash lookups | Salting passwords |
| **Man-in-the-Middle** | Intercept key exchange | PKI; certificate validation |
| **Replay Attack** | Capture and re-send valid messages | Timestamps; nonces; sequence numbers |
| **Side-Channel Attack** | Analyze power/timing/EM emissions | Physical shielding; constant-time algorithms |
| **Chosen-Plaintext Attack (CPA)** | Attacker encrypts chosen plaintexts | Semantic security; random IVs |
| **Padding Oracle Attack** | Exploit error messages about padding | Authenticated encryption (GCM); constant-time responses |

### Key Length Recommendations (current standards)
| Algorithm | Minimum Key Length | Notes |
|-----------|-------------------|-------|
| AES symmetric | 128-bit (256 recommended) | FIPS 197 |
| RSA asymmetric | 2048-bit (3072 preferred) | NIST SP 800-131A |
| ECC | 256-bit | Equivalent to RSA 3072 |
| SHA hashing | 256-bit (SHA-256) | SHA-1 deprecated |
| DH key exchange | 2048-bit | IKEv2 standard |

---

## 3.9 Digital Signatures — Exam Critical

### How Digital Signatures Work
```
Sender:
  1. Hash the message → message digest
  2. Encrypt digest with SENDER'S PRIVATE KEY → digital signature
  3. Send (message + signature)

Receiver:
  1. Decrypt signature with SENDER'S PUBLIC KEY → digest
  2. Hash received message → computed digest
  3. Compare both digests — if equal, signature is valid
```

**Provides:** Authentication + Non-repudiation + Integrity (NOT confidentiality)

> **Exam Tip — Critical distinction:**
> - **Encryption for confidentiality:** Encrypt with recipient's PUBLIC key; decrypt with recipient's PRIVATE key
> - **Digital signature:** Sign with sender's PRIVATE key; verify with sender's PUBLIC key

### Certificate Validation Process
```
Receive certificate → Check expiry → Check CRL/OCSP → Verify CA signature → Check revocation → Accept/Reject
```

---

## 3.10 Cloud and Virtualization Security Architecture

### Hypervisor Types
| Type | Description | Security |
|------|-------------|---------|
| **Type 1 (Bare-metal)** | Runs directly on hardware (VMware ESXi, Hyper-V) | More secure; smaller attack surface |
| **Type 2 (Hosted)** | Runs on top of OS (VMware Workstation, VirtualBox) | Less secure; host OS is attack vector |

### VM Escape
Attacker breaks out of VM to access hypervisor or other VMs. Severe — compromises hypervisor isolation.
**Mitigations:** Keep hypervisor patched; minimal attack surface on host; network isolation.

### Cloud Service Models — Security Responsibilities
| Service Model | Provider Manages | Customer Manages |
|---------------|-----------------|-----------------|
| **IaaS** | Hardware, hypervisor, networking | OS, applications, data |
| **PaaS** | Hardware, OS, middleware | Applications, data |
| **SaaS** | Everything except data | Data configuration, access control |

> **Exam Tip:** Shared Responsibility Model — in IaaS you are responsible for patching the OS. In SaaS, you are responsible for data and user access management, nothing else.

> **Scenario — Cloud Shared Responsibility by Service Model:**
> A ransomware attack encrypts data across three different systems:
> - **IaaS (e.g., AWS EC2):** You deployed a Windows VM and never patched it. The attacker exploited an unpatched OS vulnerability. **Your fault** — in IaaS, patching the OS is your responsibility.
> - **PaaS (e.g., AWS RDS):** AWS manages the database engine and OS. The attacker exploited a vulnerability in your application code that ran on top of the managed DB. **Your fault** — application code is customer responsibility in PaaS.
> - **SaaS (e.g., Microsoft 365):** A user was given global admin access they didn't need and used it to delete all email. **Your fault** — in SaaS, user access management is the customer's responsibility. Microsoft's platform was secure; you misconfigured permissions.
>
> **The pattern:** As you move from IaaS → PaaS → SaaS, your responsibility shrinks — but you ALWAYS own your data and who has access to it.

> **Scenario — VM Escape (Why It's Critical):**
> An attacker compromises a web app running in VM-A on a shared cloud host. Through a hypervisor vulnerability, they "escape" the VM boundary and gain access to the host hypervisor. From the hypervisor, they can read memory of VM-B (a different customer's VM), inject code into it, or shut it down. One compromised tenant now affects ALL tenants on the same physical host. This is why hypervisor patching is the cloud provider's most critical security responsibility — and why Type 1 (bare-metal) hypervisors have a smaller attack surface than Type 2 (hosted).

### Cloud Deployment Models
| Model | Description |
|-------|-------------|
| **Public** | Shared infrastructure; multi-tenant |
| **Private** | Dedicated to one organization |
| **Community** | Shared by organizations with common interests |
| **Hybrid** | Combination of public and private |

---

## 3.11 Covert Channels and TEMPEST

- **Covert Storage Channel:** Data leaked by modifying a shared storage resource (e.g., writing data to a file that another process reads as a signal). Hard to detect — doesn't use normal communication paths.
- **Covert Timing Channel:** Data leaked by modulating timing of operations (e.g., process A holds a mutex for 1 second = signal "1", releases instantly = signal "0"). Extremely difficult to eliminate entirely.
- **TEMPEST:** Countermeasures against electromagnetic emanation eavesdropping. Monitors and equipment emit EM signals that can be intercepted to reconstruct data (Van Eck phreaking).
  - **Faraday Cage:** Metal enclosure that blocks external EM fields and prevents EM signals from escaping.
  - **TEMPEST-certified equipment:** Shielded hardware that limits emanations to NIST/NSA-approved levels.

> **Tricky Scenario:** An attacker parks outside a secure government building and uses specialized equipment to read the data being displayed on CRT monitors inside by capturing the EM emissions through the walls. What type of attack is this?
> **Answer: TEMPEST / Van Eck phreaking** — exploiting electromagnetic emanations. The countermeasure is TEMPEST-certified hardware, Faraday shielding, or using LCD/flat-panel monitors (lower EM emissions than CRTs). This is a covert channel attack that bypasses all network security controls entirely.

---

## 3.11 Exam Tips Summary

- **Bell-LaPadula** = Confidentiality (no read up, no write down).
- **Biba** = Integrity (no read down, no write up).
- **Clark-Wilson** = Commercial integrity via well-formed transactions.
- **Brewer-Nash (Chinese Wall)** = conflict-of-interest prevention.
- **ECB mode** is never secure — always avoid.
- **DH** is for key **exchange**, not encryption.
- **RSA** encrypts with public key; decrypts with private key.
- Digital signatures: sign with **private** key; verify with **public** key.
- **EAL4** is the most common level for commercial security products.
- Physical security is the first line of defense — if an attacker has physical access, all other controls can fail.
- Fire suppression: never water on electrical (Class C) fires.
- **Type 1 hypervisor** (bare-metal) is more secure than Type 2 (hosted).
- **Birthday attack** targets hash collisions — mitigation is longer hash output.
- **Salting** defeats rainbow table attacks — even identical passwords produce different hashes.
- **HMAC** = hashing + secret key = both integrity AND authentication (unlike plain hashing).
