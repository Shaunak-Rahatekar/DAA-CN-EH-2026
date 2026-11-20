# Unit II: Footprinting, Reconnaissance and Scanning
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Faculty / Curriculum**: AY 2026-27 | **Exam Date**: 27 Nov 2026

---

## 1. Introduction to Footprinting and Reconnaissance

Reconnaissance (or Footprinting) is the preparatory first phase of ethical hacking. Its objective is to accumulate maximum actionable intelligence regarding the target organization’s digital footprint, network infrastructure, host systems, employee details, and external perimeter without causing disruption.

```
+---------------------------------------------------------------------------------------+
|                                RECONNAISSANCE TAXONOMY                                |
+-------------------------------------------+-------------------------------------------+
|          PASSIVE RECONNAISSANCE           |           ACTIVE RECONNAISSANCE           |
+-------------------------------------------+-------------------------------------------+
| * Indirect information gathering.         | * Direct packet interaction with target.  |
| * Zero direct traffic sent to target.     | * Packets touch target firewalls/hosts.   |
| * Completely stealthy; undetectable.      | * Logged by IDS, IPS, Firewalls, SIEM.    |
| * Sources: OSINT, WHOIS, DNS, Dorking.    | * Tools: Nmap, Ping sweeps, Port probes.  |
+-------------------------------------------+-------------------------------------------+
```

### 1.1 Passive Reconnaissance
* **Concept**: The attacker or penetration tester gathers data from third-party intermediaries, public databases, search engine caches, and public registries. The target’s intrusion detection systems (IDS) and firewalls record zero inbound connection attempts from the tester's IP address.
* **Key Targets**: Domain names, IP address blocks (CIDR allocations), autonomous system numbers (ASNs), employee email conventions, tech stack job advertisements, and organizational hierarchy.

### 1.2 Active Reconnaissance
* **Concept**: The tester sends crafted network packets directly to target interfaces, routers, and firewalls to elicit operational responses.
* **Risk**: High visibility. Active probes trigger alert thresholds on Web Application Firewalls (WAF), Network Intrusion Detection Systems (NIDS), and Endpoint Detection and Response (EDR) platforms.
* **Scope Control**: Must be rigorously conducted only within agreed testing windows and IP boundaries established in the client's Rules of Engagement (RoE).

---

## 2. Open Source Intelligence (OSINT) & Passive Reconnaissance Techniques

OSINT involves collecting, processing, and analyzing publicly available data to construct an actionable threat profile.

### 2.1 WHOIS Reconnaissance
WHOIS is a query and response protocol (RFC 3912, TCP port 43) used for querying databases that store the registered users or assignees of an Internet resource:
* **Information Disclosed**:
  * Registrar Name and IANA ID.
  * Domain Registration, Update, and Expiration Dates.
  * Registrant Name, Organization, Physical Address, Email, and Telephone (unless WHOIS Privacy/GDPR redaction is applied).
  * Authoritative Name Servers (NS records) pointing to host providers or CDN proxies (e.g., Cloudflare, Akamai).
* **Command Syntax**:
  ```bash
  whois example.com
  whois 198.51.100.1
  ```

### 2.2 DNS Enumeration & Architecture
Domain Name System (DNS) maps human-readable domain names to numerical IP addresses (UDP/TCP Port 53). Querying DNS provides deep insights into network topology:

#### Standard Resource Records (RR)
1. **A**: IPv4 Host Address mapping (32-bit).
2. **AAAA**: IPv6 Host Address mapping (128-bit).
3. **MX (Mail Exchange)**: Mail servers handling inbound email (with priority integers; lower number = higher priority).
4. **NS (Name Server)**: Authoritative DNS servers for the domain zone.
5. **CNAME (Canonical Name)**: Alias mapping pointing to another domain name.
6. **TXT (Text)**: Human/machine-readable text; commonly stores SPF (`v=spf1 ...`), DKIM, DMARC, and domain ownership verification strings.
7. **SOA (Start of Authority)**: Zone serial numbers, refresh/retry timers, and primary authoritative master server.
8. **PTR (Pointer)**: Reverse DNS mapping (IP address to FQDN).
9. **SRV (Service)**: Defines location (hostname and port) of specific enterprise services (e.g., SIP, LDAP, Active Directory Kerberos).

#### DNS Query Tools
```bash
# Querying Name Servers
dig example.com NS +short
nslookup -type=ns example.com

# Querying Mail Exchangers
dig example.com MX
nslookup -type=mx example.com

# Reverse Lookup
dig -x 192.0.2.1
```

#### DNS Zone Transfer (AXFR) Misconfiguration Attack
* **Mechanism**: In a high-availability master-slave DNS architecture, secondary (slave) name servers synchronize zone records from the primary (master) using an **Asynchronous Full Transfer (AXFR)** query over **TCP port 53**.
* **Vulnerability**: If the administrator fails to restrict AXFR transfers to authorized secondary slave IP addresses (allowing queries from `any`), any external attacker can issue an AXFR request and download the **entire DNS zone database**.
* **Impact**: Total compromise of internal domain topology, revealing hidden intranet portals, staging servers, VPN endpoints, development subdomains, and internal IP addressing schemes.
* **Exploitation Command**:
  ```bash
  dig @ns1.target.com target.com AXFR
  host -l target.com ns1.target.com
  ```
* **Remediation**: In BIND (`named.conf`), configure strict Access Control Lists (ACLs) using `allow-transfer { 192.168.1.5; };` and enforce Transaction Signatures (TSIG) using cryptographic shared keys.

### 2.3 Email Enumeration
* **Objective**: Discover corporate email naming conventions (e.g., `firstname.lastname@company.com`, `flastname@company.com`) and harvesting employee email addresses.
* **Relevance**: Facilitates targeted spear-phishing campaigns and password brute-forcing / credential stuffing dictionaries.
* **OSINT Tools**:
  * **theHarvester**: Gathers emails, subdomains, employee names, open ports, and banners from search engines, PGP key servers, and LinkedIn.
    ```bash
    theHarvester -d target.com -b google,linkedin,bing -l 500
    ```
  * **Hunter.io**: Commercial API indexing web-scraped employee addresses and corporate syntax patterns.
  * **Have I Been Pwned (HIBP)**: Validates if gathered corporate credentials appear in historical third-party data breaches.

### 2.4 Google Dorking (Google Hacking)
Google Dorking utilizes advanced search engine syntax and operators to locate sensitive, unindexed, or accidentally exposed data cached by Google web crawlers.

#### Essential Search Operators
* `site:target.com` — Restricts search results strictly to the specified domain.
* `filetype:pdf` or `ext:sql` — Restricts results to specific file extensions.
* `inurl:admin` — Locates occurrences of the string inside the URL structure.
* `intitle:"index of /"` — Locates directory listing pages where server directory browsing is enabled.
* `cache:target.com` — Displays Google’s cached historical snapshot of a webpage.

#### High-Yield Exam Dorks
```text
# Directory Browsing / Sensitive Backups:
site:target.com intitle:"Index of" "parent directory"
site:target.com filetype:sql "INSERT INTO"
site:target.com ext:env "DB_PASSWORD"

# Exposed Admin Portals:
site:target.com inurl:admin | inurl:login | inurl:portal

# Publicly Indexed Configuration Files:
site:target.com filetype:xml inurl:web.config
site:target.com ext:log inurl:error_log

# Exposed CCTV / Network Hardware:
inurl:"ViewerFrame?Mode="
intitle:"RouterOS router configuration page"
```

#### Defensive Countermeasures
* Deploy properly configured `robots.txt`? **CAUTION: `robots.txt` is an advisory protocol, NOT an access control mechanism**. Malicious attackers routinely inspect `Disallow:` paths in `robots.txt` to find sensitive directories!
* Enforce server-side authentication (HTTP Basic Auth, OAuth2) on internal directories.
* Disable web server directory browsing (`Options -Indexes` in Apache HTTPD).
* Request emergency URL cache removal via Google Search Console.

### 2.5 Specialized OSINT Search Engines
* **Shodan (`shodan.io`)**: Search engine for Internet-connected devices. Scans the entire IPv4 address space, grabbing banners from webcams, industrial control systems (SCADA/ICS), IoT devices, routers, and open database ports (e.g., Elasticsearch port 9200, MongoDB port 27017).
* **Censys (`censys.io`)**: Continuously catalogs Internet assets, SSL/TLS certificates, and exposed host configurations.
* **Maltego**: Visual link analysis software mapping relationships between domains, subnets, email addresses, individuals, and organizational infrastructure into graphical relationship trees.

---

## 3. Active Reconnaissance & Network Scanning

Network scanning involves sending raw packets to discover live hosts, open network ports, and actively running services.

```
       [ Target IP Subnet ]
                │
     ┌──────────┴──────────┐
     ▼                     ▼
[ Host Discovery ]   [ Port Scanning ]
  (Ping Sweeps,       (TCP Connect, SYN,
   ARP, ICMP)          UDP, FIN, Xmas)
                           │
                           ▼
                    [ Port States ]
                   Open | Closed | Filtered
                           │
                           ▼
                 [ Service Detection ]
                  (Banner Grabbing,
                   Nmap -sV, NSE)
```

### 3.1 Host Discovery (Ping Sweeps)
Determines which IP addresses on a subnet have active, responsive network nodes.
* **ARP Ping (`-PR`)**: Used on local Ethernet subnets (Layer 2). Extremely fast and cannot be blocked by host software firewalls because ARP is required for MAC resolution.
* **ICMP Echo Ping (`-PE`)**: Sends standard ICMP Type 8 (Echo Request). Responsive hosts return ICMP Type 0 (Echo Reply). Frequently dropped by modern corporate firewalls.
* **TCP SYN Ping (`-PS<port>`)**: Sends an empty TCP SYN packet to specific ports (default 80, 443). If the host is alive, it responds with SYN/ACK (port open) or RST (port closed). Both prove the host is active even if ICMP is blocked!
* **TCP ACK Ping (`-PA<port>`)**: Sends a bare TCP ACK. Hosts respond with RST if alive, cutting through stateless firewalls.
* **Nmap Flag `-Pn`**: Disables host discovery completely. Tells Nmap to assume all target hosts are online; essential when scanning targets behind strict firewalls that drop all ping probes.

---

## 4. Nmap Port Scanning Mechanics & Scan Types

A port scan systematically probes TCP and UDP ports (from 1 to 65,535) to determine the status of listening network daemons.

### 4.1 The Three Primary Nmap Port States
1. **Open**: An application actively accepts connections on the port. A response packet (e.g., SYN/ACK for TCP) was received.
2. **Closed**: The port is accessible (packets reach the host), but no application daemon is listening. The target host responds with a TCP **RST** packet (or ICMP Port Unreachable for UDP).
3. **Filtered**: Nmap cannot determine if the port is open because packet filtering (firewall, router ACL, or IDS) intercepts and drops the probes before reaching the port, or drops the responses. The probe times out, or returns an ICMP Type 3 Destination Unreachable (Code 1, 2, 3, 9, 10, or 13).

---

### 4.2 Comprehensive Nmap Scan Types

#### 1. TCP Connect Scan (`-sT`)
* **Mechanism**: Completes the full RFC 793 **Three-Way Handshake** using the operating system's native `connect()` system call:
  1. Attacker sends `SYN`.
  2. Target responds with `SYN/ACK` (if Open).
  3. Attacker sends `ACK` (completing connection), followed immediately by `RST` or `FIN` to close.
* **Response Summary**:
  * **Open**: `SYN/ACK` received.
  * **Closed**: `RST` received.
  * **Filtered**: No response (timeout) or ICMP Type 3 error.
* **Pros**: Does not require administrative/root privileges; works reliably on any operating system.
* **Cons**: Noisiest scan type; web and system logs record an established and abruptly terminated connection. Easily detected by simple application log monitors.

```
Attacker                         Target (Port OPEN)
   │                                     │
   │ ────────── TCP SYN ───────────────> │
   │ <───────── TCP SYN/ACK ───────────  │
   │ ────────── TCP ACK ───────────────> │ (Handshake Complete)
   │ ────────── TCP RST ───────────────> │ (Torn Down)
```

#### 2. TCP SYN "Stealth" / Half-Open Scan (`-sS`)
* **Mechanism**: Bypasses full connection establishment:
  1. Attacker sends `SYN`.
  2. Target responds with `SYN/ACK` (if Open).
  3. Attacker immediately transmits a `RST` packet instead of the final `ACK`!
* **Why Stealthy?**: Because the 3-way handshake is never completed, the target application layer never receives an accepted socket connection, preventing application-level server daemons (e.g., Apache, Nginx) from writing an access log entry.
* **Pros**: Extremely fast, stealthy, scans thousands of ports per minute. Default Nmap scan mode when executed as root.
* **Cons**: Requires raw socket creation privileges (`sudo` / root / `admin`). Readily flagged by modern stateful firewalls tracking half-open states.

```
Attacker                         Target (Port OPEN)
   │                                     │
   │ ────────── TCP SYN ───────────────> │
   │ <───────── TCP SYN/ACK ───────────  │
   │ ────────── TCP RST ───────────────> │ (Never Completed!)
```

#### 3. Inverse TCP Scans: Null, FIN, and Xmas Scans
These scan types exploit an architectural loophole in **RFC 793** (TCP specification page 65): *"If the destination port state is CLOSED, an incoming segment not containing a RST causes a RST to be sent in response. If the destination port is OPEN, the segment is discarded without response."*

* **Null Scan (`-sN`)**: Sends a packet with **zero TCP flags** set (`Flags = 0x00`).
* **FIN Scan (`-sF`)**: Sends a packet with only the **FIN flag** set (`Flags = 0x01`).
* **Xmas Tree Scan (`-sX`)**: Sends a packet with **FIN, PSH, and URG** flags set (lighting up the packet like a Christmas tree, `Flags = 0x29`).
* **Behavior Matrix**:
  * **Target Port is CLOSED**: Target OS generates a standard TCP **RST** packet.
  * **Target Port is OPEN**: Target OS drops the malformed packet silently — **NO RESPONSE**.
  * **Target Port is FILTERED**: Probe is blocked by a firewall — **NO RESPONSE** (or ICMP unreachable).
  * *Resulting State*: Nmap reports the state ambiguously as `Open|Filtered`.
* **Advantage**: Penetrates non-stateful (stateless) packet-filtering firewalls configured to block inbound SYN packets.
* **Critical Limitation / Exception**: **Does NOT work against Microsoft Windows, Cisco, BSDI, or IBM OS/400 systems!** These systems violate RFC 793 and send a `RST` response regardless of whether the port is open or closed. Only works reliably on compliant Unix/Linux/BSD systems.

#### 4. UDP Scan (`-sU`)
* **Mechanism**: UDP is connectionless; sends raw empty UDP packet (or protocol-specific payload like DNS query to port 53).
* **Behavior**:
  * If the port is **CLOSED**: The target operating system responds with an **ICMP Type 3, Code 3 (Destination Unreachable: Port Unreachable)** error packet.
  * If the port is **OPEN**: Most UDP services send **NO RESPONSE** (unless the payload matches the expected application protocol, e.g., SNMP or DNS, which returns a response).
  * If no response is received, Nmap reports `Open|Filtered`.
* **Limitation**: Incredibly slow. Operating systems strictly rate-limit ICMP error generation (e.g., Linux kernel limits ICMP Destination Unreachable replies to 1 per second), causing a full 65,535-port UDP scan to take 18+ hours.

#### 5. TCP ACK Scan (`-sA`)
* **Mechanism**: Sends a packet with only the **ACK flag** set.
* **Purpose**: **Does NOT determine whether a port is open!** Used exclusively to map firewall rulesets and determine whether firewall filtering is stateful or stateless.
* **Behavior**:
  * If target responds with **RST**: The port is **Unfiltered** (the ACK packet passed through the firewall and hit the OS TCP stack).
  * If **No Response** (timeout) or ICMP Type 3 Code 13 error: The port is **Filtered** (the firewall blocked the probe).

---

### 4.3 Nmap Command Cheat Sheet for University Exams

| Flag | Category | Technical Operation |
| :--- | :--- | :--- |
| `nmap -sn <target>` | Host Discovery | Ping scan (disables port scanning; discovers live hosts). |
| `nmap -Pn <target>` | Firewall Evasion | Treats all hosts as online; bypasses ICMP ping blocking. |
| `nmap -sS <target>` | Port Scan | TCP SYN Stealth (Half-open) scan (default root). |
| `nmap -sT <target>` | Port Scan | TCP Connect scan (full 3-way handshake; unprivileged). |
| `nmap -sU <target>` | Port Scan | UDP port scan. |
| `nmap -sN / -sF / -sX` | Port Scan | Null / FIN / Xmas tree RFC 793 inverse scan. |
| `nmap -sA <target>` | Firewall Recon | TCP ACK scan (differentiates filtered vs unfiltered). |
| `nmap -sV <target>` | Service Version | Probes open ports to determine application service & version. |
| `nmap -O <target>` | OS Detection | TCP/IP stack fingerprinting to determine exact OS & kernel. |
| `nmap -A <target>` | Aggressive Scan | Combines `-sV`, `-O`, traceroute, and default NSE scripts. |
| `nmap -p <ports>` | Port Range | e.g., `-p 80,443`, `-p 1-1024`, or `-p-` (all 65,535 ports). |
| `nmap -T<0-5>` | Timing Template | `T0` (Paranoid), `T1` (Sneaky), `T2` (Polite), `T3` (Normal), `T4` (Aggressive), `T5` (Insane). |
| `nmap --script vuln` | Vulnerability Scan| Executes Nmap Scripting Engine (NSE) vulnerability detection category. |

---

## 5. Banner Grabbing & Service Enumeration

Once open ports are mapped, service enumeration extracts granular metadata regarding running daemons to pinpoint known vulnerabilities.

### 5.1 Banner Grabbing
Banner grabbing queries a service’s welcome greeting message displayed upon connection:
* **Information Disclosed**: Application software name, exact version number, patch revision, and underlying OS platform (e.g., `Apache/2.4.41 (Ubuntu) Server at 192.168.1.50 Port 80`).
* **Active Banner Grabbing**:
  Directly initiating a network socket connection:
  ```bash
  # Web Server Banner via Curl
  curl -I http://192.168.1.50

  # FTP Banner via Netcat
  nc -v 192.168.1.50 21
  # Response: 220 ProFTPD 1.3.5 Server (Ubuntu) [192.168.1.50]

  # SMTP Banner via Telnet
  telnet 192.168.1.50 25
  # Response: 220 mail.corporate.com ESMTP Postfix (Debian)
  ```
* **Passive Banner Grabbing**:
  Inspecting captured packet pcaps in Wireshark without transmitting packets. Analyzes client-server negotiation fields, TCP Window Size, IP TTL, and Server HTTP headers.
* **Defensive Hardening**:
  * In Apache: Configure `ServerTokens Prod` and `ServerSignature Off` (reduces banner strictly to `Apache`).
  * In Nginx: Configure `server_tokens off;`.
  * Replace default software banners with generic or misleading strings.

---

### 5.2 Deep Protocol Enumeration

#### 1. Server Message Block (SMB - TCP Ports 139 / 445)
* Windows and Samba file/print sharing. High-priority target in internal penetration tests.
* **Information Leaked**: Active usernames, local group memberships, network share names (`C$`, `ADMIN$`, `IPC$`), domain name, OS build version, null session support.
* **Tools**:
  ```bash
  enum4linux -a 192.168.1.50
  smbclient -L //192.168.1.50 -N   # List shares with NULL password
  nmap --script smb-enum-shares,smb-enum-users -p 445 192.168.1.50
  ```

#### 2. Simple Network Management Protocol (SNMP - UDP Port 161)
* Used by network engineers to monitor network devices, switches, routers, and servers.
* **Vulnerability**: Legacy SNMP v1 and v2c transmit authentication credentials as plaintext **Community Strings** (commonly left as default: `public` for Read-Only, `private` for Read-Write).
* **Information Leaked**: Full process list, open TCP/UDP sockets, connected network interfaces, routing tables, installed software, and domain usernames via the Management Information Base (MIB).
* **Tools**:
  ```bash
  snmpwalk -v2c -c public 192.168.1.50 1.3.6.1.2.1.25.4.2.1.2 # Process listing
  onesixtyone -c community.txt 192.168.1.50
  ```

#### 3. Simple Mail Transfer Protocol (SMTP - TCP Port 25)
* SMTP servers support built-in enumeration commands:
  * `VRFY <username>`: Asks the server to verify if a mailbox exists.
  * `EXPN <mailing-list>`: Expands and reveals members of an email distribution list.
  * `RCPT TO:<username>`: Tests if the server accepts mail for an address.
* Testers automate username harvesting scripts against dictionaries:
  ```text
  VRFY admin  ──> 250 2.1.5 Administrator <admin@company.com> (Valid User!)
  VRFY fake   ──> 550 5.1.1 User unknown (Invalid)
  ```

---

## 6. Vulnerability Scanning Concepts & Tooling

A Vulnerability Assessment (VA) is an automated inspection comparing discovered network services, ports, and banners against centralized vulnerability databases (such as the National Vulnerability Database - NVD, tracking Common Vulnerabilities and Exposures - CVEs).

### 6.1 Vulnerability Scanning Lifecycle
```
[ Scope Definition ] ──> [ Host Discovery ] ──> [ Port & Service Detection ]
                                                          │
[ Remediation & Patching ] <── [ Reporting & CVSS ] <── [ CVE Database Matching ]
```

### 6.2 Credentialed (Authenticated) vs Non-Credentialed (Unauthenticated) Scans

| Feature | Non-Credentialed (External) Scan | Credentialed (Internal) Scan |
| :--- | :--- | :--- |
| **Perspective** | External perimeter attacker with no login rights. | Malicious insider or compromised domain workstation. |
| **Method** | Probes open ports, inspects banners, checks network services. | Logs into host OS via SSH or SMB using admin credentials. |
| **Visibility** | Limited to externally listening services and banners. | Deep internal visibility: unpatched DLLs, registry keys, missing OS patches, local misconfigurations. |
| **False Positives** | **High** (cannot verify if an exposed banner is backported). | **Extremely Low** (checks exact file versions and hashes). |
| **System Impact** | Low CPU overhead; generates firewall alerts. | Minimal network traffic; higher local CPU during audit. |

### 6.3 Industry Standard Vulnerability Scanning Tools
* **Tenable Nessus**: Comprehensive commercial enterprise scanner featuring vast plugin libraries covering network devices, virtualization, operating systems, and compliance benchmarks.
* **OpenVAS (Greenbone Vulnerability Management)**: Leading open-source vulnerability scanner utilizing daily updated Network Vulnerability Tests (NVTs).
* **Qualys VMDR**: Cloud-native vulnerability management, detection, and response solution.
* **Nikto**: Specialized open-source command-line web server scanner identifying outdated server software, dangerous default files, CGIs, and misconfigured HTTP headers.
  ```bash
  nikto -h http://192.168.1.50
  ```

---

## 7. High-Yield University Exam Comparison Tables

### Table 1: Passive vs Active Reconnaissance

| Dimension | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Direct Contact** | None. Interacts only with third-party databases. | Yes. Transmits raw IP packets directly to target. |
| **Detectability** | Impossible for target IDS/firewall to detect. | Easily detected and logged by firewalls, NIDS, and SIEM. |
| **Tools Used** | WHOIS, DNS queries, Google Dorking, Shodan, Maltego. | Nmap, Ping sweeps, Port scanners, Nessus, Banner grabs. |
| **Information Type** | Domain ownership, IP ranges, public records, staff emails. | Open ports, running daemon versions, OS stack, live hosts. |
| **Legal Exposure** | Low (accessing publicly available public data). | Moderate to High (requires explicit written client scope). |

---

### Table 2: Nmap Port Scanning Techniques Comparison

| Scan Type | Command Flag | Packet Sent | Response (Open) | Response (Closed) | Privileges | Firewall Detection |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TCP Connect** | `-sT` | `SYN` | `SYN/ACK` (then `ACK`, `RST`) | `RST` | User | High (logged in app logs) |
| **TCP SYN** | `-sS` | `SYN` | `SYN/ACK` (then `RST`) | `RST` | Root / Admin | Moderate (no app log) |
| **TCP Null** | `-sN` | None (`0x00`) | No response | `RST` | Root / Admin | Low (bypasses stateless) |
| **TCP FIN** | `-sF` | `FIN` | No response | `RST` | Root / Admin | Low (bypasses stateless) |
| **TCP Xmas** | `-sX` | `FIN, PSH, URG`| No response | `RST` | Root / Admin | Low (bypasses stateless) |
| **UDP Scan** | `-sU` | UDP packet | No response / UDP reply | ICMP Port Unreachable | Root / Admin | Low (rate-limited by OS) |
| **TCP ACK** | `-sA` | `ACK` | `RST` (Unfiltered) | `RST` (Unfiltered) | Root / Admin | Maps firewall rulesets |

---

### Table 3: Port States (Open vs Closed vs Filtered)

| Port State | Meaning | Network Behavior & Response |
| :--- | :--- | :--- |
| **Open** | Service daemon is listening and accepting connections. | Responds with `SYN/ACK` (TCP) or application payload. |
| **Closed** | Host received packet, but no daemon is listening. | Responds with `RST` (TCP) or ICMP Port Unreachable (UDP). |
| **Filtered** | A firewall, filter, or router is blocking probe delivery. | Probe times out with **No Response**, or returns ICMP Destination Unreachable Type 3 (Code 13: Communication Administratively Prohibited). |
