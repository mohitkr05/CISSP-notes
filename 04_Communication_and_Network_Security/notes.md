# Domain 4 — Communication and Network Security

**Exam Weight: 13%**

---

## 4.1 OSI Model

### The 7 Layers (memorize: Please Do Not Throw Sausage Pizza Away)

| Layer | Name | Protocols/Devices | PDU |
|-------|------|-------------------|-----|
| 7 | **Application** | HTTP, HTTPS, FTP, DNS, SMTP, LDAP | Data |
| 6 | **Presentation** | TLS/SSL, encryption, compression | Data |
| 5 | **Session** | NetBIOS, RPC, SOCKS | Data |
| 4 | **Transport** | TCP, UDP | Segment |
| 3 | **Network** | IP, ICMP, OSPF, BGP | Packet |
| 2 | **Data Link** | Ethernet, ARP, MAC, switches, VLANs | Frame |
| 1 | **Physical** | Cables, hubs, wireless signals | Bits |

> **Mnemonic (top-down):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

### Key Layer Associations
- **Firewall** — Layer 3/4 (packet filtering) or Layer 7 (application firewall)
- **Switch** — Layer 2
- **Router** — Layer 3
- **IDS/IPS** — Layer 3–7 depending on type
- **VPN** — Layer 2 (L2TP) or Layer 3 (IPSec) or Layer 4/7 (SSL)

---

## 4.2 TCP/IP Protocols

### TCP vs. UDP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery (ACK) | Best-effort |
| Order | In-order delivery | Not guaranteed |
| Speed | Slower | Faster |
| Use case | HTTP, FTP, SMTP | DNS, DHCP, VoIP, streaming |

### TCP Three-Way Handshake
```
Client → SYN → Server
Client ← SYN-ACK ← Server
Client → ACK → Server
[Connection established]
```

**SYN Flood Attack** — attacker sends many SYNs without completing handshake; exhausts server connection table.
**Mitigation:** SYN cookies; rate limiting; ingress filtering.

### Key Ports to Know

| Port | Protocol |
|------|---------|
| 20/21 | FTP (data/control) |
| 22 | SSH |
| 23 | Telnet (insecure) |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 514 | Syslog |
| 636 | LDAPS |
| 1433 | MS SQL |
| 3306 | MySQL |
| 3389 | RDP |

### IPv4 vs. IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address length | 32-bit | 128-bit |
| Format | Dotted decimal | Hexadecimal colons |
| NAT required | Yes (address exhaustion) | No (vast address space) |
| IPSec | Optional | Mandatory |
| Broadcast | Yes | No (uses multicast) |

---

## 4.3 Network Devices and Their Security Roles

### Firewalls

| Type | How It Works | OSI Layer |
|------|-------------|-----------|
| **Packet Filtering** | Filter by IP, port, protocol; stateless | 3-4 |
| **Stateful Inspection** | Tracks connection state; aware of session context | 3-4 |
| **Application-Layer (Proxy)** | Inspects application data; content-aware | 7 |
| **NGFW (Next-Gen)** | Deep packet inspection; IDS/IPS; app awareness; user-identity | 3-7 |

**Implicit Deny** — all firewalls should have a default rule denying all traffic not explicitly permitted.

### DMZ (Demilitarized Zone)
A screened subnet placed between the internet and internal network. Public-facing servers (web, email, DNS) live here.

```
Internet → [Firewall] → DMZ (Web, Email, DNS servers) → [Firewall] → Internal Network
```

### Network Segmentation Controls
- **VLANs** — logical segmentation at Layer 2
- **Subnets** — logical segmentation at Layer 3
- **Firewalls** — enforce segmentation policies
- **Micro-segmentation** — granular segmentation per workload (used in zero trust)

---

## 4.4 Secure vs. Insecure Protocols

| Insecure Protocol | Port | Secure Alternative | Port |
|------------------|------|-------------------|------|
| Telnet | 23 | **SSH** | 22 |
| FTP | 20/21 | **SFTP / FTPS** | 22 / 990 |
| HTTP | 80 | **HTTPS (TLS)** | 443 |
| SNMP v1/v2 | 161 | **SNMPv3** | 161 |
| LDAP | 389 | **LDAPS** | 636 |
| POP3 | 110 | **POP3S** | 995 |
| IMAP | 143 | **IMAPS** | 993 |
| SMTP | 25 | **SMTPS** | 465/587 |

> **Exam Tip:** Any protocol that transmits credentials in cleartext (Telnet, FTP, HTTP, SNMP v1/v2) should be replaced with its encrypted equivalent. SSH replaces both Telnet and (insecure) rlogin/rsh.

> **Scenario — Why Cleartext Protocols Are Critical Risks:**
> A network admin uses Telnet to manage a core router. An attacker on the same network segment runs Wireshark. Within seconds, they capture the admin's username and password in plaintext in the packet capture. They now have privileged access to the router and can reroute traffic, intercept communications, or bring down the network. **SSH** encrypts the session — the same Wireshark capture shows only encrypted gibberish.
>
> **SNMP v1/v2 specific trap:** SNMP v1/v2 uses a "community string" (essentially a shared password) in plain text for authentication. The community string "public" (read) and "private" (write) are the defaults and are often never changed. An attacker with access to sniff the network or simply try default community strings can read device configs and even modify routing tables. **SNMPv3** adds authentication (MD5/SHA) and encryption (DES/AES) — the only version acceptable for enterprise networks.

---

## 4.5 IPSec

IPSec provides network-layer security for IP communications.

### Two Protocols

| Protocol | Provides | Header |
|----------|---------|--------|
| **AH (Authentication Header)** | Integrity + authentication (no encryption) | New AH header |
| **ESP (Encapsulating Security Payload)** | Confidentiality + integrity + authentication | ESP header/trailer |

> **Exam Tip:** Use **ESP** when you need encryption. AH provides integrity only.

> **Scenario — IPSec AH vs. ESP, Transport vs. Tunnel:**
>
> **AH only:** Two financial servers need to verify that packets between them haven't been tampered with, but the data itself is not sensitive (it's aggregate statistics). Use AH — adds authentication headers that detect tampering, but no encryption overhead.
>
> **ESP:** A remote employee VPNs into the corporate network. Their laptop traffic (email, file shares, internal apps) must be kept confidential from ISP snooping. Use ESP — encrypts the payload so intercepted packets are useless.
>
> **Transport Mode:** Your laptop directly communicates with a company server. The original IP headers stay visible (source = your IP, destination = server IP). Only the payload is protected. Used when you don't need to hide the communication endpoints.
>
> **Tunnel Mode:** A branch office (192.168.1.0/24) connects to headquarters (10.0.0.0/8) via a site-to-site VPN. Each packet gets wrapped in a *new* IP header (VPN gateway IPs visible, internal IPs hidden). The entire original packet — including the original IP header — is encrypted inside the tunnel. An eavesdropper sees only the VPN gateway IPs, not the individual machines talking.

### Two Modes

| Mode | What is encrypted | Use case |
|------|-----------------|---------|
| **Transport Mode** | Payload only; original IP header exposed | Host-to-host |
| **Tunnel Mode** | Entire original packet (header + payload) wrapped | VPN (gateway-to-gateway) |

### IKE (Internet Key Exchange)
- Phase 1: Establishes secure channel; authenticates peers (IKE SA)
- Phase 2: Negotiates IPSec SAs for data transfer

---

## 4.6 VPN Technologies

| Type | Use Case | Protocol |
|------|---------|---------|
| **Site-to-Site VPN** | Connect two networks permanently | IPSec, GRE over IPSec |
| **Remote Access VPN** | Individual user connecting to corp network | SSL/TLS VPN, IPSec VPN |
| **SSL VPN** | Browser-based or client; works through firewalls | TLS |
| **PPTP** | Legacy; insecure; avoid | Point-to-Point Tunneling |
| **L2TP/IPSec** | Secure; combines L2TP tunneling with IPSec | L2TP + IPSec |

**Split Tunneling** — only traffic destined for corporate network goes through VPN; internet traffic goes direct. Risk: malware on the client can reach corporate resources. Many organizations disable split tunneling.

> **Scenario — Split Tunneling Risk:**
> An employee works from home with split tunneling enabled. She visits a malicious website while VPN'd in — her browser goes directly out to the internet (not through corporate VPN). The site drops malware on her machine. Because split tunneling is enabled, that malware can now pivot directly to corporate resources through the VPN tunnel that's already open. If split tunneling were disabled, ALL traffic would flow through the corporate firewall and proxy, which could detect and block the malware before it reached internal systems.
>
> **The trade-off:** Full tunnel (no split) protects corporate resources but uses more corporate bandwidth for non-work traffic (YouTube, personal email all goes through company's internet connection). Split tunnel is faster and cheaper but creates a security gap.

---

## 4.7 Wireless Security

### Wireless Security Protocols (weakest to strongest)

| Protocol | Encryption | Authentication | Status |
|----------|-----------|---------------|--------|
| **WEP** | RC4 (flawed) | Shared key | Broken; never use |
| **WPA** | TKIP (weak) | Pre-shared key | Deprecated |
| **WPA2-Personal** | AES/CCMP | Pre-shared key (PSK) | Acceptable |
| **WPA2-Enterprise** | AES/CCMP | 802.1X + EAP | Recommended for enterprise |
| **WPA3-Personal** | AES-GCMP | SAE (Dragonfly) | Current; resists offline dictionary attacks |
| **WPA3-Enterprise** | AES-GCMP 256-bit | 802.1X + EAP | Highest security |

### 802.1X (Port-Based Network Access Control)
- Requires authentication before network access is granted
- Components: Supplicant (client), Authenticator (switch/AP), Authentication Server (RADIUS)
- Used for both wired and wireless enterprise security

### EAP Methods (common on exam)

| Method | Description | Security |
|--------|-------------|---------|
| **EAP-TLS** | Mutual certificate authentication | Strongest; requires client certs |
| **PEAP** | Tunneled EAP; only server cert required | Good; widely used |
| **EAP-TTLS** | Like PEAP; more flexible inner methods | Good |
| **EAP-MD5** | Password hash; no server validation | Weak; avoid |

### Common Wireless Attacks

| Attack | Description | Mitigation |
|--------|-------------|-----------|
| **Evil Twin** | Rogue AP mimics legitimate AP | 802.1X; certificate validation |
| **Deauthentication attack** | Sends forged deauth frames to disconnect clients | 802.11w (management frame protection) |
| **WPS PIN attack** | Brute-force WPS PIN (8 digits = 10^7) | Disable WPS |
| **Wardriving** | Scanning for open/insecure networks | Secure all APs; use WPA3 |
| **Replay attack** | Captures and retransmits valid frames | Per-packet keying; sequence numbers |
| **Rogue AP** | Unauthorized AP on corporate network | Wireless IDS; port security |

---

## 4.8 Network Attacks

### Common Layer 2 Attacks

| Attack | Description | Mitigation |
|--------|-------------|-----------|
| **ARP Poisoning/Spoofing** | Sends fake ARP replies; associates attacker MAC with victim IP; enables MitM | Dynamic ARP Inspection (DAI); static ARP entries |
| **MAC Flooding** | Overflow switch CAM table; switch becomes a hub | Port security; 802.1X |
| **VLAN Hopping** | Double-tagging or switch spoofing to access other VLANs | Disable auto-trunking; dedicated native VLAN |

> **Scenario — ARP Poisoning MitM Attack:**
> Alice (192.168.1.10) wants to communicate with her bank server (192.168.1.1). ARP doesn't validate responses — any device can claim to be any IP. Attacker Eve sends a broadcast: "I am 192.168.1.1 — my MAC is EE:EE:EE:EE:EE:EE." Alice's ARP table now points the bank server's IP to Eve's MAC. Alice's packets go to Eve instead of the bank. Eve can read them, modify them, and forward them to the real bank — a perfect MitM.
>
> **Dynamic ARP Inspection (DAI)** prevents this by validating ARP responses against the DHCP snooping binding table. If a port hasn't legitimately been assigned an IP via DHCP, the switch won't accept ARP replies claiming that IP.
>
> **VLAN Hopping Scenario:** VLAN 10 is for users; VLAN 20 is for servers. An attacker on VLAN 10 double-tags a frame: outer tag = VLAN 10 (their VLAN), inner tag = VLAN 20 (target). The switch strips the outer tag and forwards the frame with the VLAN 20 tag to a trunk port — the frame arrives at the VLAN 20 switch and gets delivered to a server. This only works in one direction and requires the attacker to be on the native VLAN. Fix: change the native VLAN to an unused VLAN and disable auto-trunking on all access ports.

### DNS Attacks

| Attack | Description | Mitigation |
|--------|-------------|-----------|
| **DNS Poisoning/Cache Poisoning** | Inject false DNS records; redirect traffic | DNSSEC; use trusted resolvers |
| **DNS Amplification DDoS** | Use open resolvers to amplify DDoS traffic | Disable open recursion; BCP38 |
| **DNS Hijacking** | Compromise DNS server or registrar | DNSSEC; registry locks |

### DoS / DDoS

| Type | Description |
|------|-------------|
| **SYN Flood** | Half-open TCP connections exhaust server |
| **Smurf Attack** | ICMP echo to broadcast with spoofed source IP |
| **DDoS** | Distributed attack using botnet |
| **Amplification** | Small request → large response (DNS, NTP, memcached) |

**Mitigations:** Rate limiting, ingress/egress filtering (BCP38), scrubbing centers, anycast, CDN.

### Man-in-the-Middle (MitM)
Attacker intercepts communication between two parties.
**Mitigation:** TLS/certificate validation; PKI; HTTPS; SSH known-hosts; HSTS.

---

## 4.9 Email Security

### Email Authentication Standards

| Standard | Purpose | Mechanism |
|----------|---------|----------|
| **SPF** (Sender Policy Framework) | Specifies which servers can send email for a domain | DNS TXT record |
| **DKIM** (DomainKeys Identified Mail) | Cryptographically signs emails | DNS public key; email signature |
| **DMARC** (Domain-based Message Authentication) | Policy for what to do when SPF/DKIM fail | DNS TXT record; align, quarantine, reject |

> **Exam Tip:** SPF verifies the **sending server**. DKIM verifies **message integrity** (not altered in transit). DMARC ties them together with a **policy** (none/quarantine/reject).

> **Scenario — Email Spoofing and SPF/DKIM/DMARC in Action:**
> A phishing attacker sends an email that appears to come from `ceo@company.com` to trick employees into wiring money.
>
> **SPF check:** The recipient's mail server looks up the SPF DNS record for company.com. The record lists the authorized sending mail servers (e.g., "v=spf1 include:sendgrid.net ~all"). The attacker's server (random AWS IP) isn't on the list → SPF fails.
>
> **DKIM check:** A legitimate email from company.com would have a cryptographic signature in its header, signed with the company's private DKIM key. The recipient server looks up company.com's public DKIM key in DNS and verifies the signature. The attacker doesn't have the private key → they can't produce a valid DKIM signature → DKIM fails.
>
> **DMARC policy:** company.com's DMARC record says "p=reject" — if SPF or DKIM fail, reject the message outright. The recipient's mail server sees both SPF and DKIM failed and deletes the email before it reaches anyone's inbox.
>
> **The problem without DMARC:** SPF and DKIM can both fail but the email still gets delivered unless DMARC enforces a reject/quarantine policy. Many organizations publish DMARC but leave it at "p=none" (monitor only, no enforcement) — this gives visibility but no protection.

---

## 4.10 Zero Trust Architecture

Core principle: **"Never trust, always verify"** — no implicit trust based on network location.

**Pillars:**
1. Verify explicitly — authenticate and authorize every request
2. Use least privilege access — limit access scope and duration
3. Assume breach — minimize blast radius; monitor all traffic

**Components:**
- **Identity as the perimeter** — MFA for all access
- **Micro-segmentation** — East-West traffic control
- **Continuous monitoring** — verify device health and behavior
- **ZTNA (Zero Trust Network Access)** — replaces VPN with identity-based access

---

## 4.11 Network Access Control (NAC)

Controls network access based on device posture before granting access.

**NAC checks:**
- OS version and patch level
- Antivirus presence and signature currency
- Host firewall status
- Disk encryption enabled
- Certificate-based device identity

**Enforcement modes:**
- Pre-admission NAC: device assessed **before** network access
- Post-admission NAC: device monitored **after** access (can quarantine if behavior changes)

**Quarantine VLAN** — non-compliant devices placed in restricted segment; can access remediation resources only.

> **Scenario — NAC Quarantine VLAN in Practice:**
> A contractor brings a personal laptop to plug into the corporate network. The 802.1X-enabled switch demands authentication and a posture check. The laptop:
> - Fails because it's not domain-joined (no machine certificate)
> - Has no corporate antivirus installed
> - Is running Windows 10 with 6 months of pending updates
>
> The NAC system places it in the **Quarantine VLAN** (192.168.99.0/24). In this VLAN, the laptop can reach only the patch server and an AV download server — nothing else. The contractor can install AV, run Windows Update, then re-authenticate. After passing the posture check, the NAC moves the port to the appropriate corporate VLAN.
>
> **802.1X role flow to memorize:**
> - **Supplicant** = the device wanting access (contractor's laptop)
> - **Authenticator** = the switch (enforces access control, passes credentials along)
> - **Authentication Server** = RADIUS server (validates credentials, returns allow/deny/VLAN assignment)

---

## 4.12 Network Monitoring and Detection

### Packet Capture and Analysis
| Tool | Purpose |
|------|---------|
| **Wireshark** | Protocol analyzer; captures and decodes packets |
| **tcpdump** | Command-line packet capture |
| **NetFlow** | Flow records showing who talked to whom, when, volume |
| **SPAN/mirror port** | Copies switch traffic to monitoring port for IDS/sniffer |

### Network Baselines
Documenting normal network behavior — critical for anomaly detection:
- Typical bandwidth utilization per link
- Normal connection patterns (which hosts communicate)
- Expected protocol mix
- Business-hours vs. off-hours traffic patterns

---

## 4.13 Content Distribution and Cloud Networking

### Content Delivery Networks (CDN)
- Distribute content globally; reduce latency; provide DDoS resilience
- Edge nodes absorb volumetric attacks before reaching origin

### SD-WAN (Software-Defined WAN)
- Centralizes WAN management; routes traffic intelligently across multiple links
- Security: ensure encrypted overlays; centralized policy management

### SASE (Secure Access Service Edge)
- Converges networking (SD-WAN) + security (FWaaS, CASB, ZTNA) in cloud-delivered service
- Supports remote workers without hairpinning through corporate datacenter

---

## 4.14 Protocol-Level Security Details

### TLS Versions
| Version | Status |
|---------|--------|
| SSL 2.0 / 3.0 | Broken — never use |
| TLS 1.0 / 1.1 | Deprecated — disable |
| **TLS 1.2** | Acceptable — widely supported |
| **TLS 1.3** | Preferred — faster, more secure, removed weak ciphers |

### DNS Security
| Mechanism | Purpose |
|-----------|---------|
| **DNSSEC** | Cryptographically signs DNS records; prevents cache poisoning |
| **DoH (DNS over HTTPS)** | Encrypts DNS queries to prevent eavesdropping |
| **DoT (DNS over TLS)** | Same as DoH using TLS directly |

### BGP Security
- BGP hijacking: attacker announces more-specific route to redirect traffic
- **RPKI (Resource Public Key Infrastructure)** — cryptographically validates route origins

---

## 4.15 Exam Tips Summary

- Know the **OSI model layers** and which devices/protocols operate at each.
- Always replace insecure protocols (Telnet→SSH, FTP→SFTP, HTTP→HTTPS, SNMPv1→v3).
- **IPSec ESP** provides both encryption and integrity; **AH** provides integrity only.
- **IPSec Tunnel mode** = VPN (wraps entire packet). **Transport mode** = host-to-host.
- **WEP is broken** — never a correct answer for "best" wireless security.
- **WPA2-Enterprise** (802.1X) is the minimum for corporate wireless.
- **ARP poisoning** enables MitM attacks at Layer 2 — mitigated by Dynamic ARP Inspection.
- **Split tunneling** is a security risk in VPNs.
- Email security: SPF + DKIM + DMARC work together; DMARC enforces the policy.
- **Zero Trust** = identity-based, assume breach, verify everything — no implicit trust.
- **TLS 1.3** is the current preferred version — TLS 1.0/1.1 are deprecated.
- **802.1X** = port-based NAC: supplicant → authenticator → RADIUS server.
- **EAP-TLS** requires client certificates (strongest). **PEAP** only requires server certificate.
- **NAC** assesses device posture before granting network access — non-compliant goes to quarantine VLAN.
- **DNSSEC** prevents DNS cache poisoning by signing DNS records cryptographically.
