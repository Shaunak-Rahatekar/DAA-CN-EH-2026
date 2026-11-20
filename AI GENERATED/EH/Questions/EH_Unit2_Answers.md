# Ethical Hacking (EH) — Unit II Model Answers & Marking Scheme
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit II**: Footprinting, Reconnaissance and Scanning

---

## Answer 2.1 [4 Marks]
### Marking Scheme:
* **Part (a) Passive vs Active Recon Matrix**: 2 Marks (0.5 marks per parameter)
* **Part (b) WHOIS & DNS Enumeration + 4 Records**: 2 Marks (1 mark explanation + 1 mark for 4 records)

### Model Answer:
#### Part (a) Passive vs Active Reconnaissance Comparison

| Parameter | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Target Contact** | Zero direct traffic sent to target systems; queries third parties. | Transmits crafted packets directly to target network interfaces. |
| **Detectability** | Undetectable by target firewalls, IDS, and SIEM platforms. | Highly visible; recorded in network logs, triggers IDS/WAF alerts. |
| **Tooling Used** | WHOIS, Public DNS, Google Dorking, Shodan, theHarvester. | Nmap, Ping sweeps, Port scanners, Nessus, Netcat, Nikto. |
| **Legal Risk** | Minimal (accessing publicly indexed and open records). | Requires explicit written Scope and Rules of Engagement (RoE). |

#### Part (b) WHOIS & DNS Enumeration
1. **Technical Purposes**:
   * **WHOIS (TCP Port 43)**: Queries regional internet registries (RIRs) to uncover domain ownership, registrant contact information, registration/expiration milestones, and authoritative name servers.
   * **DNS Enumeration (UDP/TCP Port 53)**: Systematically maps all domain-to-IP and host mappings to reveal an organization's network perimeter, mail infrastructure, and internal subdomains.
2. **Four Critical Resource Records (RR)**:
   * **A Record**: Maps Fully Qualified Domain Names (FQDN) to 32-bit IPv4 addresses (e.g., `portal.target.com -> 198.51.100.10`).
   * **MX Record**: Identifies Mail Exchange hosts handling corporate email, revealing email filtering gateways (e.g., Proofpoint, Google Workspace).
   * **NS Record**: Specifies authoritative DNS name servers hosting the zone.
   * **TXT Record**: Contains SPF (`v=spf1 ...`), DKIM, and DMARC policies, which leak authorized third-party mailers and cloud providers.

---

## Answer 2.2 [4 Marks]
### Marking Scheme:
* **Google Dorking Concept**: 1 Mark
* **4 Search Operators with Practical Examples**: 2 Marks (0.5 marks each)
* **`robots.txt` Misconception & Defense**: 1 Mark

### Model Answer:
1. **Google Dorking (Google Hacking) Concept**:
   Google Dorking leverages advanced search engine syntax and boolean operators to filter Google’s search index, locating sensitive files, unlinked admin panels, configuration scripts, and confidential data exposed to web crawlers without proper access restrictions.

2. **Four Essential Operators & Practical Attack Examples**:
   * **`site:`**: Restricts searches to a specific target domain.
     * *Example*: `site:target.com inurl:login` (Discovers exposed corporate authentication endpoints).
   * **`filetype:` (or `ext:`)**: Restricts queries to specified file types.
     * *Example*: `site:target.com filetype:sql "INSERT INTO"` (Finds publicly exposed database backup dumps containing plaintext records).
   * **`intitle:`**: Searches within HTML page title tags.
     * *Example*: `site:target.com intitle:"Index of /"` (Locates open web server directory listings where default indexing is enabled).
   * **`inurl:`**: Matches strings located inside the URL path.
     * *Example*: `site:target.com ext:env "DB_PASSWORD"` (Uncovers exposed environment configuration files containing database master credentials).

3. **The `robots.txt` Misconception**:
   * **False Assumption**: Administrators mistakenly believe placing paths in `robots.txt` hides or secures private pages.
   * **Reality**: `robots.txt` is purely an advisory protocol (Robots Exclusion Standard) meant for polite web crawlers. It enforces **zero authentication or access control**.
   * **Adversarial Exploitation**: Attackers routinely inspect `http://target.com/robots.txt` to read `Disallow:` directives, which serve as a public roadmap to the organization's most sensitive, hidden administrative folders (e.g., `Disallow: /admin_backup/`).
   * **Proper Mitigation**: Enforce server-side authentication (e.g., HTTP Basic Auth, JWT, OAuth2) and disable server directory listings (`Options -Indexes`).

---

## Answer 2.3 [8 Marks]
### Marking Scheme:
* **Part (a) DNS Zone Transfer (AXFR) Mechanism & Exploit**: 4 Marks
  * Mechanism & Risk: 2 Marks
  * CLI Command: 1 Mark
  * Hardening in BIND: 1 Mark
* **Part (b) TCP Connect vs TCP SYN Stealth Scan**: 4 Marks
  * Sequence Diagrams / Packet Flows: 2 Marks
  * Log Avoidance Explanation: 1 Mark
  * OS Privilege Comparison: 1 Mark

### Model Answer:
#### Part (a) DNS Zone Transfer (AXFR) Mechanism & Threat

```
Legitimate Sync:   [Primary Master DNS] <── AXFR (TCP 53) ── [Secondary Slave DNS]
                                 │
Unrestricted Flaw:               ▼
Leaked Zone Dump:  [Primary Master DNS] ─── AXFR Dump ─────> [Unauthorized Attacker]
                   (Leaks vpn.target.com, staging.target.com, internal subnets)
```

1. **Mechanism**:
   * In a redundant DNS setup, primary (master) name servers replicate their entire zone database to secondary (slave) name servers using an **Asynchronous Full Transfer (AXFR)** query running over **TCP Port 53**.
2. **Enterprise Threat**:
   * If the primary server is misconfigured with `allow-transfer { any; };`, any external actor can request an AXFR transfer.
   * The server dumps its complete record database, exposing the entire internal network topology: hidden intranet subdomains (`dev.company.com`, `vpn.company.com`), development servers, database endpoints, and private RFC 1918 IP addresses.
3. **Exploitation Command**:
   ```bash
   dig @ns1.target.com target.com AXFR
   # Alternative using host:
   host -l target.com ns1.target.com
   ```
4. **BIND DNS Hardening**:
   * In `/etc/bind/named.conf.local`, restrict transfers strictly to trusted secondary slave IP addresses:
     ```text
     zone "target.com" {
         type master;
         file "/etc/bind/db.target.com";
         allow-transfer { 192.168.1.15; };  // Restrict to authorized slave IP
     };
     ```
   * Enforce **Transaction Signatures (TSIG)** using shared cryptographic secret keys to authenticate all zone transfer requests between master and slave.

---

#### Part (b) TCP Connect Scan (`-sT`) vs TCP SYN Stealth Scan (`-sS`)

```
1. TCP Connect Scan (-sT)                2. TCP SYN Stealth Scan (-sS)
Attacker                 Target          Attacker                 Target
   │                        │               │                        │
   ├─── TCP SYN ──────────> │               ├─── TCP SYN ──────────> │
   │ <── TCP SYN/ACK ───────┤               │ <── TCP SYN/ACK ───────┤
   ├─── TCP ACK ──────────> │ (Established) └─── TCP RST ──────────> │ (Aborted!)
   └─── TCP RST ──────────> │ (Teardown)
```

1. **Packet Exchange Differences**:
   * **TCP Connect (`-sT`)**: Executes the full 3-way handshake using the OS `connect()` API call (`SYN` $\rightarrow$ `SYN/ACK` $\rightarrow$ `ACK`). Once established, it sends a `RST` to cleanly close the connection.
   * **TCP SYN Stealth (`-sS`)**: Executes a "half-open" scan. The tester sends `SYN`, receives `SYN/ACK` (confirming the port is open), and immediately transmits a **`RST` packet** instead of the completing `ACK` packet.
2. **Why SYN Scans Avoid Application Access Logs**:
   * Application servers (e.g., Apache HTTPD, Nginx, IIS) do not handle raw transport layer packets directly. The operating system kernel only passes an incoming socket connection up to the application daemon *after* the 3-way handshake successfully completes via the `accept()` system call.
   * Because the SYN scan transmits a `RST` before sending the final `ACK`, the connection is reset at the kernel level. The application daemon is never notified of a connection, and **zero entries are written to web or application access logs**.
3. **Privilege Requirements**:
   * **TCP Connect (`-sT`)**: Uses standard high-level socket system calls; requires **unprivileged standard user rights**.
   * **TCP SYN (`-sS`)**: Requires direct raw packet crafting and raw socket manipulation (`SOCK_RAW`); requires **root / administrator privileges**.

---

## Answer 2.4 [8 Marks]
### Marking Scheme:
* **Part (a) Nmap Port States (Open, Closed, Filtered)**: 4 Marks (1 mark definition/packet received + 1 mark firewall mechanism)
* **Part (b) RFC 793 Inverse Scans (FIN, Null, Xmas)**: 4 Marks (1.5 marks principle & behavior + 1.5 marks stateless bypass + 1 mark Windows exception)

### Model Answer:
#### Part (a) Nmap Port States & Firewall Interaction

```
[Attacker SYN Probe]
         │
         ├───> [Target: Listening Daemon] ──> Returns TCP SYN/ACK  ──> [State: OPEN]
         ├───> [Target: No Daemon Active] ──> Returns TCP RST      ──> [State: CLOSED]
         └───> [Firewall Drops Packet]    ──> Timeout / ICMP Code 13──> [State: FILTERED]
```

1. **Open State**:
   * *Packet Received*: **TCP SYN/ACK**.
   * *Meaning*: A target application daemon is actively listening on the port and accepted the connection.
2. **Closed State**:
   * *Packet Received*: **TCP RST** (Reset).
   * *Meaning*: The target host received the probe, but no application daemon is currently bound to or listening on that port.
3. **Filtered State**:
   * *Packet Received*: **No response (Connection Timeout)** OR an **ICMP Type 3 Destination Unreachable error** (specifically Code 13: Communication Administratively Prohibited, Code 1: Host Unreachable, or Code 3: Port Unreachable).
   * *Firewall Mechanism*: An intermediate network firewall, host-based firewall (e.g., `iptables`, Windows Defender Firewall), or router ACL intercepts the incoming probe before it reaches the listening socket. The firewall either drops the packet silently (blackholing) or actively rejects it with an ICMP administrative prohibition message.

---

#### Part (b) RFC 793 Inverse Scans (FIN, Null, and Xmas Tree)

1. **Working Principle**:
   Inverse scans exploit a loophole in **RFC 793** (page 65) governing TCP error handling:
   * **Null Scan (`-sN`)**: No flags set (`0x00`).
   * **FIN Scan (`-sF`)**: Only the FIN flag set (`0x01`).
   * **Xmas Tree Scan (`-sX`)**: FIN, PSH, and URG flags set simultaneously (`0x29`), lighting the packet up like a Christmas tree.
2. **Port State Determination**:
   * If the target port is **CLOSED**: RFC 793 specifies that the receiving host kernel must generate and return a **TCP RST** packet.
   * If the target port is **OPEN**: The malformed packet without SYN, ACK, or RST is discarded silently — resulting in **NO RESPONSE**.
   * If no response is received, Nmap reports `Open|Filtered`.
3. **Bypassing Stateless Packet-Filtering Firewalls**:
   * Simple stateless packet-filtering firewalls block incoming connections by checking for the `SYN` flag on incoming packets (blocking new connection initiations while allowing established replies).
   * Because FIN, Null, and Xmas packets contain **no SYN flag**, stateless firewall rules allow them to pass through unhindered to the target OS stack.
4. **The Microsoft Windows Failure Exception**:
   * Inverse scans **fail completely against Microsoft Windows, Cisco, and BSDI devices**.
   * Microsoft’s TCP/IP stack violates RFC 793: Windows hosts reply with a `RST` packet to **all** malformed packets, regardless of whether the port is open or closed. Consequently, all ports on a Windows target appear "Closed". These scans only function against RFC-compliant Unix/Linux/BSD systems.

---

## Answer 2.5 [10 Marks]
### Marking Scheme:
* **Part (a) Scanning Strategy & Banner Grabbing**: 5 Marks
  * Nmap Command Execution Chain: 2.5 Marks (Host discovery, port/version scan, NSE vuln scan)
  * Banner Grabbing (HTTP/SMTP) & Sanitization: 2.5 Marks
* **Part (b) Vulnerability Assessment Architecture**: 5 Marks
  * 5-Phase VA Lifecycle: 2.5 Marks
  * Credentialed vs Non-Credentialed Matrix & Tool Names: 2.5 Marks

### Model Answer:
#### Part (a) Port Scanning Strategy & Banner Grabbing

1. **Step-by-Step Nmap Command Execution**:
   * *Step 1: Host Discovery without Port Scanning*:
     ```bash
     nmap -sn -PR 192.168.10.0/24
     # If ICMP is blocked across firewalls, skip discovery:
     nmap -Pn 192.168.10.0/24
     ```
   * *Step 2: Comprehensive TCP Port, Version & OS Scan*:
     ```bash
     nmap -sS -sV -O -p- -T4 192.168.10.50 -oN initial_scan.txt
     ```
     *Flags Explained*: `-sS` (SYN Stealth scan), `-sV` (Service version probing), `-O` (OS detection), `-p-` (all 65,535 ports), `-T4` (aggressive speed template), `-oN` (save formatted text output).
   * *Step 3: Automated Vulnerability Assessment via NSE*:
     ```bash
     nmap --script vuln -p 80,443,445,21,22 192.168.10.50 -oN vuln_results.txt
     ```

2. **Banner Grabbing Mechanics**:
   * **HTTP (Port 80)**:
     ```bash
     curl -I http://192.168.10.50
     # Server Response:
     # HTTP/1.1 200 OK
     # Server: Apache/2.4.41 (Ubuntu)
     ```
   * **SMTP (Port 25)**:
     ```bash
     nc -v 192.168.10.50 25
     # Server Response:
     # 220 mail.corporate.com ESMTP Postfix (Debian)
     ```
3. **Defensive Banner Sanitization**:
   * *Apache HTTP Server*: In `httpd.conf` or `security.conf`, set:
     ```text
     ServerTokens Prod
     ServerSignature Off
     ```
     (Strips version numbers and OS names, displaying only generic `Server: Apache`).
   * *Nginx*: In `nginx.conf`, set:
     ```text
     server_tokens off;
     ```

---

#### Part (b) Vulnerability Assessment (VA) Architecture & Scan Paradigms

1. **5-Phase Vulnerability Assessment Lifecycle**:
   ```
   [ 1. Scope Definition ] ──> [ 2. Information Discovery ] ──> [ 3. Vulnerability Scan ]
                                                                          │
   [ 5. Remediation & Re-test ] <── [ 4. Analysis & CVSS Scoring ] <──────┘
   ```
   * **Phase 1: Scope & Planning**: Define IP targets, scan windows, compliance requirements, and exclusions.
   * **Phase 2: Information Discovery**: Run host sweeps, active port mapping, and daemon identification.
   * **Phase 3: Vulnerability Scanning**: Automated scanner queries active services and compares signatures against vulnerability databases.
   * **Phase 4: Vulnerability Analysis & Scoring**: Categorize discovered flaws using CVSS v3.1 base metrics (Severity: Low, Medium, High, Critical) and weed out obvious false positives.
   * **Phase 5: Reporting, Remediation & Verification**: Issue technical and executive remediation reports; verify patch deployment via follow-up scans.

2. **Credentialed (Authenticated) vs Non-Credentialed (Unauthenticated) Scans**:

| Evaluation Criterion | Non-Credentialed (External) Scan | Credentialed (Internal) Scan |
| :--- | :--- | :--- |
| **Testing Perspective** | External remote adversary targeting public perimeter. | Malicious insider or compromised workstation. |
| **Authentication Mode**| Zero credentials; interacts only with listening ports. | Authenticates via SSH (Linux) or SMB/WMI (Windows). |
| **Audit Depth** | Limited to network banners and externally visible flaws. | Deep internal audit: unpatched DLLs, registry keys, software versions, missing hotfixes. |
| **False Positive Rate**| **High** (misinterprets backported security patches). | **Near Zero** (inspects exact file hashes and patch IDs). |
| **System Overhead** | Generates network firewall traffic; low host CPU. | Minimal network packets; brief host CPU overhead. |

3. **Two Industry-Standard Automated Vulnerability Scanners**:
   * **Tenable Nessus**: Enterprise vulnerability scanner featuring extensive plugin feeds.
   * **OpenVAS (Greenbone Vulnerability Management)**: Comprehensive open-source vulnerability scanner maintaining community Network Vulnerability Tests (NVTs).
