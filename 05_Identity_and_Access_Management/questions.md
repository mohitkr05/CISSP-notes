# Domain 5 — Identity and Access Management: Practice Questions

---

**Q1.** A user logs in with their password and then receives a push notification on their mobile phone to approve. Which type of authentication is being used?

- A) Single-factor authentication
- B) Multi-factor authentication
- C) Step-up authentication
- D) Two-step verification using the same factor category

**Answer: B**
**Multi-factor authentication (MFA)** uses two or more factors from different categories. Password = "something you know" and phone push approval = "something you have" — two different factor categories.

---

**Q2.** A biometric system's CER is 0.01%. Another system has a CER of 0.5%. Which system is MORE accurate?

- A) The system with 0.5% CER
- B) Both systems are equally accurate
- C) The system with 0.01% CER
- D) CER cannot be used to compare biometric systems

**Answer: C**
**Lower CER (Crossover Error Rate) = more accurate biometric system**. The CER is where FAR equals FRR — the lower this crossover point, the better the system performs at distinguishing legitimate users from impostors.

---

**Q3.** A Windows file server allows the file owner to grant read access to specific colleagues by modifying the file's access control list. Which access control model does this represent?

- A) MAC
- B) RBAC
- C) DAC
- D) ABAC

**Answer: C**
**Discretionary Access Control (DAC)** lets resource owners control access at their discretion by modifying ACLs. Windows NTFS permissions are a classic DAC implementation — the owner can grant/revoke permissions as they choose.

---

**Q4.** An organization needs to implement access control that automatically grants or denies access based on the user's department, time of day, device compliance status, and the data classification of the resource being accessed. Which model is MOST appropriate?

- A) RBAC
- B) MAC
- C) DAC
- D) ABAC

**Answer: D**
**ABAC (Attribute-Based Access Control)** evaluates multiple attributes of the subject (department), object (data classification), and environment (time, device health) to make fine-grained access decisions. RBAC alone cannot handle this level of contextual, multi-attribute policy.

---

**Q5.** After authenticating to a corporate portal, a user can access email, HR systems, and project management tools without logging in again. What is this capability called?

- A) Federated identity
- B) Multi-factor authentication
- C) Single Sign-On (SSO)
- D) OAuth delegation

**Answer: C**
**Single Sign-On (SSO)** allows a user to authenticate once and access multiple systems without re-authenticating. The user's session token or ticket is silently presented to each resource.

---

**Q6.** In Kerberos, an attacker obtains the hash of the KRBTGT account from a compromised domain controller and uses it to forge TGTs for any user. What attack is this?

- A) Silver Ticket attack
- B) Pass-the-Hash attack
- C) Golden Ticket attack
- D) Kerberoasting

**Answer: C**
A **Golden Ticket** attack involves forging TGTs (Ticket Granting Tickets) using the KRBTGT account's password hash. The attacker can create tickets for any user, including domain administrators, with any expiration time — full domain compromise.

---

**Q7.** A network engineer configures Cisco routers to use centralized authentication where accounting records all commands entered. The organization requires full packet encryption for authentication. Which protocol should be used?

- A) RADIUS
- B) TACACS+
- C) Kerberos
- D) LDAP

**Answer: B**
**TACACS+** is the correct choice for device administration. It encrypts the **entire authentication packet** (not just the password), uses TCP for reliability, and separately handles Authentication, Authorization, and Accounting — allowing granular command-level authorization logging.

---

**Q8.** An employee transfers from the Finance department to Engineering. Three months later, an access review reveals the employee still has Finance system access in addition to Engineering access. What is this condition called?

- A) Separation of duties violation
- B) Access creep (privilege accumulation)
- C) Least privilege violation
- D) Both B and C

**Answer: D**
This is **access creep** (accumulation of access rights over time, especially after role changes) AND a **least privilege violation** (the user has more access than their current role requires). Both terms apply and both are correct answers — D is most complete.

---

**Q9.** A user's SAML assertion is intercepted after authentication and replayed by an attacker to gain unauthorized access. How does SAML prevent this type of attack?

- A) Encrypting the assertion with the user's public key
- B) Including a short validity window (NotBefore/NotOnOrAfter) and assertion ID with replay prevention
- C) Requiring the user to re-authenticate for each service
- D) Using JWT tokens instead of XML assertions

**Answer: B**
SAML assertions include **time-bound validity conditions** (NotBefore/NotOnOrAfter) and a unique assertion ID. Recipients must reject assertions outside the time window and track used assertion IDs to prevent replay attacks.

---

**Q10.** A mobile app wants to access a user's Google Drive without the user sharing their Google password. Which protocol enables this?

- A) Kerberos
- B) SAML
- C) OAuth 2.0
- D) LDAP

**Answer: C**
**OAuth 2.0** is an authorization delegation framework that allows third-party applications to access resources on behalf of users without sharing passwords. The app receives an **access token** from Google's authorization server.

---

**Q11.** An organization implements mandatory access control. A SECRET-cleared user creates a document and attempts to assign a PUBLIC classification to it. The system prevents this. Which MAC property is being enforced?

- A) Simple security property (no read up)
- B) Star property (no write down) from Bell-LaPadula
- C) Simple integrity property (no read down) from Biba
- D) Need-to-know enforcement

**Answer: B**
The **Bell-LaPadula star property (no write down)** prevents a subject from writing to an object at a lower classification level — preventing downgrading of information. In MAC, users cannot declassify data on their own; this is enforced by the system.

---

**Q12.** Which of the following BEST describes the difference between OAuth 2.0 and OpenID Connect?

- A) OAuth 2.0 provides authentication; OIDC provides authorization
- B) OAuth 2.0 is for API authorization; OIDC adds authentication (identity) on top of OAuth
- C) OIDC replaces OAuth 2.0 entirely
- D) OAuth 2.0 uses XML assertions; OIDC uses JSON tokens

**Answer: B**
**OAuth 2.0** provides **authorization** — delegating access to resources. **OpenID Connect (OIDC)** is built on top of OAuth 2.0 and adds **authentication** — providing an ID Token (JWT) that proves who the user is.

---

**Q13.** An employee is terminated. The IT team disables their Active Directory account but does not delete it. Why might this approach be preferred over immediate deletion?

- A) Disabled accounts continue to be charged licenses
- B) Deletion would remove audit log entries associated with the account
- C) Disabled accounts preserve the audit trail and can be re-enabled if needed
- D) Deleting accounts requires approval from the data owner

**Answer: C**
**Disabling before deleting** preserves the account's audit trail (logs referencing that account SID remain meaningful), allows recovery if needed (e.g., legal holds), and maintains referential integrity in audit systems. Deletion is permanent and may break forensic investigations.

---

**Q14.** An organization uses a PAM solution that automatically rotates privileged account passwords after each use. What security benefit does this provide?

- A) Prevents unauthorized users from knowing the password at any given time
- B) Ensures administrators remember their passwords
- C) Eliminates the need for MFA on privileged accounts
- D) Reduces the number of privileged accounts required

**Answer: A**
Automatic password rotation after each use ensures that even if credentials are captured during a session, they are **invalid for any subsequent use**. This limits the time window in which stolen credentials are useful and prevents credential sharing.

---

**Q15.** A company allows employees from a partner organization to log in using their partner credentials to access a shared project portal. Which concept does this describe?

- A) Single Sign-On
- B) Federated identity
- C) Delegated authorization
- D) Privileged access management

**Answer: B**
**Federated identity** allows users from one organization (the partner) to authenticate using their home organization's identity provider to access resources in another organization (the project portal). Trust is established between the organizations' identity systems via SAML or OIDC.

---

**Q16.** What does it mean when a RADIUS server returns an "Access-Reject" response?

- A) The user's account exists but is locked
- B) The authentication credentials are incorrect or access policy is not met
- C) The RADIUS server is unavailable
- D) The network device configuration is incorrect

**Answer: B**
**Access-Reject** from RADIUS means authentication **failed** or the access policy was not satisfied. This could mean wrong credentials, expired account, policy violation, or any other condition the RADIUS server evaluates. The specifics are logged server-side, not returned to the client.

---

**Q17.** Which Kerberos attack involves requesting service tickets for service accounts and cracking them offline to obtain service account passwords?

- A) Golden Ticket attack
- B) Silver Ticket attack
- C) Pass-the-Ticket
- D) Kerberoasting

**Answer: D**
**Kerberoasting** exploits the fact that service tickets are encrypted with the service account's password hash. Any authenticated domain user can request service tickets, which can then be taken offline and cracked with tools like Hashcat to obtain the service account's plaintext password.

---

**Q18.** An organization implements just-in-time (JIT) privileged access. An administrator requests temporary admin rights, performs a system change, and the rights are automatically revoked after 30 minutes. What security principle does this MOST directly support?

- A) Defense in depth
- B) Least privilege
- C) Separation of duties
- D) Need to know

**Answer: B**
**Just-in-time access** is a direct implementation of **least privilege** — granting the minimum access necessary, only when needed, and only for as long as needed. This minimizes the attack surface of privileged accounts.

---

**Q19.** During an access recertification campaign, a manager approves access for all their direct reports without reviewing individual permissions. What risk does this practice create?

- A) Increased authentication burden on employees
- B) Accumulation of excessive privileges through rubber-stamp approvals
- C) Violation of the separation of duties principle
- D) Non-compliance with LDAP directory standards

**Answer: B**
Rubber-stamp approvals during recertification defeat the purpose of the review. They perpetuate **access creep** by validating excessive permissions without scrutiny. Effective recertification requires managers to genuinely review and justify each access grant.

---

**Q20.** An organization discovers several service accounts in Active Directory that are associated with former employees and systems that have been decommissioned. What are these accounts called, and what is the PRIMARY risk?

- A) Privileged accounts; risk of administrative abuse
- B) Orphaned accounts; risk of unauthorized use since no one monitors them
- C) Shared accounts; risk of non-repudiation failure
- D) Stale accounts; risk of password expiration lockout

**Answer: B**
**Orphaned accounts** have no active owner — the associated user or system no longer exists. They present a serious risk because: no one monitors them for suspicious activity, they may retain broad permissions, and they can be exploited by attackers for persistent, undetected access.

---

**Q21.** Which IAM protocol was identified as appearing frequently on 2026 CISSP exams and is commonly tested for its role in API authorization?

- A) SAML
- B) Kerberos
- C) OAuth 2.0
- D) LDAP

**Answer: C**
**OAuth 2.0** is a high-frequency exam topic in 2026, tested for its role in **API authorization** — allowing third-party apps to access resources on behalf of users without sharing passwords. It is frequently confused with SAML (federated SSO) and OIDC (authentication). Know all three and when each applies.

---

**Q22.** Humans make first contact with aliens and want to share scientific data without revealing defense capabilities. Which access control model BEST supports this requirement?

- A) MAC — rigid label-based control enforced by the security kernel
- B) DAC — resource owners grant access at their discretion
- C) ABAC — flexible, attribute-driven access control
- D) RBAC — role-based access aligned to job function

**Answer: C**
**ABAC** is the best fit: it allows dynamic, context-sensitive access decisions based on attributes of the subject (human), the object (data type), and the environment (sensitivity). Scientific data (attribute = "scientific") can be shared while defense data (attribute = "classified-military") is denied — without rigid labels or pre-defined roles. MAC is too inflexible; DAC provides no central control.

---

**Q23.** SAML uses which data format for exchanging authentication assertions between Identity Provider and Service Provider?

- A) JSON (JavaScript Object Notation)
- B) XML (eXtensible Markup Language)
- C) JWT (JSON Web Token)
- D) SOAP envelope

**Answer: B**
**SAML (Security Assertion Markup Language)** is explicitly **XML-based**. Assertions are structured XML documents signed with the IdP's private key. This is a key distinguishing feature: SAML = XML assertions; OAuth/OIDC = JSON/JWT tokens. Knowing this distinction separates correct from incorrect answers on federated identity questions.

---

**Q24.** A biometric system at a high-security facility has a FAR of 0.001% and an FRR of 8%. What is the likely security and usability implication?

- A) The system is well-balanced — both error rates are acceptable
- B) The system has very low security risk but very high user rejection rate — legitimate users are frequently denied
- C) The system has very high security risk with a high false acceptance rate
- D) The system cannot be compared without knowing the CER

**Answer: B**
A FAR of 0.001% means almost no unauthorized person is accepted (very high security). An FRR of 8% means **8% of legitimate users are rejected** at each attempt — a significant usability problem that may lead to workarounds (propped doors, shared credentials). The sensitivity is tuned too high, increasing FRR at the expense of user experience while maintaining strong security.

---

**Q25.** An employee is terminated on a Friday afternoon. IT receives the notification Monday morning and disables the account then. What risk existed over the weekend?

- A) The account was still active and could have been used for unauthorized access
- B) The employee's data was automatically deleted by the system
- C) The employee could have changed their password, locking IT out
- D) This is acceptable; weekend access is not monitored anyway

**Answer: A**
A **72-hour window** where a terminated employee's account remains active is a critical risk — especially for hostile terminations. The account could be used to exfiltrate data, sabotage systems, or establish persistence (backdoors). Best practice: disable accounts **immediately** at the moment of termination notification, not at IT's next business day convenience.

---

**Q26.** In the Kerberos flow, what does the client present to the Ticket Granting Service (TGS) to obtain a service ticket?

- A) The user's password hash
- B) The Ticket Granting Ticket (TGT)
- C) The service account's NTLM hash
- D) The client's digital certificate

**Answer: B**
After initial authentication to the AS, the client receives a **TGT** (Ticket Granting Ticket). To access a specific service (file share, database), the client presents the TGT to the **TGS**, which issues a service-specific ticket. The user's password is only used once at initial login — Kerberos is designed so the password never travels on the network.

---

**Q27.** What is the MAIN security difference between RADIUS and TACACS+ encryption?

- A) RADIUS encrypts all fields; TACACS+ encrypts only the password
- B) TACACS+ encrypts the entire authentication packet; RADIUS encrypts only the password field
- C) Both encrypt all data but use different algorithms
- D) Neither protocol encrypts authentication data by default

**Answer: B**
**TACACS+** encrypts the **entire packet body** (all fields). **RADIUS** encrypts **only the password** field — usernames, accounting data, and authorization attributes travel in cleartext. For device administration (routers, switches), TACACS+ is preferred because admin commands in authorization packets are also encrypted.

---

**Q28.** What does FIDO2 / WebAuthn use to eliminate passwords and phishing risk?

- A) One-time passwords sent via SMS
- B) Hardware security keys or biometrics bound to a specific origin (domain)
- C) Software-generated TOTP codes
- D) Smart cards with X.509 certificates

**Answer: B**
**FIDO2 / WebAuthn** uses **public-key cryptography** where the private key is stored on a hardware security key or device TPM/secure enclave. Authentication is **origin-bound** — credentials are cryptographically tied to the specific domain, making them impossible to phish to a different site. No password is ever transmitted or stored.

---

**Q29.** An organization's access review shows a user has accumulated 14 different system roles over 6 years and multiple job changes. What corrective action should be taken first?

- A) Delete the user account and recreate it with only current role permissions
- B) Conduct a role-by-role review with the user's current manager and revoke all roles no longer required
- C) Implement MFA on all 14 system roles as a compensating control
- D) Notify the user that their access is under review

**Answer: B**
The correct remediation for **access creep** is a structured **access recertification** review: the current manager validates each role, and any access not justified by the current job function is revoked. Deleting the account loses the audit trail. MFA does not reduce excess access — it only adds an authentication factor. The root problem is authorization, not authentication.

---

**Q30.** An organization deploys context-aware (adaptive) authentication. A user logs in from a country they have never accessed from before at 3 AM local time. What should the system do?

- A) Deny the login outright regardless of credentials
- B) Allow access if the correct password is provided
- C) Trigger step-up authentication requiring a second factor
- D) Immediately lock the account and alert the security team

**Answer: C**
**Adaptive / context-aware authentication** uses risk signals (unusual location, unusual time, impossible travel) to dynamically increase authentication requirements. A new country + unusual hours is a high-risk signal that should trigger **step-up authentication** (MFA challenge) rather than an outright deny (which would block legitimate users traveling) or silent allow (which accepts potentially stolen credentials).
