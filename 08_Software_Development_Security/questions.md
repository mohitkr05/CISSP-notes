# Domain 8 — Software Development Security: Practice Questions

---

**Q1.** A developer writes the following SQL query:
`"SELECT * FROM users WHERE username = '" + username + "'"` 
What vulnerability does this code contain, and what is the BEST fix?

- A) XSS vulnerability; use output encoding
- B) CSRF vulnerability; add CSRF tokens
- C) SQL injection; use parameterized queries
- D) Buffer overflow; add input length validation

**Answer: C**
The string concatenation directly embeds user input into the SQL query, creating a **SQL injection** vulnerability. The fix is **parameterized queries (prepared statements)** with bind variables, which prevent user input from being interpreted as SQL code.

---

**Q2.** An attacker stores malicious JavaScript in a web application's comment field. When other users view the page, the script executes in their browsers. What type of attack is this?

- A) Reflected XSS
- B) Stored XSS
- C) DOM-based XSS
- D) CSRF

**Answer: B**
**Stored (persistent) XSS** occurs when malicious scripts are saved to a database or server and then served to other users. Every visitor to the page receives and executes the malicious script.

---

**Q3.** Which SDLC phase should threat modeling be performed in to be MOST effective?

- A) Testing phase
- B) Deployment phase
- C) Design phase
- D) Maintenance phase

**Answer: C**
**Threat modeling** should be performed during the **design phase** — before development begins. Identifying threats early ("shift left") allows the team to design mitigations in, rather than retrofitting security after code is written.

---

**Q4.** Using the STRIDE threat model, an attacker who gains unauthorized administrator access to a system without proper credentials has exploited which threat category?

- A) Spoofing
- B) Tampering
- C) Repudiation
- D) Elevation of Privilege

**Answer: D**
**Elevation of Privilege (EoP)** in STRIDE refers to gaining access rights or permissions beyond what was originally granted. Bypassing authorization to gain admin access is a classic EoP scenario.

---

**Q5.** A security team uses a tool that sends thousands of malformed, random, and unexpected inputs to an application to discover crashes and unexpected behavior. What testing technique is this?

- A) Static Application Security Testing (SAST)
- B) Dynamic Application Security Testing (DAST)
- C) Fuzz testing
- D) Regression testing

**Answer: C**
**Fuzz testing (fuzzing)** automatically generates malformed or unexpected inputs and sends them to the application. It is effective at discovering input validation failures, buffer overflows, and crash-inducing edge cases.

---

**Q6.** A developer discovers an API key hardcoded in a public GitHub repository that was accidentally committed. What should be done FIRST?

- A) Remove the API key from the repository's commit history
- B) Immediately revoke and rotate the compromised API key
- C) Notify the development team and document the incident
- D) Set the repository to private

**Answer: B**
**Immediately revoke/rotate** the compromised key — once a secret is exposed in a public repository, it must be treated as compromised regardless of whether malicious use has been observed. Removing it from history (step A) is important but comes after revocation.

---

**Q7.** Which testing method analyzes source code without executing the application and is typically integrated into the CI/CD pipeline?

- A) DAST
- B) IAST
- C) SAST
- D) Fuzz testing

**Answer: C**
**SAST (Static Application Security Testing)** analyzes source code, bytecode, or binary without executing the application. It integrates into CI/CD pipelines (run at commit or build time) and identifies vulnerabilities like SQL injection, XSS, and insecure functions.

---

**Q8.** An organization is deploying containerized microservices. Which security control is MOST important to implement for containers?

- A) Full disk encryption on the container host
- B) Scanning container images for CVEs before deployment
- C) Installing antivirus on each container
- D) Assigning static IPs to each container

**Answer: B**
**Scanning container images for CVEs** before deployment is the most critical container security control. Containers are typically immutable — vulnerabilities must be found before they enter production. Installing AV in containers is impractical and ineffective given container design.

---

**Q9.** A multilevel security database serves users at different classification levels. For the same employee record, SECRET-cleared users see actual salary data, while UNCLASSIFIED users see a different (false) value. What database concept is this?

- A) Data classification
- B) Polyinstantiation
- C) View-based access control
- D) Data masking

**Answer: B**
**Polyinstantiation** allows multiple instances of the same data record to exist at different classification levels. Each instance contains different (potentially false) values for users below the actual classification — preventing inference attacks.

---

**Q10.** Which component of a JWT (JSON Web Token) should ALWAYS be validated server-side to prevent tampering?

- A) The header algorithm field
- B) The payload claims
- C) The signature
- D) The expiration timestamp

**Answer: C**
The **signature** must always be validated to ensure the token has not been tampered with. A known attack (`alg: none`) tricks servers into skipping signature validation. Both signature and expiration (`exp`) should be validated, but signature integrity is the primary defense.

---

**Q11.** An organization uses a Software Bill of Materials (SBOM). What is the PRIMARY security benefit?

- A) It documents the deployment architecture of the application
- B) It provides visibility into all software components and their known vulnerabilities
- C) It encrypts software artifacts in the CI/CD pipeline
- D) It tracks changes to source code over time

**Answer: B**
An **SBOM** is an inventory of all software components (libraries, frameworks, versions). Its primary security benefit is **visibility** — enabling rapid identification of which products are affected when a new CVE is disclosed (e.g., Log4Shell).

---

**Q12.** A web application lacks CSRF tokens. An attacker tricks a logged-in banking user into visiting a malicious page that submits a fund transfer request. What attack has occurred?

- A) Reflected XSS
- B) Clickjacking
- C) Cross-Site Request Forgery (CSRF)
- D) Session hijacking

**Answer: C**
**CSRF** forces an authenticated user's browser to submit an unwanted request to a trusted site. Because the browser automatically includes session cookies, the server processes the request as legitimate. CSRF tokens prevent this by requiring knowledge of a server-generated secret.

---

**Q13.** Which OWASP Top 10 category covers using third-party libraries with known security flaws?

- A) A04: Insecure Design
- B) A05: Security Misconfiguration
- C) A06: Vulnerable and Outdated Components
- D) A08: Software and Data Integrity Failures

**Answer: C**
**A06: Vulnerable and Outdated Components** specifically addresses the risk of using libraries, frameworks, and other components with known vulnerabilities. Mitigation: inventory components, monitor CVE databases, update promptly.

---

**Q14.** What is the PRIMARY advantage of IAST over SAST or DAST alone?

- A) IAST does not require access to source code
- B) IAST instruments the running application for more accurate, contextual vulnerability detection
- C) IAST is faster than SAST or DAST
- D) IAST can be run against production systems without risk

**Answer: B**
**IAST (Interactive Application Security Testing)** instruments the running application with sensors that provide real-time, contextual vulnerability detection during testing. It combines the accuracy of SAST (knows code context) with the runtime coverage of DAST, significantly reducing false positives.

---

**Q15.** A security architect is performing threat modeling for a new financial application. The team identifies that an attacker could submit a request to an internal payment service by providing a user-supplied URL. Which OWASP Top 10 threat is this?

- A) A03: Injection
- B) A07: Identification and Authentication Failures
- C) A10: Server-Side Request Forgery (SSRF)
- D) A01: Broken Access Control

**Answer: C**
**SSRF (Server-Side Request Forgery)** allows an attacker to induce the server-side application to make HTTP requests to an unintended location — including internal services, cloud metadata endpoints, or other internal network resources — by manipulating a user-supplied URL.

---

**Q16.** In a DevSecOps pipeline, at which stage should secrets scanning be implemented to be MOST effective?

- A) At the deployment stage before production release
- B) At the pre-commit or commit stage (before code is pushed)
- C) During the DAST testing phase
- D) After production deployment during runtime monitoring

**Answer: B**
Secrets scanning at the **pre-commit or commit stage** prevents secrets from ever entering the repository. Once a secret is in version control (even briefly), it may be cached by external systems. Tools like GitLeaks and truffleHog integrate as pre-commit hooks.

---

**Q17.** Which secure coding practice DIRECTLY prevents buffer overflow attacks?

- A) Output encoding
- B) CSRF tokens
- C) Parameterized queries
- D) Input length validation and bounds checking

**Answer: D**
**Input length validation and bounds checking** prevent buffer overflows by ensuring data written to a buffer does not exceed its allocated size. Compiler-level protections (stack canaries, ASLR, DEP/NX) add additional layers of defense.

---

**Q18.** An organization's security policy requires that all external software libraries be reviewed and approved before use. A developer wants to use an open-source library. Which process supports this requirement?

- A) Static Application Security Testing
- B) Software Composition Analysis (SCA)
- C) Dynamic Application Security Testing
- D) Penetration testing

**Answer: B**
**Software Composition Analysis (SCA)** tools (e.g., OWASP Dependency-Check, Snyk) identify and analyze open-source and third-party libraries for known vulnerabilities, license compliance issues, and policy violations. It directly supports the approval process for external libraries.

---

**Q19.** The Spiral model of software development is BEST suited for which type of project?

- A) Small projects with fixed, well-understood requirements
- B) Large, complex, high-risk projects requiring iterative risk analysis
- C) Projects requiring rapid deployment with frequent user feedback
- D) Projects with immutable requirements and strict documentation

**Answer: B**
The **Spiral model** is risk-driven and iterative. Each spiral iteration includes risk analysis, making it ideal for **large, complex, high-risk projects** (e.g., defense systems, nuclear plant control software) where requirements may evolve and risk management is critical.

---

**Q20.** What is the MAIN security concern with using `eval()` or `exec()` functions in web application code that processes user input?

- A) It creates a race condition vulnerability
- B) It can lead to remote code execution (command injection)
- C) It causes buffer overflows in the application
- D) It exposes the application to path traversal attacks

**Answer: B**
`eval()` and `exec()` execute arbitrary code. If user input is passed to these functions without sanitization, an attacker can inject and execute **arbitrary code on the server** — a critical Remote Code Execution (RCE) / Command Injection vulnerability. Never pass user input to dynamic execution functions.
