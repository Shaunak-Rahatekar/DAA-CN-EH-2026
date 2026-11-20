# End-Semester Examination: Ethical Hacking (MD***)

**Course Code**: MD*** | **Subject**: Ethical Hacking  
**Duration**: 2 Hours (11:00 AM – 1:00 PM) | **Maximum Marks**: 60  
*Instructions*:
1. Answer **ALL** questions in Section A (Compulsory).
2. Answer any **TWO** questions from Section B.
3. Answer any **TWO** questions from Section C.
4. Draw neat, labeled diagrams and show step-by-step reasoning where required.
5. Universal Unicode notation is used throughout.

---

## SECTION A: Short Answer Conceptual Questions (Compulsory)
**Answer all 5 questions. Each question carries 4 marks.** (5 × 4 = 20 Marks)

### Q1. [4 Marks] (Unit I)
(a) Define Vulnerability, Threat, and Exploit. State the relationship between them using the standard Cyber Risk formula, and provide a concrete real-world example illustrating their intersection. [2M]  
(b) Explain the CIA Triad and define the property of **Non-Repudiation**. State which cryptographic mechanism guarantees non-repudiation in electronic transactions. [2M]

### Q2. [4 Marks] (Unit II)
(a) Differentiate between **Passive Reconnaissance** and **Active Reconnaissance** with respect to direct target interaction, detection surface by intrusion detection systems (IDS), and typical tools utilized. [2M]  
(b) Explain the mechanism and security risks associated with an unrestricted **DNS Zone Transfer (AXFR)**. Provide the exact CLI command used to query for an AXFR transfer. [2M]

### Q3. [4 Marks] (Unit III)
(a) Define **Password Spraying** and **Credential Stuffing**. Explain why password spraying successfully bypasses traditional account lockout policies. [2M]  
(b) What is cryptographic **Salting** in password storage? Write the mathematical expression for a salted hash and explain why salting neutralizes precomputed **Rainbow Table** attacks. [2M]

### Q4. [4 Marks] (Unit IV)
(a) Explain how a **Tautology SQL Injection attack** (`' OR '1'='1' --`) results in an authentication bypass. Explain why Parameterized Prepared Statements structurally eliminate SQL injection. [2M]  
(b) Contrast **Reflected XSS** (Type I) with **Stored XSS** (Type II) based on persistence, delivery vector, and audience impact. [2M]

### Q5. [4 Marks] (Unit V)
(a) Contrast the target audience, technical depth, and primary objective of the **Executive Summary** versus the **Detailed Technical Findings** section of a VAPT report. [2M]  
(b) In enterprise Windows security auditing, state the technical meaning of **Event ID 4624**, **Event ID 4625**, and **Event ID 4720**. Which of these is monitored to detect brute-force attacks? [2M]

---

## SECTION B: Medium Analytical & Methodological Questions
**Answer any TWO questions from Q6, Q7, and Q8. Each carries 8 marks.** (2 × 8 = 16 Marks)

### Q6. [8 Marks] (Units I & II)
(a) Walk through the 5 phases of the **CEH Ethical Hacking Methodology** in sequential order. For each phase, describe the technical objective and explain the critical distinction between how a malicious black hat acts versus how an authorized ethical hacker acts during that phase. [4M]  
(b) Analyze the packet exchange mechanics of **Nmap Port Scanning**:
    1. Draw and explain the sequence for **TCP Connect Scan (`-sT`)** versus **TCP SYN Stealth Scan (`-sS`)**.
    2. Explain how **RFC 793 Inverse Scans** (Null `-sN`, FIN `-sF`, Xmas `-sX`) determine port states and why they fail against Microsoft Windows hosts.
    3. Define the three primary port states: **Open**, **Closed**, and **Filtered**. [4M]

### Q7. [8 Marks] (Unit III)
(a) **Address Resolution Protocol (ARP) Vulnerability & MITM**:
    1. Explain the fundamental architectural flaw in the design of ARP (RFC 826).
    2. Walk through the step-by-step execution of an **ARP Spoofing Man-in-the-Middle (MITM)** attack between a victim workstation (`192.168.1.10`) and default gateway (`192.168.1.1`) with an illustrative diagram.
    3. Detail how **Dynamic ARP Inspection (DAI)** on enterprise managed switches prevents ARP poisoning. [4M]  
(b) **Network Security Controls & Firewalls**:
    1. Compare the 4 generations of firewalls: **Packet Filtering (L3/4)**, **Stateful Inspection (L4/5)**, **Application Proxy (L7)**, and **Next-Generation Firewalls (NGFW)**.
    2. Draw a **Screened-Subnet DMZ Architecture**. State which public servers belong in the DMZ and state the fundamental access rule governing DMZ-to-Internal LAN traffic. [4M]

### Q8. [8 Marks] (Units IV & V)
(a) **Wireless Security Evolution & Protocols**:
    1. Construct a comprehensive comparison table contrasting **WEP**, **WPA**, **WPA2**, and **WPA3** across encryption cipher, integrity mechanism, and known vulnerabilities.
    2. Describe the **WPA2 4-Way Handshake** and explain how an attacker utilizes spoofed **Deauthentication Frames** to capture the handshake for offline dictionary cracking.
    3. Explain how **WPA3 Simultaneous Authentication of Equals (SAE)** resolves this vulnerability. [4M]  
(b) **Incident Response & Defense**:
    1. Draw and explain the 4 phases of the **NIST SP 800-61 Incident Response Lifecycle**.
    2. Detail the specific actions undertaken during **Short-Term Containment**, **Eradication**, and **Recovery** when responding to an active enterprise ransomware outbreak. [4M]

---

## SECTION C: Comprehensive Scenario & Applied Design Questions
**Answer any TWO questions from Q9, Q10, and Q11. Each carries 12 marks.** (2 × 12 = 24 Marks)

### Q9. [12 Marks] (Units I, II & IV — External Web Assessment & Cyber Law)
A multinational fintech bank commissions an external penetration test on its online banking portal `https://ebank.corp.com`.
(a) **Reconnaissance & OSINT**: Design a passive reconnaissance strategy to map the target's external attack surface. Formulate 4 concrete **Google Dorks** targeting exposed database backups, admin portals, and environment configuration files. Explain why relying on `robots.txt` for security is a dangerous misconception. [4M]  
(b) **Web Application Exploitation**:
    1. An attacker discovers that user transaction statements are fetched via `GET /api/statement?account_id=10042`. Explain how tampering with this parameter leads to **Horizontal Privilege Escalation (IDOR)**, and provide the server-side code remediation.
    2. Explain how an attacker constructs a **Cross-Site Request Forgery (CSRF)** attack against the funds transfer endpoint. Detail how **Anti-CSRF Synchronizer Tokens** and the **`SameSite=Strict`** cookie attribute structurally neutralize CSRF. [4M]  
(c) **Legal Framework (Indian IT Act 2000)**:
    1. Analyze the legal provisions, scope of offences, and maximum penalties under **Section 43, Section 66, Section 66C, Section 66D, and Section 66F (Cyber Terrorism)**.
    2. State the 4 mandatory legal prerequisites (e.g., SOW, RoE, NDA, Letter of Authorization) an ethical hacker must execute before initiating testing to avoid criminal prosecution under Section 66. [4M]

### Q10. [12 Marks] (Units III, IV & V — Internal Penetration Testing & Lateral Movement)
During an internal network security assessment, you are connected to a standard corporate employee VLAN.
(a) **Wireless Exploitation & Rogue APs**: Walk through the deployment of an **Evil Twin Access Point Attack**. Describe how the attacker clones the corporate SSID, forces client disconnection, and harvests Active Directory credentials via a rogue captive portal. Propose 3 campus wireless hardening countermeasures including **Protected Management Frames (PMF)**. [4M]  
(b) **Privilege Escalation on Compromised Endpoints**:
    1. On a compromised Linux server, explain how an ethical hacker discovers and exploits misconfigured **SUID binaries** (e.g., `find` or `vim`) and insecure `/etc/sudoers` rules to spawn a root shell.
    2. On a Windows workstation, explain how an attacker exploits **Unquoted Service Paths** to escalate privileges to `NT AUTHORITY\SYSTEM`. [4M]  
(c) **Metasploit Exploitation & Pivoting**:
    1. Explain the operational workflow of configuring Metasploit's `exploit/windows/smb/ms17_010_eternalblue` with a `windows/x64/meterpreter/reverse_tcp` payload caught by `exploit/multi/handler`.
    2. Define **Network Pivoting**. Explain how an attacker uses a compromised dual-homed DMZ host to route traffic and attack internal database subnets that are unreachable from the outside. State the mandatory ethical cleanup rules prior to engagement closure. [4M]

### Q11. [12 Marks] (Units I & V — Incident Analysis, CVSS v3.1 & VAPT Reporting)
A healthcare provider suffered a major breach where an unauthenticated remote attacker exploited a vulnerable Apache service listening on external port 443, executing commands as `root` and accessing 500,000 patient electronic health records.
(a) **Cyber Case Studies & Lessons**:
    1. Contrast the **WannaCry (2017)** ransomware attack with the **Equifax (2017)** data breach in terms of vulnerability exploited, propagation vector, and business damage.
    2. Formulate a 5-step enterprise **Patch Management Lifecycle** that directly addresses the root causes of both incidents. [4M]  
(b) **CVSS v3.1 Base Metric Evaluation**:
    Evaluate the vulnerability based on CVSS v3.1 criteria:
    * Attack Vector (`AV`): Network vs Local
    * Attack Complexity (`AC`): Low vs High
    * Privileges Required (`PR`): None vs High
    * User Interaction (`UI`): None vs Required
    * Scope (`S`): Unchanged vs Changed
    * Impact: Confidentiality (`C`), Integrity (`I`), Availability (`A`)
    Derive the exact **CVSS v3.1 Vector String**, calculate the base score, and state its qualitative severity band (None, Low, Medium, High, Critical). [4M]  
(c) **Professional VAPT Report Finding Entry**:
    Draft a complete, production-grade **Technical Finding Entry** for this vulnerability matching professional VAPT reporting standards, containing: Finding Title, Severity Rating, Affected Asset, Vulnerability Description, Proof of Concept (PoC) trace, Real-World Business Impact, and Step-by-Step Technical Remediation. [4M]
