# EH Unit 2: Footprinting, Reconnaissance and Scanning
**Course**: Ethical Hacking (MD***) | **VIT Pune** | **Exam Date**: 27 Nov 2026

---

### 1. Passive vs Active Reconnaissance
* **Passive Recon**: Indirect information gathering with zero packets sent to target. Completely stealthy; unrecorded by firewalls/IDS. Sources: WHOIS, DNS, Google Dorking, Shodan.
* **Active Recon**: Direct packet interaction with target hosts, firewalls, and ports. Highly visible; logged by firewalls and SIEM. Tools: Nmap, Ping sweeps, Nessus, Banner grabs.

---

### 2. OSINT & Reconnaissance Tooling
* **WHOIS (TCP 43)**: Domain registrar, creation/expiry dates, registrant details, authoritative name servers.
* **DNS Records**: `A` (IPv4), `AAAA` (IPv6), `MX` (Mail), `NS` (Name Server), `CNAME` (Alias), `TXT` (SPF/DMARC), `SOA` (Zone authority), `PTR` (Reverse IP).
* **DNS Zone Transfer (AXFR, TCP 53)**: Primary-to-secondary DNS replication query. If misconfigured (`allow-transfer { any; };`), leaks entire domain topology, subdomains, and private IPs. Tool: `dig @ns1.target.com target.com AXFR`.
* **theHarvester**: Gathers emails, subdomains, employee names, and open ports via search engines and PGP servers.
* **Shodan (`shodan.io`)**: IoT and device search engine indexing banners, open ports, SCADA, webcams, and unauthenticated databases across IPv4.

---

### 3. Google Dorking (Key Operators)
* `site:target.com` — Scope strictly to target domain.
* `filetype:pdf / ext:sql` — Restricts search to specified file extension.
* `inurl:admin` — Locates string within the URL path.
* `intitle:"index of /"` — Finds open server directory listings.
* **Exam Dork**: `site:target.com ext:env "DB_PASSWORD"` or `filetype:sql "INSERT INTO"`.
* **Trap**: `robots.txt` is an advisory directive for search crawlers, NOT security! Attackers read `Disallow:` lines to find hidden directories.

---

### 4. Nmap Port Scanning Mechanics
| Scan Type | Flag | Packets Exchanged | Open Response | Closed Response | Attributes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TCP Connect** | `-sT` | Full 3-way Handshake | `SYN/ACK` $\rightarrow$ `ACK` $\rightarrow$ `RST` | `RST` | User rights; recorded in app access logs |
| **TCP SYN Stealth**| `-sS` | Half-Open Connection | `SYN/ACK` $\rightarrow$ `RST` | `RST` | Root required; stealthy, fast (default root) |
| **Inverse (Null/FIN/Xmas)**| `-sN/-sF/-sX`| RFC 793 Flaw Probes | **No Response** (Dropped) | `RST` | Bypasses stateless firewalls; FAILS on Windows |
| **UDP Scan** | `-sU` | Empty UDP Probe | No Response / UDP Reply | ICMP Port Unreachable | Extremely slow due to OS ICMP rate limiting |
| **TCP ACK Scan** | `-sA` | TCP `ACK` Probe | `RST` (Unfiltered) | `RST` (Unfiltered) | Maps firewall rulesets; does NOT detect open ports |

---

### 5. Nmap Three Primary Port States
1. **Open**: Service daemon actively listening and accepting connections (`SYN/ACK` received).
2. **Closed**: Host is online, but no daemon is listening on that port (`RST` received).
3. **Filtered**: Firewall, filter, or ACL intercepts packet. Probe times out with **No Response** or returns ICMP Type 3 Destination Unreachable (Code 13: Administratively Prohibited).

---

### 6. Banner Grabbing & Service Enumeration
* **Banner Grabbing**: Connecting to a port to read the initial welcome greeting revealing software name, version, and OS.
  * Active: `curl -I http://ip`, `nc -v ip 21`, `telnet ip 25`, `nmap -sV ip`.
  * Passive: Sniffing Wireshark pcaps without sending packets.
  * Defense: `ServerTokens Prod` (Apache), `server_tokens off;` (Nginx).
* **SMB Enumeration (Port 139/445)**: Extracts usernames, shares (`C$`, `ADMIN$`), and domain info via `enum4linux -a <IP>`.
* **SNMP Enumeration (Port 161 UDP)**: Queries MIB using default community strings (`public`, `private`) via `snmpwalk`.
* **SMTP Enumeration (Port 25)**: User harvesting via `VRFY <user>` and `EXPN <list>`.

---

### 7. Vulnerability Assessment (VA) & Scanners
* **Definition**: Automated scanning to identify known CVEs without active exploitation.
* **Tools**: Nessus, OpenVAS (Greenbone), Qualys VMDR, Nikto (Web server misconfigurations).
* **Credentialed Scan**: Uses SSH/SMB admin credentials; inspects installed packages, DLL hashes, and registry keys. Near-zero false positives.
* **Non-Credentialed Scan**: Probes from outside without credentials; relies on network banners. Higher false positives (misses backported patches).

---

### 8. Essential Exam Traps
* **Trap 1**: `nmap -sA` does NOT find open ports! It only tests if ports are filtered by a firewall.
* **Trap 2**: Null, FIN, and Xmas scans do NOT work on Microsoft Windows (Windows replies `RST` to all abnormal probes).
* **Trap 3**: `robots.txt` never secures a directory; it publicly announces sensitive directory paths to attackers!
* **Trap 4**: DNS AXFR uses TCP port 53 (bulk data transfer), while normal DNS lookups use UDP port 53.
