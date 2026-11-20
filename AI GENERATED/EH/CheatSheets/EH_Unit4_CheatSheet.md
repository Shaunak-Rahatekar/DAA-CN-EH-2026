# EH Unit 4: Web Application and Wireless Security
**Course**: Ethical Hacking (MD***) | **VIT Pune** | **Exam Date**: 27 Nov 2026

---

### 1. HTTP vs HTTPS & TLS
* **HTTP (Port 80)**: Stateless, unencrypted plaintext; vulnerable to sniffing & MITM.
* **HTTPS (Port 443)**: HTTP over TLS 1.2/1.3; provides Confidentiality, Integrity, and Server Authentication (X.509 PKI certificates).
* **TLS Handshake**: ClientHello $\rightarrow$ ServerHello + Cert $\rightarrow$ ECDHE Key Exchange (PFS) $\rightarrow$ Symmetric AES-GCM encryption.

---

### 2. OWASP Top 10 High-Yield Summary
* **A01: Broken Access Control**: Unauthorized data access (IDOR / SSRF). *Fix*: Enforce server-side checks; PoLP.
* **A02: Security Misconfiguration**: Default passwords, exposed debug logs, open S3 buckets. *Fix*: Hardened baseline.
* **A03: Injection (SQLi, Command)**: Untrusted input concatenated into interpreters. *Fix*: Parameterized queries.
* **A04: Cryptographic Failures**: Weak algorithms (MD5, DES), plaintext transit. *Fix*: TLS 1.3, AES-256.
* **A05: Insecure Design**: Flaws in architecture/threat modeling. *Fix*: Secure design lifecycle (SDL).
* **A06: Outdated Components**: Libraries with known CVEs (Log4j). *Fix*: Software Composition Analysis (SCA).
* **A07: Identification & Auth Failures**: Missing MFA, brute-force exposure. *Fix*: Mandatory MFA, rate-limiting.

---

### 3. SQL Injection (SQLi) Taxonomy & Defense
* **In-Band SQLi**:
  * *Tautology*: `' OR 1=1 --` (Bypasses authentication).
  * *UNION-Based*: `' UNION SELECT 1, username, password FROM users--` (Appends attacker query results).
* **Inferential (Blind) SQLi**:
  * *Boolean-Based*: Injects True/False conditions; observes page content changes.
  * *Time-Based*: Injects sleep functions (e.g., `SLEEP(5)`, `WAITFOR DELAY '0:0:5'`); observes latency.
* **Structural Defense**: **Parameterized Prepared Statements** (separates SQL grammar from literal scalar data).

---

### 4. Cross-Site Scripting (XSS) Typology
* **Reflected XSS (Type I)**: Non-persistent; payload in URL query string reflected in server response.
* **Stored XSS (Type II)**: Persistent; payload saved in database (comments/forums) and served to all future visitors.
* **DOM-Based XSS (Type 0)**: Pure client-side; source (`location.hash`) feeds unsafe sink (`innerHTML`).
* **Defenses**: Contextual Output Encoding, Content Security Policy (CSP), `HttpOnly` cookie flag.

---

### 5. Cross-Site Request Forgery (CSRF) & Defenses
* **Mechanism**: Authenticated victim visits malicious site (`evil.com`), which auto-submits a forged POST request to `bank.com` using the browser's automatically attached session cookie.
* **Defenses**:
  * **Anti-CSRF Synchronizer Tokens**: Random, unpredictable token in forms verified server-side.
  * **`SameSite=Strict` Cookie Flag**: Blocks cookie sending in cross-site requests.
  * **Re-Authentication**: Require password/MFA for sensitive fund transfers.

---

### 6. Directory Traversal, File Inclusion & Headers
* **Directory Traversal**: Using `../../etc/passwd` to escape web root. *Fix*: `realpath()`, allowlisting.
* **LFI vs RFI**: Local File Inclusion executes local server files; Remote File Inclusion includes remote scripts (`http://evil.com/shell.txt`). *Fix*: Disable `allow_url_include` in PHP.
* **Security Headers**:
  * `Content-Security-Policy`: Restricts authorized script sources (mitigates XSS).
  * `Strict-Transport-Security` (HSTS): Enforces HTTPS connections.
  * `X-Frame-Options: DENY`: Prevents Clickjacking in `<iframe>`.
  * `X-Content-Type-Options: nosniff`: Disables MIME sniffing.

---

### 7. Wireless Security Architecture & Protocols
| Protocol | Cipher | Integrity Check | Security Status / Vulnerability |
| :--- | :--- | :--- | :--- |
| **WEP** | RC4 (Stream) | CRC-32 (Broken) | 24-bit weak IV; **Completely broken in 60 sec**. |
| **WPA** | RC4 + TKIP | Michael MIC (64-bit) | Deprecated interim patch; vulnerable to packet forging. |
| **WPA2** | AES-CCMP | CBC-MAC | 4-Way Handshake vulnerable to offline dictionary/KRACK. |
| **WPA3** | AES-GCM | BIP-GMAC / PMF | **SAE (Dragonfly)**; forward secrecy; resists offline dict. |

---

### 8. Wireless Attack Vectors & Countermeasures
* **Evil Twin**: Rogue AP broadcasting legitimate SSID with higher transmission power to harvest credentials.
* **Deauthentication Attack**: Sending spoofed 802.11 deauth frames (`aireplay-ng -0`) to disconnect clients and capture the 4-way handshake for offline dictionary cracking.
* **WPA3 SAE Defense**: Replaces PSK with Dragonfly handshake (no password on air) and mandates **Protected Management Frames (PMF: 802.11w)**, completely blocking deauth attacks!
* **Enterprise Hardening**: WPA2/WPA3-Enterprise with 802.1X/RADIUS and EAP-TLS; Wireless IPS (WIPS).
