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
