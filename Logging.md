# Logging

- Logging is the process of recording events, information and actions that occur in a system or application.

- Logging is done to document What happened, when it happened, where it happened and who did it - and sometimes how it happened.

- Prerequisite for logging - Authentication & identification - Secure identification and authentication practices are a prerequisite for logging.

- Logs are sometimes referred as audit logs, however, auditing is more than logging.

#### 1. Foundational AAA Concepts

Logging and monitoring are critical components of the **AAA (Authentication, Authorization, and Accounting)** framework.

- **Auditing vs. Monitoring:** While often used interchangeably, they have distinct roles. **Monitoring** is the continuous act of watching or oversight to detect abnormal activity. **Auditing** is the programmatic process of recording those activities into a record or file to hold subjects accountable. You can monitor without auditing, but you cannot audit without some form of monitoring.
- **Accountability and Nonrepudiation:** The ultimate goal is to link a specific individual to their online actions. When logs are properly secured, they provide **nonrepudiation**, meaning a user cannot deny performing an action recorded in the audit trail.
- **Accounting (Accountability):** This is the actual **review** of log files to check for compliance with security policies and to identify violations.

---

#### 2. Detailed Common Log Types

- **Security Logs:** Record access to resources like files and folders. They document who accessed a file, when, and what they did (read, modify, delete).
- **System Logs:** Document "health" and status events, such as system startups/shutdowns, service crashes, or unauthorized attempts to change service attributes.
- **Application Logs:** Capture events specific to a program, such as database table access or application errors, as defined by the developers.
- **Firewall/Network Logs:** Record all traffic reaching the perimeter, typically logging source/destination IP addresses and ports. They are vital for identifying scanning activity or blocked connection attempts.
- **Proxy Logs:** Track web activity, including URLs visited and time spent, which is useful for enforcing **Acceptable Use Policies (AUP)**.
- **Change Logs:** Part of the **Change Management** process; they record every requested, approved, and implemented modification to the system, providing a road map for disaster recovery or reversing failed updates.

---

#### 3. Technical Log Management (SIEM & Tools)

- **Security Information and Event Management (SIEM):** SIEMs provide centralized logging and real-time analysis. Key features include:
  - **Aggregation:** Collecting data from dissimilar sources (firewalls, servers, IDSs).
  - **Correlation:** Sophisticated engines that link related events from different systems to identify complex attack patterns.
  - **Alarm Triggers:** Automated alerts based on preconfigured rules (e.g., notifying an administrator if an email queue backs up).
- **Syslog (RFC 5424):** A standard protocol for sending event notification messages to a central server. Extensions like **syslog-ng** or **rsyslog** allow servers to accept data from diverse, non-Unix sources.
- **Transmission Logging:** A specialized form of auditing focused on communications, recording packet sizes, transmission status, and identification codes.

---

#### 4. Security and Integrity of Logs

- **Remote Logging:** Immediately sending records to a centralized, locked-down server is a primary defense. This prevents attackers from "sanitizing" (deleting) local logs to hide their tracks.
- **Time Synchronization (NTP):** Using the **Network Time Protocol (NTP)** to sync all system clocks to a trusted source (like NIST) is mandatory. It ensures a consistent timeline across all logs, which is vital for legal evidence and forensic investigations.
- **Integrity Protections:** Logs should be set to read-only and restricted via strict permissions. **Digital signatures** can be used to prove that a log file has not been tampered with since its creation.
- **Retention Policies:** Policies must define how long logs are kept (e.g., 1 year vs. 10 years). Retaining logs too long can increase discovery costs during litigation, while deleting them too early may violate regulations like **PCI DSS** or **HIPAA**.

---

#### 5. Operational Analysis Techniques

- **Clipping Levels:** A nonstatistical sampling method that sets a threshold for alarms. It allows systems to ignore routine errors (like one failed login) but triggers an alert when a threshold is exceeded (e.g., five failures in a minute), signaling a potential attack.
- **Sampling (Data Extraction):** The process of extracting specific elements from a massive body of data to create a meaningful summary. **Statistical sampling** is more mathematically defensible, while **nonstatistical sampling** is often done at the auditor's discretion to focus on specific high-risk events.
- **Egress Monitoring:** Specifically watching traffic as it leaves the internal network to detect botnet beaconing or data exfiltration.
- **Threat Hunting:** A proactive process where security professionals use log data and **threat feeds** to aggressively search for indicators of compromise (IoCs) that traditional tools might have missed.

---

#### 6. Administrative & Physical Auditing

- **Log Reviews:** Managers should periodically review logs for sensitive functions (e.g., eDiscovery tool usage) to ensure privileged users are not abusing their access.
- **Privileged Account Monitoring:** Strict oversight of administrator or "root" actions (creating accounts, changing firewall rules) is a top priority for detecting insider threats or **Advanced Persistent Threats (APTs)**.
- **Physical Access Logs:** Visitor logs and smartcard records establish context for logical logs, such as determining if a person was physically present in the building when their account was used to access a server.

*
