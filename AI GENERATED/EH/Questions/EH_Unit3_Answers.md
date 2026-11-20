# Ethical Hacking (EH) — Unit III Model Answers & Marking Scheme
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit III**: System Hacking and Network Security

---

## Answer 3.1 [4 Marks]
### Marking Scheme:
* **Part (a) Password Spraying & Credential Stuffing**: 2 Marks (1 mark definitions + 1 mark lockout evasion mechanism)
* **Part (b) Password Salting & Rainbow Table Neutralization**: 2 Marks (1 mark definition/formula + 1 mark rainbow table explanation)

### Model Answer:
#### Part (a) Password Spraying vs Credential Stuffing
1. **Definitions**:
   * **Password Spraying**: An online horizontal password attack where an adversary attempts a single common, high-probability password (e.g., `Company@2026!`, `Summer2026`) across thousands of different employee usernames.
   * **Credential Stuffing**: An automated online attack that injects vast databases of stolen username/password pairs harvested from past breaches across unrelated corporate portals, exploiting widespread end-user password reuse.
2. **Why Password Spraying Bypasses Account Lockout Policies**:
   * Standard account lockout policies lock an account after $N$ consecutive failed attempts (typically 3 to 5 failed attempts within 15 minutes) on a **single user account**.
   * In a traditional brute-force attack, testing hundreds of passwords on user `alice` triggers lockout within seconds.
   * In Password Spraying, the attacker tests only **one password per user account** across thousands of accounts, then pauses until the lockout reset window expires before trying the next candidate password. Because no individual account receives more than 1 failed attempt per cycle, the security threshold is never breached.

#### Part (b) Cryptographic Salting & Rainbow Table Neutralization
1. **Concept & Mathematical Formula**:
   A **Salt** is a cryptographically strong, non-secret random bit-string (at least 16 to 32 bytes) generated uniquely for each user during account creation. It is appended or prepended to the plaintext password before hashing:
   $$\text{Stored Digest} = \text{Hash}(\text{Password} \,\|\, \text{Salt})$$
   Both the salt and resulting digest are stored in the authentication database (e.g., in `/etc/shadow`).
2. **How Salting Neutralizes Rainbow Table Attacks**:
   * A **Rainbow Table** is a massive precomputed lookup table storing millions of plaintext passwords and their corresponding cryptographic hash digests, enabling instant $O(1)$ reverse hash lookups.
   * Rainbow tables are effective only when identical passwords always produce the exact same hash (deterministic fast hashing).
   * **Salt Impact**: When unique salts are applied, even if 10,000 corporate users set their password to `Password123`, each user has a unique random salt, resulting in 10,000 completely unique hash digests.
   * An attacker would be forced to precompute a dedicated billion-entry rainbow table for **each unique user salt**, rendering precomputation computationally and storage infeasible.

---

## Answer 3.2 [4 Marks]
### Marking Scheme:
* **Part (a) Virus vs Worm Comparison**: 2 Marks (Host dependence, propagation autonomy, velocity)
* **Part (b) Rootkit Stealth & System Call Subversion**: 2 Marks (Definition, kernel hook mechanism, Task Manager blind spot)

### Model Answer:
#### Part (a) Computer Virus vs Computer Worm

| Evaluation Dimension | Computer Virus | Computer Worm |
| :--- | :--- | :--- |
| **Host Dependency** | **Parasitic**: Cannot exist independently; must attach itself to a legitimate host executable (`.exe`, `.dll`) or macro document. | **Standalone**: Independent, complete program; operates without attaching to host files. |
| **Propagation Autonomy** | **Requires Human Execution**: Spreads only when a human user executes the infected file, opens an attachment, or runs a macro. | **Fully Autonomous**: Self-propagates across networks automatically by exploiting unpatched network vulnerabilities (e.g., EternalBlue). |
| **Infection Velocity** | Slower; dependent on human file-sharing behaviors. | Exponentially fast; can compromise hundreds of thousands of network hosts within minutes (e.g., WannaCry, Conficker). |

#### Part (b) Rootkit Stealth Mechanics
1. **Definition**:
   A **Rootkit** is a collection of stealthy malicious software tools designed to maintain root/administrative access to a computer while actively subverting operating system integrity to conceal its presence and that of other associated malware.
2. **Subversion of System Calls**:
   * Rootkits operate at the OS kernel level (Ring 0) or user-space API layer (Ring 3).
   * In a kernel-mode rootkit, the malware hooks into the operating system’s **System Call Table (e.g., `sys_call_table` in Linux)** or modifies the Kernel Object Tree (Direct Kernel Object Manipulation - DKOM).
   * When user-space administrative tools (such as Task Manager, `ps`, `netstat`, or `dir`) make a system call (e.g., `sys_getdents` to list directory contents or enumerate running processes), the rootkit intercepts the call, filters out its own process IDs (PIDs), file names, and open listening sockets, and returns a sanitized result to the user.
3. **Why Task Manager Cannot Detect It**:
   Because Task Manager relies entirely on querying the underlying OS kernel API (`NtQuerySystemInformation`), and the kernel API itself has been hooked and tampered with by the rootkit, Task Manager receives altered data and is physically blind to the hidden malicious processes.

---

## Answer 3.3 [8 Marks]
### Marking Scheme:
* **Part (a) Vertical vs Horizontal Privilege Escalation**: 4 Marks (2 marks definition & diagram + 2 marks concrete examples)
* **Part (b) John the Ripper vs Hashcat Comparison**: 4 Marks (2.5 marks comparison matrix + 1.5 marks exact John command syntax)

### Model Answer:
#### Part (a) Vertical vs Horizontal Privilege Escalation

```
1. Vertical Privilege Escalation            2. Horizontal Privilege Escalation
   [ Root / Administrator ]                    [ User A (Attacker) ]
              ▲                                         │
              │ Kernel Exploit / SUID                   ▼ (IDOR Parameter Tamper)
              │                                [ User B (Victim) ]
   [ Standard User (Unprivileged) ]            (Same privilege tier: Customer)
```

1. **Definitions & Mechanics**:
   * **Vertical Privilege Escalation**: An attacker with low-privilege access elevates their privileges to a higher security boundary (e.g., from `www-data` or guest user to `root` or Windows `NT AUTHORITY\SYSTEM`).
   * **Horizontal Privilege Escalation**: An attacker accesses resources, functions, or data belonging to another user who holds the exact same privilege tier (e.g., Customer A accessing Customer B’s bank statement).
2. **Concrete Exploitation Examples**:
   * **Vertical Example (Linux)**:
     * *SUID Binary Shell Escape*: An administrator configures the SUID bit on the `find` binary (`chmod u+s /usr/bin/find`). A low-privilege user executes:
       ```bash
       find . -exec /bin/sh -p \; -quit
       ```
       The binary executes with the EUID of the owner (`root`), spawning an interactive root shell.
   * **Horizontal Example (Web / Insecure Direct Object Reference - IDOR)**:
     * A banking portal displays statements via:
       `GET /api/v1/statement?account_id=10042`
     * User A (authenticated as Account 10042) modifies the parameter to `account_id=10043`. If the backend application queries the database using the supplied parameter without validating whether Account 10043 belongs to the currently logged-in session, User A views User B's private financial records.

---

#### Part (b) John the Ripper vs Hashcat Comparison

| Parameter | John the Ripper (JtR) | Hashcat |
| :--- | :--- | :--- |
| **Primary Architecture**| Highly optimized for **CPU-based** multi-threading; extensible plugin framework. | World’s fastest **GPU-accelerated** cracking engine (OpenCL / CUDA). |
| **Hardware Utilization**| Uses CPU SSE/AVX vector instruction sets; lower throughput for fast hashes. | Uses thousands of GPU stream processors; calculates billions of hashes/sec. |
| **Attack Modes** | Single crack mode (mangling username), Wordlist mode, Incremental (brute force). | Straight (`-a 0`), Combination (`-a 1`), Mask (`-a 3`), Hybrid (`-a 6`). |
| **Enterprise Usage** | Fast auditing of unshadowed Unix/Linux shadow files and complex password hashes. | High-speed enterprise cracking rigs targeting NTLM, WPA2 handshakes, and MD5. |

#### Exact John the Ripper Command Syntax:
1. Combine `/etc/passwd` and `/etc/shadow` into a single cracking format:
   ```bash
   unshadow /etc/passwd /etc/shadow > /tmp/unshadowed.txt
   ```
2. Execute dictionary crack using `rockyou.txt` wordlist with mangling rules:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt --rules /tmp/unshadowed.txt
   ```
3. Display all successfully cracked plaintext passwords:
   ```bash
   john --show /tmp/unshadowed.txt
   ```

---

## Answer 3.4 [8 Marks]
### Marking Scheme:
* **Part (a) ARP Vulnerability & Step-by-Step MITM Flow**: 5 Marks (1 mark vulnerability + 3 marks step-by-step walkthrough + 1 mark diagram)
* **Part (b) 3 Enterprise Defenses & DAI with DHCP Snooping**: 3 Marks (1.5 marks DAI explanation + 1.5 marks Port Security & Static ARP)

### Model Answer:
#### Part (a) ARP Architectural Vulnerability & Step-by-Step MITM Walkthrough

```
                      +-----------------------------+
                      |   ATTACKER (192.168.1.50)   |
                      |     MAC: CC:CC:CC:CC:CC:CC  |
                      +-----------------------------+
                        ▲                         ▲
       Forged ARP Reply |                         | Forged ARP Reply
  "192.168.1.1 is at CC"|                         | "192.168.1.10 is at CC"
                        ▼                         ▼
+-----------------------------+             +-----------------------------+
|    VICTIM (192.168.1.10)    |             |   GATEWAY (192.168.1.1)     |
|     MAC: AA:AA:AA:AA:AA:AA  |             |     MAC: BB:BB:BB:BB:BB:BB  |
+-----------------------------+             +-----------------------------+
| Poisoned Cache:             |             | Poisoned Cache:             |
| 192.168.1.1 -> CC:CC:CC     |             | 192.168.1.10 -> CC:CC:CC    |
+-----------------------------+             +-----------------------------+
```

1. **Inherent Architectural Vulnerability of ARP (RFC 826)**:
   * **Stateless and Unauthenticated**: ARP has no mechanism to verify the sender of a message.
   * **Accepts Unsolicited Replies**: Operating systems accept and process **Gratuitous ARP replies** without ever having issued an ARP request.
   * **Blind Cache Overwrite**: Any device receiving an ARP reply automatically overwrites its internal IP-to-MAC mapping table without verification.

2. **Step-by-Step Attack Walkthrough**:
   * **Step 1: Attacker Probes Subnet**: Attacker identifies Victim IP (`192.168.1.10`, MAC `AA`), Gateway IP (`192.168.1.1`, MAC `BB`), and Attacker MAC (`CC`).
   * **Step 2: Poisoning the Victim**: Attacker continuously transmits forged gratuitous ARP replies to the Victim stating:
     `"192.168.1.1 is at MAC CC:CC:CC:CC:CC:CC"`
     *Result*: Victim updates its ARP table: `192.168.1.1 -> CC:CC:CC:CC:CC:CC`.
   * **Step 3: Poisoning the Gateway**: Attacker sends forged gratuitous ARP replies to the Gateway stating:
     `"192.168.1.10 is at MAC CC:CC:CC:CC:CC:CC"`
     *Result*: Gateway updates its ARP table: `192.168.1.10 -> CC:CC:CC:CC:CC:CC`.
   * **Step 4: Enabling IP Forwarding**: Attacker enables kernel forwarding (`sysctl -w net.ipv4.ip_forward=1`).
   * **Step 5: MITM Interception**: When Victim transmits web traffic to the internet, Layer 2 frames are addressed to Attacker's MAC `CC`. Attacker captures or alters the payload (via Wireshark or Ettercap) and forwards the frames to Gateway MAC `BB`. Return traffic from the Gateway is similarly intercepted.

---

#### Part (b) Enterprise Defensive Controls against ARP Spoofing

1. **Dynamic ARP Inspection (DAI)**:
   * Enterprise managed switches inspect all incoming ARP packets on untrusted switch ports.
   * DAI relies on the **DHCP Snooping binding database**, which records trusted IP-to-MAC-to-Port mappings during DHCP assignment.
   * When an ARP packet arrives on a switch port, DAI cross-checks the sender's MAC and IP against the DHCP Snooping table. If an ARP reply claims an IP address not bound to that specific MAC address, the switch **immediately drops the forged ARP packet** and logs an alert.
2. **Switch Port Security**:
   * Restricts physical switch ports to a fixed number of authorized MAC addresses (e.g., maximum 1 MAC per port using "Sticky MAC"). If an attacker attempts to inject multiple spoofed MAC addresses or flood the CAM table, the port enters `err-disable` shutdown state.
3. **Static ARP Tables & End-to-End Encryption**:
   * Configure static, immutable ARP cache entries for critical enterprise default gateways (`arp -s 192.168.1.1 BB-BB-BB-BB-BB-BB`).
   * Enforce **TLS 1.3, SSH, and IPsec VPNs** so that even if an attacker successfully achieves a MITM position, all intercepted payloads remain strongly encrypted and unreadable.

---

## Answer 3.5 [10 Marks]
### Marking Scheme:
* **Part (a.1) 4 Generations of Firewalls Comparison Matrix**: 4 Marks (1 mark per generation)
* **Part (a.2) Screened-Subnet DMZ Architecture & Traffic Rules**: 2 Marks (1 mark diagram/services + 1 mark access rules)
* **Part (b.1) IDS vs IPS Technical Comparison**: 2 Marks (Deployment, action, latency, failure modes)
* **Part (b.2) Signature vs Anomaly-Based Detection Comparison**: 2 Marks (Definitions, zero-day blindness, false positive analysis)

### Model Answer:
#### Part (a.1) Four Generations of Firewall Technologies

| Generation / Type | OSI Layer | Inspection Depth & State Tracking | Performance & Limitations |
| :--- | :--- | :--- | :--- |
| **1. Packet Filtering** | Network / Transport (Layers 3 & 4) | **Stateless**: Evaluates individual packets in isolation against static ACLs based on Source/Dest IP, Protocol, and Port. | **Fastest throughput**, lowest latency. Cannot inspect payloads or verify connection states; vulnerable to IP spoofing. |
| **2. Stateful Inspection** | Network / Transport / Session (Layers 3, 4, 5) | **Stateful**: Maintains a dynamic **State Table** tracking connection states (`NEW`, `ESTABLISHED`, `RELATED`). Automatically permits return packets for valid sessions. | High throughput. Prevents port scanning and flag spoofing, but blind to malicious application-layer payloads (e.g., SQLi, malware). |
| **3. Application Proxy** | Application (Layer 7) | **Deep Content Inspection**: Terminates incoming connections, fully inspects application protocol payload (HTTP, FTP, SMTP), and regenerates a new connection to destination. | **Highest latency**, CPU-heavy; cannot scale to high-bandwidth enterprise cores. Hides internal network topology completely. |
| **4. Next-Gen Firewall (NGFW)** | Layers 3 through 7 | **Deep Packet Inspection (DPI)** + Native inline **IPS** + **Application Awareness** (identifies BitTorrent or Skype tunneling over port 443) + Cloud threat intelligence. | Enterprise standard; balances high-speed hardware ASICs with comprehensive layer 7 malware, threat, and application filtering. |

---

#### Part (a.2) Screened-Subnet DMZ Architecture

```
                       [ UNTRUSTED INTERNET ]
                                 │
                                 ▼
                     [ EXTERNAL FIREWALL ]
                                 │
            ┌────────────────────┴────────────────────┐
            ▼                                         ▼
    [ DMZ SUBNET ]                         [ INTERNAL FIREWALL ]
 * Public Web Server (Port 80/443)                    │
 * Mail Relay Server (SMTP Port 25)                   ▼
 * Public DNS Server (Port 53)              [ TRUSTED INTERNAL LAN ]
                                            * Corporate Workstations
                                            * Transaction Databases
                                            * Active Directory Core
```

1. **Services Hosted in the DMZ**:
   * Only publicly accessible services are deployed in the Demilitarized Zone (DMZ): **Public Web Servers, Mail Relays (MTA), Reverse Proxies, and External DNS**.
2. **Fundamental Firewall Access Rule**:
   * **RULE 1 (Inbound from Internet)**: Permitted strictly to DMZ services on specific ports (e.g., 443, 25).
   * **RULE 2 (DMZ to Internal LAN)**: **STRICTLY FORBIDDEN (DENY ALL)** by default. Direct inbound connection initiation from DMZ hosts into the internal LAN is completely blocked.
   * *Exception*: DMZ web servers may connect to internal database servers only over specific, restricted ports (e.g., port 3306) via explicit internal firewall rules.

---

#### Part (b.1) Intrusion Detection System (IDS) vs Intrusion Prevention System (IPS)

| Technical Dimension | Intrusion Detection System (IDS) | Intrusion Prevention System (IPS) |
| :--- | :--- | :--- |
| **Deployment Mode** | **Out-of-band**: Connected via switch **SPAN / Mirror port** or physical network TAP. | **In-line**: Placed directly in the physical communication pathway (between router and switch). |
| **Operational Action** | **Passive**: Analyzes a copy of traffic, logs events, and generates alerts to SIEM. | **Active**: Inspects live traffic in real-time; **drops packets** and transmits TCP `RST` to abort sessions. |
| **Network Latency Impact**| **Zero latency**: Failure or congestion of IDS has zero impact on live network throughput. | **Introduces minor latency**: Every packet must be processed by the inspection engine before forwarding. |
| **Failure Mode** | **Fail-Open**: If the IDS crashes, live production network traffic continues unhindered. | **Fail-Closed or Fail-Bypass**: Hardware bypass switches are required to prevent network blackout upon crash. |

---

#### Part (b.2) Signature-Based vs Anomaly-Based Detection Engines

1. **Signature-Based Detection**:
   * *Mechanism*: Compares observed packet bytes and header patterns against a database of known threat signatures (e.g., a known Snort rule for Log4j: `${jndi:ldap://...}`).
   * *Strength*: Extremely fast, highly accurate, and produces **virtually zero false positives** for known threats.
   * *Zero-Day Blindness*: Completely ineffective against **Zero-Day (0-day) vulnerabilities** and novel polymorphic malware because no signature has yet been authored or published.
2. **Anomaly-Based (Behavioral / Heuristic) Detection**:
   * *Mechanism*: Uses statistical baselines or machine learning models to profile "normal" network behavior (e.g., average bandwidth, typical port usage, normal login hours). Alerts whenever traffic deviates significantly beyond predefined statistical thresholds.
   * *Strength*: Highly effective at detecting **Zero-Day exploits**, targeted advanced persistent threats (APTs), and anomalous insider data exfiltration.
   * *High False Positive Rate*: Any legitimate, sudden surge in network activity (e.g., an unexpected corporate software update, quarterly report upload, or holiday sales traffic spike) deviates from historical baselines and triggers false alarm floods, causing alert fatigue for SOC analysts.
