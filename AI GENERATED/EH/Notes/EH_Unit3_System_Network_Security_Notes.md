# Unit III: System Hacking and Network Security
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Faculty / Curriculum**: AY 2026-27 | **Exam Date**: 27 Nov 2026

---

## 1. Password Attacks and Cracking Techniques

Authentication is the primary line of defense in access control. Password attacks seek to compromise authentication credentials through mathematical, algorithmic, social, or software-based vectors.

```
+---------------------------------------------------------------------------------------+
|                              PASSWORD ATTACK TAXONOMY                                 |
+---------------------------+---------------------------+-------------------------------+
|   NON-ELECTRONIC ATTACKS  |      ONLINE ATTACKS       |        OFFLINE ATTACKS        |
+---------------------------+---------------------------+-------------------------------+
| * Shoulder Surfing        | * Brute Force (High risk) | * Offline Hash Cracking       |
| * Dumpster Diving         | * Dictionary Attacks      | * Rainbow Table Lookups       |
| * Social Engineering      | * Password Spraying       | * Mask / Rule-Based Attacks   |
| * Pretexting              | * Credential Stuffing     | * Tools: Hashcat, John (JtR)  |
+---------------------------+---------------------------+-------------------------------+
```

### 1.1 Classification of Password Attacks

#### 1. Non-Electronic (Physical & Social) Attacks
* **Shoulder Surfing**: Directly observing a victim entering credentials (via direct line-of-sight, smartphone cameras, or binoculars).
* **Dumpster Diving**: Sifting through physical trash bins to retrieve discarded sticky notes, onboarding sheets, network diagrams, and hardware printouts.
* **Social Engineering / Pretexting**: Impersonating IT support or executive leadership to convince users to divulge credentials over telephone or email.

#### 2. Online Password Attacks
The attacker directly interacts with a live authentication interface (SSH, RDP, HTTP form, SMB):
* **Online Brute Force**: Systematically attempting every possible combination of characters ($A-Z, a-z, 0-9, \text{symbols}$). High volume of failed attempts rapidly triggers account lockout policies.
* **Dictionary Attack**: Feeding a predefined wordlist of common passwords (e.g., `rockyou.txt`) against a single target user account.
* **Password Spraying**: Trying a **single common password** (e.g., `Autumn2026!`, `Company@123`) across thousands of employee usernames.
  * *Strategic Value*: Bypasses account lockout thresholds (e.g., 3 failed attempts per user) because each account only receives one login attempt per cycle.
* **Credential Stuffing**: Automated injection of breached username/password pairs harvested from past public corporate data breaches against unrelated services, exploiting widespread user password reuse.

#### 3. Offline Password Attacks
The attacker obtains a copy of the password storage file (e.g., `/etc/shadow` on Linux, SAM database or `ntds.dit` on Windows Active Directory).
* **Advantage**: Zero risk of account lockout; no network latency; probes cannot be detected or blocked by the target authentication server.
* **Hash Cracking**: Generating candidate plaintext passwords, computing their cryptographic hash, and comparing the result to the stolen hash.

---

### 1.2 Cryptographic Password Storage: Hashing, Salting, and Peppering

```
Plaintext Password: "P@ssword123"
        │
        ▼
   [ Append Salt ] ──> "P@ssword123" + "$6$qZ7yX..." (Unique per user)
        │
        ▼
  [ Slow Hashing ] ──> Argon2id / bcrypt / PBKDF2 (Thousands of iterations)
        │
        ▼
   Stored Hash: "$6$qZ7yX...$m9A5dKf8... " (Saved in /etc/shadow or DB)
```

1. **Cryptographic Hashing**: A one-way mathematical function mapping arbitrary-length input strings to a fixed-size digest ($H(M)$). Must possess:
   * **Pre-image Resistance**: Computationally infeasible to reverse $H(M)$ to find $M$.
   * **Collision Resistance**: Infeasible to find any two distinct inputs $M_1 \neq M_2$ such that $H(M_1) = H(M_2)$.
2. **The Danger of Fast Hashes**:
   * Legacy algorithms like **MD5, SHA-1, and NTLM** were designed for file checksum verification, computing billions of hashes per second.
   * Modern GPUs can calculate over **100 billion NTLM hashes per second**, making unsalted brute-force trivial.
3. **Salting**:
   * A **Salt** is a cryptographically random string (typically 16-32 bytes) generated uniquely for each user upon account creation and prepended/appended to the plaintext password before hashing:
     $$\text{Stored Digest} = \text{Hash}(\text{Password} \,\|\, \text{Salt})$$
   * *Defense Mechanism*: **Neutralizes Rainbow Tables** and precomputed lookup dictionaries. Even if two users share the identical password `Password123`, their resulting stored hashes will be completely different due to their unique salts.
4. **Key Stretching (Slow Hashing Algorithms)**:
   * Modern password hashing incorporates deliberate CPU and memory-intensive work factors (cost parameters):
     * **bcrypt**: Based on Blowfish cipher; incorporates configurable iteration cost ($2^{\text{cost}}$).
     * **Argon2 (Argon2id)**: Winner of the Password Hashing Competition (PHC); provides configurable time cost, memory cost, and degree of parallelism to resist both GPU and ASIC hardware attacks.
     * **PBKDF2**: Applies a pseudorandom function (HMAC-SHA256) across thousands of iterations (minimum 600,000 recommended by OWASP).

---

### 1.3 Offline Cracking Tools: John the Ripper & Hashcat

#### 1. John the Ripper (JtR)
Open-source, highly versatile offline password auditing tool:
* **Modes of Operation**:
  * **Single Crack Mode**: Leverages user information (username, home directory name) with built-in mangling rules. Fastest mode.
  * **Wordlist Mode**: Reads passwords from an external dictionary file (e.g., `rockyou.txt`) and applies mangling rules (capitalization, leet-speak substitutions).
  * **Incremental Mode**: Brute-force mode attempting all possible character combinations.
* **Command Syntax**:
  ```bash
  # Unshadow Linux passwd and shadow files
  unshadow /etc/passwd /etc/shadow > unshadowed.txt

  # Crack using RockYou dictionary
  john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt

  # View cracked passwords
  john --show unshadowed.txt
  ```

#### 2. Hashcat
The world's fastest, GPU-accelerated rule-based password recovery engine utilizing OpenCL/CUDA:
* **Attack Modes (`-a`)**:
  * `-a 0`: Straight / Dictionary Attack.
  * `-a 1`: Combination Attack (combines two wordlists).
  * `-a 3`: Mask Attack (brute force with pattern templates, e.g., `?u?l?l?l?d?d?d?s`).
  * `-a 6`: Hybrid Wordlist + Mask.
* **Hash Types (`-m`)**:
  * `-m 0`: MD5 | `-m 1000`: NTLM | `-m 1800`: SHA-512 ($6$) Linux Shadow | `-m 3200`: bcrypt.
* **Command Syntax**:
  ```bash
  # Crack NTLM hashes using GPU with RockYou and Best64 rules
  hashcat -m 1000 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt -r rules/best64.rule
  ```

---

## 2. Authentication Attacks and Privilege Escalation

### 2.1 Authentication Flaws & Modern Attacks
* **Default Credentials**: Failure of administrators to alter factory shipping credentials (e.g., `admin:admin`, `cisco:cisco`, `root:toor`) on routers, IoT cameras, and web consoles.
* **MFA Fatigue / Push Bombing**: Flooding an employee’s mobile authenticator app with dozens of high-frequency push notifications in the middle of the night until the victim accidentally or frustratingly taps "Approve".
* **Session Replay Attacks**: Intercepting valid authentication tokens or Kerberos tickets transmitted over unencrypted channels and replaying them to gain unauthorized access.

---

### 2.2 Privilege Escalation Concepts

Privilege escalation occurs when an attacker exploits a system flaw, architectural bug, or misconfiguration to gain elevated access to resources normally protected from that user account.

```
+---------------------------------------------------------------------------------------+
|                             PRIVILEGE ESCALATION VECTORS                              |
+-------------------------------------------+-------------------------------------------+
|      VERTICAL PRIVILEGE ESCALATION        |      HORIZONTAL PRIVILEGE ESCALATION      |
|           (Privilege Level Jump)          |           (Lateral Identity Shift)        |
+-------------------------------------------+-------------------------------------------+
| * Standard User ──► Administrator / root  | * User A (Account #101) ──► User B (#102) |
| * Exploits: Unpatched OS kernels, SUID,   | * Exploits: Broken Object Level Auth      |
|   misconfigured sudo rules, DLL hijacks.  |   (BOLA / IDOR), weak session logic.      |
+-------------------------------------------+-------------------------------------------+
```

#### 1. Vertical Privilege Escalation Vectors (Linux & Windows)
* **Kernel Exploits**: Exploiting unpatched vulnerabilities in the operating system kernel (e.g., Dirty COW - CVE-2016-5195, PwnKit - CVE-2021-4034) to execute arbitrary shell code with ring-0 kernel/root privileges.
* **Misconfigured SUID / SGID Binaries (Linux)**:
  * SetUID (SUID) allows an executable to run with the permissions of the file owner (typically root).
  * If a binary with SUID permissions can execute shell escapes (e.g., `find`, `vim`, `nmap`, `bash`), a local user can spawn a root shell:
    ```bash
    # Finding SUID binaries
    find / -perm -u=s -type f 2>/dev/null
    ```
* **Insecure Sudo Configurations**: Excessive permissions in `/etc/sudoers` allowing unprivileged accounts to run commands as root without a password (`sudo ALL=(ALL) NOPASSWD: ALL`).
* **Unquoted Service Paths & DLL Hijacking (Windows)**:
  * When a Windows service executable path contains spaces and is not enclosed in quotes (e.g., `C:\Program Files\Custom App\service.exe`), Windows searches for `C:\Program.exe` first. An attacker drops a malicious binary at `C:\Program.exe` which executes with `SYSTEM` rights upon system reboot.

#### 2. Horizontal Privilege Escalation
* The adversary assumes the identity or data of a peer account sharing identical security privileges (e.g., standard customer).
* **Root Cause**: Insecure Direct Object References (IDOR). An application uses user-supplied input to access database records directly without validating authorization:
  * *Request*: `GET /api/user/statement?account_id=10052`
  * *Exploitation*: Attacker changes `account_id` to `10053` and views another customer's financial records.

---

## 3. Malware Taxonomy

Malware (Malicious Software) comprises any software intentionally designed to cause disruption, unauthorized access, data theft, or damage to computer assets.

| Malware Type | Defining Characteristic | Propagation Mechanism | Primary Objective / Threat |
| :--- | :--- | :--- | :--- |
| **Virus** | Parasitic code; attaches itself to legitimate host files (`.exe`, `.dll`, macros). | **Requires human action** (user executes infected file, opens macro). | File corruption, system disruption, backdoor insertion. |
| **Worm** | Standalone, self-contained executable program. | **Self-propagating across networks** automatically (exploits unpatched network daemons). | Network bandwidth exhaustion, massive lateral worm propagation (e.g., Conficker, WannaCry via SMB). |
| **Trojan Horse** | Masquerades as legitimate, useful software (game, utility). | Social engineering (tricks user into downloading and running). | Establishes remote access (RAT), drops keyloggers, steals credentials (e.g., Zeus banking trojan). |
| **Ransomware** | Encrypts victim's files using strong hybrid cryptography (AES + RSA). | Phishing, drive-by web downloads, RDP brute force, worm propagation. | Financial extortion; demands cryptocurrency for private decryption key. |
| **Rootkit** | Subverts operating system integrity; intercepts OS system calls. | Exploits or malicious drivers installed with administrative access. | **Hides presence** of processes, network sockets, and files from Task Manager and AV. |
| **Keylogger** | Captures keystroke sequences. | Software hook (APIs like `GetAsyncKeyState`) or hardware dongle. | Harvests credentials, credit card details, confidential chats. |
| **Spyware** | Secretly tracks user behavior and browser habits. | Bundled with freeware, adware downloads. | Data harvesting, targeted advertising, unauthorized microphone/camera access. |
| **Botnet / Bot** | Compromised device ("zombie") commanded by a C2 server. | Exploited vulnerabilities, default credentials. | Coordinated DDoS attacks, high-volume spam delivery, crypto-mining. |
| **Fileless Malware** | Operates strictly in volatile RAM without writing binary files to disk. | Exploits living-off-the-land binaries (LOLBins: PowerShell, WMI). | Evades traditional disk-based antivirus scanners. |

---

## 4. Network Sniffing, ARP Spoofing & Man-in-the-Middle (MITM)

### 4.1 Network Sniffing Principles
* **Packet Sniffing**: Capturing raw network traffic passing across physical or wireless network interfaces using tools like **Wireshark** or **tcpdump**.
* **Promiscuous Mode**: A network interface card (NIC) setting that instructs the hardware to pass all received frames to the operating system, rather than just frames addressed to its own MAC address or broadcast address.
* **Hub vs Switch Environments**:
  * **Hubs (Layer 1)**: Broadcast every frame out of all ports. Any host running a sniffer can see all conversations passively.
  * **Switches (Layer 2)**: Forward frames strictly to the destination port matching the destination MAC address recorded in the switch’s **Content Addressable Memory (CAM) Table**. Sniffing on a switch requires active redirection (ARP Spoofing or MAC Flooding).

---

### 4.2 ARP Spoofing / Poisoning & MITM Attacks

The Address Resolution Protocol (**ARP**, RFC 826) dynamically resolves 32-bit IPv4 addresses to 48-bit Layer 2 MAC addresses.

#### The Fundamental Vulnerability of ARP
**ARP is entirely stateless and unauthenticated**:
* Hosts accept **unsolicited (Gratuitous) ARP replies** without ever having sent an ARP request.
* Devices blindly overwrite their internal ARP cache tables whenever an incoming ARP packet is received.

```
                      +-----------------------------+
                      |   ATTACKER (192.168.1.50)   |
                      |     MAC: CC:CC:CC:CC:CC:CC  |
                      +-----------------------------+
                        ▲                        ▲
       Forged ARP Reply |                        | Forged ARP Reply
  "192.168.1.1 is at CC"|                        | "192.168.1.10 is at CC"
                        ▼                        ▼
+-----------------------------+            +-----------------------------+
|    VICTIM (192.168.1.10)    |            |   GATEWAY (192.168.1.1)     |
|     MAC: AA:AA:AA:AA:AA:AA  |            |     MAC: BB:BB:BB:BB:BB:BB  |
+-----------------------------+            +-----------------------------+
      ARP Table Poisoned:                        ARP Table Poisoned:
   192.168.1.1 -> CC:CC:CC:CC                 192.168.1.10 -> CC:CC:CC:CC
```

#### Step-by-Step Attack Execution
1. Attacker sends forged gratuitous ARP replies to the **Victim**: `"192.168.1.1 (Gateway) is at MAC CC:CC:CC:CC:CC:CC (Attacker)"`.
2. Attacker sends forged gratuitous ARP replies to the **Gateway**: `"192.168.1.10 (Victim) is at MAC CC:CC:CC:CC:CC:CC (Attacker)"`.
3. Both machines update their ARP caches with the attacker’s MAC address.
4. When the victim transmits internet traffic, packets are sent to the attacker's MAC address.
5. The attacker enables IP forwarding (`sysctl -w net.ipv4.ip_forward=1`), inspects or modifies packets in real-time using Wireshark or Ettercap, and forwards them to the genuine gateway.
6. The attacker has successfully established a **Man-in-the-Middle (MITM)** position.

#### Defensive Countermeasures
1. **Dynamic ARP Inspection (DAI)**: Enterprise switch security feature that validates ARP packets against the **DHCP Snooping binding database**, dropping invalid or forged ARP replies.
2. **Port Security**: Locks a switch port to specific authorized MAC addresses (sticky MAC) to prevent rogue MAC injection.
3. **Static ARP Tables**: Manually mapping critical gateway IPs to static MAC entries (`arp -s 192.168.1.1 BB-BB-BB-BB-BB-BB`).
4. **End-to-End Encryption**: Enforcing TLS 1.3, SSH, and VPN tunnels so that intercepted packets remain unreadable plaintext.

---

## 5. Session Hijacking

Session hijacking (Cookie hijacking) is the exploitation of a valid computer session to gain unauthorized access to data or services:
* **Mechanism**: Once a user authenticates, the server issues a unique, high-entropy **Session Token** (often stored in an HTTP cookie). The client sends this token with subsequent requests to prove identity.
* **Vectors of Hijacking**:
  * **Network Sniffing**: Sniffing unencrypted HTTP traffic over open Wi-Fi to capture the session cookie.
  * **Cross-Site Scripting (XSS)**: Injecting malicious JavaScript to read and exfiltrate `document.cookie`.
  * **Session Fixation**: Forcing a victim to authenticate using a pre-set session ID known to the attacker.
* **Defensive Controls**:
  * **HTTPS Everywhere**: Encrypt all traffic to prevent transport sniffing.
  * **Cookie Security Flags**:
    * `HttpOnly`: Prevents client-side scripts (JavaScript) from accessing the cookie, completely neutralizing XSS-based cookie theft.
    * `Secure`: Mandates that the browser only transmit the cookie over encrypted HTTPS connections.
    * `SameSite=Strict`: Restricts cookie transmission in cross-site requests, mitigating Cross-Site Request Forgery (CSRF).
  * **Session Regeneration**: Generating a completely new session token immediately upon successful login.

---

## 6. Network Security Controls: Firewalls and IDS/IPS

```
                                [ INTERNET ]
                                     │
                                     ▼
                            [ Screening Router ]
                                     │
                                     ▼
                     [ Firewall / Stateful Inspection ]
                                     │
                ┌────────────────────┴────────────────────┐
                ▼                                         ▼
        [ DMZ Subnet ]                         [ Internal Firewall ]
   (Public Web, Mail, DNS)                                │
                                                          ▼
                                                [ Trusted LAN Network ]
                                                (Workstations, DBs, AD)
```

### 6.1 Evolution of Firewall Technologies

#### 1. Packet Filtering Firewalls (1st Generation - Layer 3/4)
* Operates at Network and Transport layers (IP and TCP/UDP).
* Evaluates each packet in isolation against an Access Control List (ACL) based on: Source IP, Destination IP, Protocol, Source Port, Destination Port.
* **Limitations**: Stateless. Lacks awareness of connection states; cannot detect application-level payload attacks or spoofed TCP flags.

#### 2. Stateful Inspection Firewalls (2nd Generation - Layer 3/4/5)
* Maintains a dynamic **State Table** tracking active TCP and UDP connections.
* Tracks TCP handshake states (`SYN_SENT`, `ESTABLISHED`, `FIN_WAIT`).
* Automatically permits return traffic matching an established outgoing connection, closing ports dynamically when sessions terminate.
* **Advantage**: Superior security over static packet filtering; resists TCP spoofing attacks.

#### 3. Application Proxy Firewalls (3rd Generation - Layer 7)
* Sits as an intermediary between client and server, terminating the connection and regenerating a new connection to the destination.
* Inspects full Layer 7 application protocols (HTTP, FTP, SMTP, DNS).
* **Advantage**: Deep content inspection; hides internal network addressing.
* **Limitation**: High processing latency; cannot scale to high-bandwidth enterprise cores.

#### 4. Next-Generation Firewalls (NGFW - Modern Standard)
* Integrates stateful inspection with **Deep Packet Inspection (DPI)**, native **Intrusion Prevention Systems (IPS)**, **Application Awareness** (identifies applications regardless of port, e.g., BitTorrent or Skype tunneling over port 443), and real-time cloud threat intelligence feeds.

---

### 6.2 Intrusion Detection Systems (IDS) vs Intrusion Prevention Systems (IPS)

```
+---------------------------------------------------------------------------------------+
|                                  IDS vs IPS PARADIGMS                                 |
+-------------------------------------------+-------------------------------------------+
|      INTRUSION DETECTION SYSTEM (IDS)     |     INTRUSION PREVENTION SYSTEM (IPS)     |
+-------------------------------------------+-------------------------------------------+
| * Passive, Out-of-band monitoring.        | * Active, In-line traffic processing.     |
| * Connected via SPAN / TAP mirror port.   | * Sits directly in physical packet path.  |
| * Detects and ALERTS administrators.      | * Detects and BLOCKS / DROPS packets.     |
| * Zero impact on network throughput.      | * Introduces slight processing latency.   |
| * Generates SIEM alerts and logs.         | * Resets TCP sessions (sends RST packets).|
+-------------------------------------------+-------------------------------------------+
```

#### 1. Detection Engines
* **Signature-Based Detection**:
  * Compares packet byte sequences against a predefined database of known attack signatures (similar to antivirus rules).
  * *Advantage*: Highly accurate and fast for known, existing threats.
  * *Disadvantage*: Completely blind to **Zero-Day (0-day) vulnerabilities** and polymorphic malware.
* **Anomaly-Based (Behavioral) Detection**:
  * Establishes a baseline profile of normal network activity (traffic volume, bandwidth, protocols used). Alerts when current traffic deviates significantly from baseline thresholds (e.g., sudden spike in outbound SSH connections).
  * *Advantage*: Capable of identifying novel Zero-Day exploits.
  * *Disadvantage*: Prone to **high False Positive rates** caused by legitimate network usage spikes.

#### 2. Deployment Architectures
* **Network-based (NIDS / NIPS)**: Analyzes traffic across an entire network segment (e.g., Snort, Suricata).
* **Host-based (HIDS / HIPS)**: Installed directly on an endpoint operating system; analyzes system calls, local file integrity, Windows Event Logs, and kernel execution (e.g., OSSEC, Wazuh, Tripwire).
