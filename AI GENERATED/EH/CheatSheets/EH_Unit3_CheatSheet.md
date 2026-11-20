# EH Unit 3: System Hacking and Network Security
**Course**: Ethical Hacking (MD***) | **VIT Pune** | **Exam Date**: 27 Nov 2026

---

### 1. Password Attacks & Cracking Taxonomy
* **Online Attacks**: Live network interface probing.
  * *Dictionary*: Wordlist against 1 user. *Brute Force*: All character permutations.
  * *Password Spraying*: 1 common password tried across thousands of accounts (evades account lockouts!).
  * *Credential Stuffing*: Automated replay of leaked breach database credential pairs.
* **Offline Attacks**: Performed on stolen password databases (`/etc/shadow`, SAM, `ntds.dit`). Zero lockout risk, no network latency, GPU acceleration.
* **Salting Defense**: Prepending a unique, cryptographically random salt to the password before hashing:
  $$\text{Stored Digest} = \text{Hash}(\text{Password} \,\|\, \text{Salt})$$
  * *Impact*: Completely destroys **Rainbow Tables**; two users with identical passwords yield completely different hashes.
* **Slow Hashing Algorithms**: Argon2id, bcrypt, PBKDF2 (key stretching with configurable work factors).

---

### 2. Password Cracking Tools: John the Ripper vs Hashcat
* **John the Ripper (JtR)**: CPU-based multi-platform cracker. Modes: `single` (fastest, uses username), `wordlist` (`--wordlist=rockyou.txt`), `incremental` (brute force).
  * Syntax: `unshadow /etc/passwd /etc/shadow > unshadowed.txt; john --wordlist=rockyou.txt unshadowed.txt`.
* **Hashcat**: GPU-accelerated hash cracker. Modes (`-a`): `0` (Dictionary), `1` (Combination), `3` (Mask), `6` (Hybrid).
  * Common Hashes (`-m`): `0` (MD5), `1000` (NTLM), `1800` (SHA-512 $6$), `3200` (bcrypt).

---

### 3. Privilege Escalation (Vertical vs Horizontal)
* **Vertical Privilege Escalation**: Unprivileged user jumps to `root` or `Administrator`.
  * *Linux Vectors*: Unpatched kernel bugs (Dirty COW), SUID binaries with shell escapes (`find`, `vim`), insecure sudo rules (`NOPASSWD`).
  * *Windows Vectors*: Unquoted service paths, DLL hijacking, Token Impersonation.
* **Horizontal Privilege Escalation**: Attacker accesses data/functions of another user at the same privilege level.
  * *Root Cause*: Insecure Direct Object References (IDOR / BOLA) — changing URL ID parameters without authorization checks.

---

### 4. Malware Taxonomy Cheat Sheet
| Malware | Defining Mechanism | Spreading Method | Threat / Impact |
| :--- | :--- | :--- | :--- |
| **Virus** | Parasitic; attaches to host executables. | Human execution required. | Corrupts data, alters binaries. |
| **Worm** | Standalone; autonomous network propagator. | Self-propagates via network flaws. | Bandwidth exhaustion, rapid infection. |
| **Trojan Horse**| Disguised as legitimate, useful software. | Social engineering trickery. | Backdoor access (RAT), drops malware. |
| **Ransomware** | Strong hybrid encryption (AES+RSA). | Phishing, exploits, RDP brute force. | Demands ransom for private key. |
| **Rootkit** | Subverts OS kernel; intercepts system calls. | Kernel drivers, root access. | Hides processes, files, and sockets. |
| **Fileless** | Executes purely in memory (RAM). | LOLBins (PowerShell, WMI). | Bypasses traditional disk antivirus. |

---

### 5. ARP Spoofing & Man-in-the-Middle (MITM)
* **Vulnerability**: ARP (RFC 826) has **zero authentication**. Systems accept unsolicited (gratuitous) ARP replies without verification.
* **Attack**: Attacker sends forged ARP replies to both Victim and Gateway claiming to own the other's IP address. Traffic flows through attacker's MAC.
* **Enabling MITM Forwarding**: `sysctl -w net.ipv4.ip_forward=1`.
* **Defenses**:
  * **Dynamic ARP Inspection (DAI)** on managed switches (validates ARP against DHCP Snooping database).
  * **Port Security**: Enforces sticky MAC limits per physical switch port.
  * **Static ARP**: `arp -s <gateway-ip> <gateway-mac>`.
  * **TLS 1.3 / SSH**: End-to-end encryption renders sniffed payload unreadable.

---

### 6. Session Hijacking & Cookie Defenses
* **Vectors**: Sniffing unencrypted HTTP, Cross-Site Scripting (`document.cookie`), predictable session tokens.
* **Defensive Cookie Flags**:
  * `HttpOnly`: Blocks client-side JavaScript access to cookies (mitigates XSS cookie theft).
  * `Secure`: Transmits cookie strictly over encrypted HTTPS connections.
  * `SameSite=Strict`: Restricts cookie transmission in cross-origin requests (mitigates CSRF).
  * *Session Regeneration*: Issue a new token immediately upon login.

---

### 7. Firewalls: Evolution & DMZ
* **Packet Filtering (L3/4)**: Stateless; filters on IP/port headers. Fast, blind to payloads.
* **Stateful Inspection (L4/5)**: Tracks TCP state table (`NEW`, `ESTABLISHED`). Permits return traffic dynamically.
* **Application Proxy (L7)**: Terminates and inspects entire application session. High latency.
* **Next-Gen Firewall (NGFW)**: Deep Packet Inspection (DPI) + inline IPS + Application Awareness.
* **DMZ Architecture**: Isolated subnet for public servers (Web, Mail, DNS); blocked from accessing internal corporate LAN.

---

### 8. IDS vs IPS Paradigms
| Feature | Intrusion Detection System (IDS) | Intrusion Prevention System (IPS) |
| :--- | :--- | :--- |
| **Deployment** | Out-of-band (SPAN / TAP mirror port). | In-line (physical packet pathway). |
| **Primary Action** | Detects, logs, and triggers SIEM alerts. | Detects, **drops malicious packets**, sends TCP RST. |
| **Network Impact**| Zero throughput latency or packet delay. | Slight processing latency; risk of false-positive drop. |
| **Detection Mode**| **Signature-Based** (Known threats, low FP) vs **Anomaly-Based** (Zero-days, higher FP). |
