# Domain 4 — Communication and Network Security: Practice Questions

---

**Q1.** An organization needs to encrypt all traffic between two branch offices over the public internet, including the IP headers. Which IPSec configuration should be used?

- A) AH in transport mode
- B) ESP in transport mode
- C) ESP in tunnel mode
- D) AH in tunnel mode

**Answer: C**
**ESP in tunnel mode** encrypts the entire original packet (headers and payload) and wraps it in a new IP header — ideal for VPN connections between gateways. ESP provides encryption; tunnel mode protects the original IP headers. AH provides only authentication, not encryption.

---

**Q2.** A network administrator discovers that an attacker has flooded the switch's CAM table with fake MAC addresses, causing it to broadcast all traffic to every port. What type of attack is this?

- A) ARP poisoning
- B) VLAN hopping
- C) MAC flooding
- D) SYN flood

**Answer: C**
**MAC flooding** overwhelms the switch's CAM (content-addressable memory) table. When the table is full, the switch fails open and broadcasts all traffic to all ports — effectively becoming a hub and allowing passive eavesdropping.

---

**Q3.** A security analyst needs to monitor traffic between the firewall and the internal network. At which OSI layer do most NGFW devices primarily perform deep packet inspection?

- A) Layer 2
- B) Layer 3
- C) Layer 4
- D) Layer 7

**Answer: D**
**NGFWs (Next-Generation Firewalls)** perform deep packet inspection at **Layer 7 (Application layer)**, allowing them to identify and control specific applications regardless of port, inspect content, and enforce application-level policies.

---

**Q4.** Which wireless protocol uses SAE (Simultaneous Authentication of Equals) to protect against offline dictionary attacks on the passphrase?

- A) WEP
- B) WPA2-Personal
- C) WPA3-Personal
- D) WPA2-Enterprise

**Answer: C**
**WPA3-Personal** uses **SAE (Dragonfly handshake)** instead of the WPA2 4-way handshake. SAE prevents offline dictionary attacks because each authentication attempt requires an online interaction — attackers cannot capture handshakes and crack offline.

---

**Q5.** An attacker sends unsolicited ARP reply packets to associate their MAC address with a legitimate server's IP address. Other hosts on the subnet update their ARP caches. What is the attacker's likely goal?

- A) Denial of service against the server
- B) Man-in-the-middle interception of traffic
- C) VLAN hopping to reach other network segments
- D) Overwhelming the switch CAM table

**Answer: B**
**ARP poisoning** causes other hosts to send traffic destined for the server to the attacker instead. The attacker can then forward it to the real server (transparent MitM), intercept credentials, and modify data in transit.

---

**Q6.** A company's employees connect to the corporate VPN for remote access. When split tunneling is enabled, what is the PRIMARY security risk?

- A) VPN performance degrades significantly
- B) Employees' internet traffic bypasses the corporate proxy and DLP controls
- C) All traffic is decrypted and visible to ISPs
- D) VPN credentials can be brute-forced

**Answer: B**
With **split tunneling**, only corporate-bound traffic goes through the VPN; internet traffic goes directly through the ISP. This means employee internet activity bypasses corporate **proxy, DLP, content filtering, and malware inspection** — and compromised endpoints can reach both the internet and the corporate network simultaneously.

---

**Q7.** A web server in the DMZ is accessible from the internet and can communicate with the internal database server. Which architecture violates the principle of a properly designed DMZ?

- A) The web server can receive traffic from the internet
- B) The database server is accessible from the web server in the DMZ
- C) Internal users can access the web server through a firewall
- D) The DMZ is separated from the internet by a firewall

**Answer: B**
A properly designed DMZ should **not allow DMZ servers direct access to internal servers**. The web server in the DMZ should be treated as untrusted — if compromised, it should not directly reach the database. An application tier or firewall rule should mediate this access.

---

**Q8.** Which protocol provides email senders the ability to specify that a message was unaltered in transit by applying a cryptographic signature verified via DNS?

- A) SPF
- B) DMARC
- C) TLS
- D) DKIM

**Answer: D**
**DKIM (DomainKeys Identified Mail)** adds a cryptographic signature to email headers, verified using a public key published in DNS. It proves the message was not altered in transit and originated from an authorized source for that domain.

---

**Q9.** During a TCP connection, an attacker intercepts the SYN-ACK from the server and sends a RST packet to the client, then completes the handshake with the server. What attack is being performed?

- A) SYN flood
- B) Session hijacking
- C) TCP reset attack
- D) Replay attack

**Answer: B**
**Session hijacking** (TCP session hijacking) involves an attacker taking over an established TCP session. By intercepting traffic and predicting sequence numbers, the attacker can inject packets as if they were a legitimate party.

---

**Q10.** An organization deploys 802.1X on all wireless access points. What component is responsible for verifying user credentials during authentication?

- A) The access point (authenticator)
- B) The client device (supplicant)
- C) The RADIUS server (authentication server)
- D) The LDAP directory

**Answer: C**
In 802.1X, the **RADIUS server (Authentication Server)** verifies credentials. The access point (**Authenticator**) passes authentication requests from the client (**Supplicant**) to the RADIUS server and enforces access based on the result.

---

**Q11.** A user connects to a coffee shop Wi-Fi network that has the same SSID as the legitimate network. The rogue network does not require a password and intercepts all traffic. What attack is this?

- A) WPS brute force
- B) Deauthentication attack
- C) Evil twin attack
- D) Replay attack

**Answer: C**
An **evil twin** is a rogue access point configured with the same SSID (and possibly BSSID spoofing) as a legitimate AP to lure users into connecting. The attacker can then perform MitM attacks on all connected traffic.

---

**Q12.** Which protocol adds cryptographic validation to DNS responses to prevent DNS cache poisoning?

- A) DNSSEC
- B) LDAPS
- C) DMARC
- D) DoH (DNS over HTTPS)

**Answer: A**
**DNSSEC (DNS Security Extensions)** adds digital signatures to DNS records, allowing resolvers to cryptographically verify that responses are authentic and have not been tampered with — directly preventing DNS cache poisoning.

---

**Q13.** An organization wants to provide employees with network access only after their device's security posture is verified (patched OS, active AV, etc.). Which technology supports this requirement?

- A) VPN concentrator
- B) Network Access Control (NAC)
- C) RADIUS authentication server
- D) Web application firewall

**Answer: B**
**NAC (Network Access Control)** enforces policies by checking endpoint health and compliance (patch level, AV status, configuration) before allowing network access. Non-compliant devices are quarantined to a remediation network.

---

**Q14.** Which firewall type maintains a table of active connections and only allows inbound packets that are part of an established connection?

- A) Packet filtering firewall
- B) Stateful inspection firewall
- C) Application-layer proxy
- D) Next-generation firewall

**Answer: B**
A **stateful inspection firewall** tracks the state of active connections and only permits packets that are part of an established, valid session. Packet filtering firewalls lack state awareness and can be bypassed with crafted packets.

---

**Q15.** An attacker sends ICMP echo requests to a network broadcast address with a spoofed source IP (the victim's IP). All hosts respond to the victim, overwhelming it with traffic. What attack is this?

- A) SYN flood
- B) DDoS amplification
- C) Smurf attack
- D) Teardrop attack

**Answer: C**
A **Smurf attack** uses ICMP echo requests sent to a broadcast address with a spoofed victim IP. All hosts on the network reply to the victim, amplifying the attack. Mitigation: disable directed broadcast at routers.

---

**Q16.** When comparing SSH and Telnet for remote administration, what is the PRIMARY security advantage of SSH?

- A) SSH is faster than Telnet
- B) SSH encrypts all transmitted data including credentials
- C) SSH uses a different port that is harder to scan
- D) SSH does not require authentication

**Answer: B**
**SSH encrypts** the entire session including authentication credentials. **Telnet** transmits everything — including usernames and passwords — in plaintext, making it trivially interceptable by any network sniffer.

---

**Q17.** A security architect is designing a Zero Trust Architecture. Which of the following BEST describes the Zero Trust approach to network access?

- A) Trust internal network traffic; verify only external traffic
- B) Verify identity and device health for every access request, regardless of network location
- C) Implement strong perimeter firewalls to control all inbound traffic
- D) Require VPN for all remote access to the corporate network

**Answer: B**
**Zero Trust** eliminates the concept of a trusted internal network. **Every access request** — whether from inside or outside the perimeter — is verified based on identity, device health, and context. "Never trust, always verify" is the core principle.

---

**Q18.** An attacker performs a double-tagging attack by encapsulating a frame with two 802.1Q VLAN tags. The outer tag matches the native VLAN of the trunk port. What attack category is this?

- A) ARP spoofing
- B) MAC flooding
- C) VLAN hopping
- D) Spanning tree manipulation

**Answer: C**
**VLAN hopping** via double-tagging exploits the fact that switches strip only one VLAN tag on native VLAN trunk ports. The second (inner) tag causes the frame to be forwarded to a different VLAN. Mitigation: assign a dedicated, unused native VLAN ID.

---

**Q19.** An organization implements DMARC with a policy of "reject." What happens to emails that fail both SPF and DKIM checks?

- A) They are delivered normally with a warning header
- B) They are quarantined and held for review
- C) They are rejected and not delivered
- D) They are forwarded to the security team

**Answer: C**
DMARC with policy `reject` instructs receiving mail servers to **reject** (not deliver) emails that fail both SPF and DKIM alignment checks. This is the most protective DMARC policy and prevents spoofed emails from reaching inboxes.

---

**Q20.** A company's security policy requires that all network devices use encrypted management protocols. A network engineer discovers several switches still configured with SNMPv2 for monitoring. What is the PRIMARY risk?

- A) SNMPv2 does not support monitoring of switch interfaces
- B) SNMPv2 transmits community strings (passwords) in cleartext
- C) SNMPv2 is incompatible with modern SIEM systems
- D) SNMPv2 does not support IPv6

**Answer: B**
**SNMPv1 and SNMPv2** use community strings for authentication, which are transmitted in **plaintext**. An attacker who can intercept network traffic can capture the community string and use it to query or even modify device configurations. **SNMPv3** provides encryption and authentication.

---

**Q21.** A company deploys iSCSI to run block-level storage traffic over their existing IP network. A security engineer warns management that this decision introduces new risks. What is the PRIMARY inherited risk?

- A) iSCSI is incompatible with existing firewall rules
- B) iSCSI traffic inherits all TCP/IP vulnerabilities including sniffing and man-in-the-middle attacks
- C) iSCSI requires dedicated hardware incompatible with standard switches
- D) iSCSI cannot be encrypted and therefore cannot be secured

**Answer: B**
**iSCSI** encapsulates block-level storage commands inside TCP/IP packets. Because it runs over standard IP networks, it inherits all TCP/IP attack vectors: packet sniffing, ARP poisoning enabling MitM, and network-layer attacks that could expose or corrupt storage traffic. The mitigation is iSCSI over dedicated isolated VLANs with IPSec or MACsec encryption.

---

**Q22.** A security architect says "our SAN runs on FCoE — it's completely separate from our regular network." Why is this statement misleading from a CISSP perspective?

- A) FCoE cannot be used for SAN storage
- B) FCoE runs over standard Ethernet, so Layer 2 attacks (ARP poisoning, VLAN hopping) can affect it
- C) FCoE uses a proprietary protocol that replaces Ethernet entirely
- D) FCoE cannot be segmented using VLANs

**Answer: B**
**FCoE (Fibre Channel over Ethernet)** encapsulates Fibre Channel frames inside Ethernet frames. It runs on the same physical Ethernet infrastructure and is subject to the same Layer 2 attacks as any other Ethernet traffic. "Separate" only applies at the logical protocol level, not the physical or security threat level.

---

**Q23.** At which OSI layer does a standard Layer 2 switch primarily operate, and what is its security implication?

- A) Layer 3 — can filter by IP address
- B) Layer 2 — forwards based on MAC addresses; vulnerable to MAC flooding and ARP poisoning
- C) Layer 4 — understands TCP sessions and can block ports
- D) Layer 7 — inspects application content

**Answer: B**
Switches operate at **Layer 2 (Data Link)** and forward frames based on MAC addresses. Their security implication: they are vulnerable to MAC flooding (CAM table exhaustion) and ARP poisoning, both of which can turn a switch into a hub or redirect traffic to an attacker.

---

**Q24.** A VoIP implementation uses SIP for session control. Which security risk does this converged protocol introduce that a traditional PSTN phone system did not?

- A) VoIP calls are always recorded by default
- B) VoIP calls can be intercepted, spoofed, or subjected to DoS attacks over the IP network
- C) VoIP is less reliable than PSTN due to packet loss
- D) VoIP cannot be encrypted, making it inherently insecure

**Answer: B**
**VoIP** running over IP inherits IP-layer vulnerabilities: call interception (eavesdropping on RTP streams), caller ID spoofing, and DoS attacks (SIP flooding to exhaust call processing capacity). Traditional PSTN calls on circuit-switched networks did not face these IP-layer threats. Mitigation: SRTP (Secure RTP) for media encryption, TLS for SIP signaling.

---

**Q25.** What is the primary security advantage of EAP-TLS over PEAP for enterprise wireless authentication?

- A) EAP-TLS is faster because it uses fewer round trips
- B) EAP-TLS requires client certificates, providing mutual authentication and preventing rogue APs
- C) EAP-TLS does not require a RADIUS server
- D) EAP-TLS supports WPA3 while PEAP only supports WPA2

**Answer: B**
**EAP-TLS** requires both a server certificate AND a **client certificate**. This mutual authentication means that even if an attacker sets up a rogue AP with a valid server certificate, the client will not authenticate because the rogue AP cannot present a trusted client certificate. PEAP requires only a server certificate — a rogue AP with a valid or spoofed server cert can trick clients into connecting.

---

**Q26.** A network engineer proposes disabling split tunneling on the corporate VPN. What is the primary security benefit of this change?

- A) It reduces the amount of bandwidth consumed by the VPN tunnel
- B) All user traffic is routed through corporate security controls (proxy, DLP, firewall) regardless of destination
- C) It eliminates the risk of credential theft during VPN authentication
- D) It prevents users from connecting to the VPN from untrusted locations

**Answer: B**
Disabling split tunneling means **all traffic** (internet and corporate) flows through the corporate VPN and is inspected by corporate security controls. This prevents malware on a user's device from directly reaching the internet while also connected to corporate resources, and ensures corporate DLP/proxy rules apply to all activity.

---

**Q27.** An organization implements DMARC but leaves the policy set to "p=none." What does this mean?

- A) All emails that fail SPF and DKIM checks are rejected
- B) All emails that fail SPF and DKIM checks are quarantined
- C) DMARC monitoring is active but no enforcement action is taken — failed emails are still delivered
- D) DMARC is disabled and has no effect on email delivery

**Answer: C**
**p=none** is "monitor-only" mode — DMARC collects data about SPF/DKIM failures and sends reports to the domain owner, but takes **no enforcement action**. Failed emails are still delivered. Organizations typically start with p=none to gather visibility, then progress to p=quarantine and finally p=reject. Without enforcement, DMARC provides no actual protection.

---

**Q28.** Which BGP security mechanism cryptographically validates that a network prefix announcement originates from an authorized Autonomous System?

- A) DNSSEC
- B) BGP communities
- C) RPKI (Resource Public Key Infrastructure)
- D) BCP38 ingress filtering

**Answer: C**
**RPKI (Resource Public Key Infrastructure)** uses cryptographically signed Route Origin Authorizations (ROAs) to validate that a BGP prefix announcement is made by the legitimate AS that owns that IP space. It prevents BGP hijacking attacks where attackers announce more-specific routes to redirect internet traffic.

---

**Q29.** An organization wants the strongest possible enterprise wireless security. Which combination represents the current best practice?

- A) WPA2-Personal with AES-CCMP and a long PSK
- B) WPA2-Enterprise with 802.1X and PEAP-MSCHAPv2
- C) WPA3-Enterprise with 802.1X and EAP-TLS
- D) WPA3-Personal with SAE and certificate pinning

**Answer: C**
**WPA3-Enterprise with 802.1X and EAP-TLS** represents the strongest available wireless security: WPA3-Enterprise uses 192-bit security mode with AES-256-GCMP, combined with EAP-TLS for mutual certificate-based authentication. This combination resists all known offline attacks and provides the strongest identity assurance.

---

**Q30.** A firewall rule states "DENY ALL" at the bottom of the ruleset with no explicit permit rule for a specific traffic type. A packet matching no prior rules arrives. What happens?

- A) The packet is forwarded to the next firewall in the chain
- B) The packet is logged and sent to the security team for review
- C) The packet is dropped — implicit deny applies
- D) The packet is allowed by default until a deny rule is added

**Answer: C**
**Implicit deny (default deny)** is a fundamental firewall principle: any traffic not explicitly permitted by a rule is denied. This is the security-correct default. A "DENY ALL" at the bottom makes this explicit, but even without it, most security-focused firewalls apply implicit deny as their default action for unmatched traffic.
