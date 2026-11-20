# Ethical Hacking (EH) — Unit V Model Answers & Marking Scheme
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit V**: Penetration Testing, Reporting and Defense

---

## Answer 5.1 [4 Marks]
### Marking Scheme:
* **Part (a) Executive Summary vs Technical Findings**: 2 Marks (Audience, depth, content, objective)
* **Part (b) SIEM Centralized Logging & 3 Windows Event IDs**: 2 Marks (1 mark SIEM concept + 1 mark Event IDs)

### Model Answer:
#### Part (a) Executive Summary vs Detailed Technical Findings

| Dimension | Executive Summary | Detailed Technical Findings |
| :--- | :--- | :--- |
| **Target Audience** | C-suite leadership (CEO, CIO, CISO), Board of Directors, and non-technical stakeholders. | System administrators, network engineers, software developers, and security operations analysts. |
| **Technical Depth** | High-level; deliberately eliminates technical jargon, shell code, and raw exploits. | In-depth; contains exact exploit strings, memory addresses, port bindings, and packet traces. |
| **Content Focus** | Overall organizational security posture, business risks, financial liability, and high-level strategy. | Specific vulnerability descriptions, CVSS v3.1 vector strings, step-by-step Proof of Concept (PoC) screenshots. |
| **Strategic Objective**| Justify security budget allocation, human resource needs, and executive governance decisions. | Provide actionable, line-by-line configuration changes, source code patches, and remediation checklists. |

#### Part (b) SIEM Log Analysis & Windows Event IDs
1. **Role of Centralized SIEM Log Analysis**:
   * A Security Information and Event Management (SIEM) system ingests, normalizes, and aggregates log events from diverse endpoints, network firewalls, domain controllers, and databases.
   * By evaluating cross-device correlation rules in real time (e.g., correlating a perimeter firewall port probe with 20 failed server logins), SIEM identifies multi-stage advanced cyberattacks that isolated host logs miss.
2. **Three Critical Windows Security Event IDs**:
   * **Event ID 4625 (An account failed to log on)**: High burst frequencies indicate online password brute-forcing, dictionary attacks, or automated credential stuffing.
   * **Event ID 4624 (An account was successfully logged on)**: Analyzes Logon Type (e.g., Type 3 Network, Type 10 RDP) to detect anomalous off-hours administrative access.
   * **Event ID 4720 (A user account was created)**: Flags unauthorized persistence mechanisms established by an adversary following initial privilege escalation.

---

## Answer 5.2 [4 Marks]
### Marking Scheme:
* **WannaCry Analysis (Vulnerability, Exploit & 2 Lessons)**: 2 Marks
* **Equifax Analysis (Vulnerability & 2 Lessons)**: 2 Marks

### Model Answer:
#### 1. WannaCry Incident (May 2017)
* **Vulnerability & Exploit**:
  * Exploited an unpatched Remote Code Execution (RCE) buffer overflow vulnerability in Microsoft’s legacy **Server Message Block version 1 (SMBv1)** protocol (`CVE-2017-0143` / Microsoft Security Bulletin `MS17-010`).
  * Leveraged the NSA-leaked `EternalBlue` exploit coupled with the `DoublePulsar` backdoor payload.
  * Operated as an autonomous **cryptoworm**, self-propagating across local subnets and public internet IPs on TCP port 445 without requiring human user interaction.
* **Key Enterprise Defense Lessons**:
  1. **Enforce Rapid Patch Deployment**: Microsoft had published patch MS17-010 two months prior to the outbreak; unpatched legacy systems were compromised.
  2. **Deprecate Legacy Protocols & Enforce Segmentation**: Completely disable insecure legacy protocols (SMBv1) and block external inbound access to sensitive administration ports (ports 445, 139, 3389) at perimeter firewalls.

#### 2. Equifax Data Breach (2017)
* **Vulnerability & Root Cause**:
  * Remote Code Execution vulnerability in the **Apache Struts 2** web framework (`CVE-2017-5638`).
  * Caused by improper handling of untrusted input inside the `Content-Type` HTTP header when parsing file uploads with Jakarta Multipart parser, allowing arbitrary command execution. Resulted in the exfiltration of sensitive personal data (names, SSNs, credit cards) of **147 million consumers**.
* **Key Enterprise Defense Lessons**:
  1. **Maintain an Accurate Software Bill of Materials (SBOM)**: Equifax lacked an accurate inventory of where Apache Struts was deployed across their server fleet, preventing timely patch application.
  2. **Implement Network Micro-Segmentation & File Integrity Monitoring**: Web applications must be isolated from internal core transactional databases to prevent attackers from querying backend databases following initial perimeter web compromise.

---

## Answer 5.3 [8 Marks]
### Marking Scheme:
* **Part (a) PTES 7 Phases & Deliverables**: 4 Marks (Sequential explanation with deliverables)
* **Part (b) Metasploit Modular Architecture & Meterpreter**: 4 Marks (2.5 marks modules + 1.5 marks Meterpreter advantages)

### Model Answer:
#### Part (a) Penetration Testing Execution Standard (PTES) 7 Phases

```
[1. Pre-engagement] ──> [2. Intelligence] ──> [3. Threat Modeling] ──> [4. Vuln Analysis]
                                                                                │
[7. Reporting] <── [6. Post-Exploitation] <── [5. Exploitation] <───────────────┘
```

1. **Pre-engagement Interactions**:
   * *Objective*: Establish legal boundaries, scope, target IP lists, testing windows, and emergency communication channels.
   * *Deliverables*: Signed Statement of Work (SOW), Non-Disclosure Agreement (NDA), and Rules of Engagement (RoE).
2. **Intelligence Gathering (Reconnaissance)**:
   * *Objective*: Accumulate maximum technical intelligence about the target using passive OSINT, WHOIS, DNS queries, and social media.
   * *Deliverables*: Attack surface map, IP address allocations, email formats, and tech stack inventory.
3. **Threat Modeling**:
   * *Objective*: Categorize business assets, map business risks, and define realistic adversary capability profiles.
   * *Deliverables*: Threat matrix ranking high-value targets (databases, domain controllers).
4. **Vulnerability Analysis**:
   * *Objective*: Systematically discover and catalog security weaknesses using automated scanners (Nessus, OpenVAS) and manual verification.
   * *Deliverables*: Verified list of vulnerabilities with CVSS scores.
5. **Exploitation**:
   * *Objective*: Actively bypass security boundaries to prove exploitability while ensuring zero operational downtime.
   * *Deliverables*: Validated Proof of Concept (PoC) and initial foothold access.
6. **Post-Exploitation**:
   * *Objective*: Determine the business impact of the compromise via privilege escalation, sensitive data identification, and pivoting.
   * *Deliverables*: Documented lateral movement paths and evidence of high-value business risk.
7. **Reporting**:
   * *Objective*: Translate technical findings into prioritized remediation plans for both management and technical teams.
   * *Deliverables*: Formal VAPT Assessment Report (Executive Summary + Technical Findings).

---

#### Part (b) Metasploit Framework Modular Architecture

1. **Core Modules**:
   * **Exploits (`exploit/`)**: Active modules containing shellcode designed to target and leverage specific software vulnerabilities (e.g., `exploit/windows/smb/ms17_010_eternalblue`).
   * **Payloads (`payload/`)**: Code executed on the victim machine once the exploit completes:
     * *Singles*: Standalone, self-contained shellcode (e.g., execute `calc.exe` or add local user).
     * *Stagers*: Small downloaders that establish a network connection back to the attacker and inject a larger stage payload into target memory.
   * **Auxiliaries (`auxiliary/`)**: Modules that perform non-payload operations, including port scanning (`auxiliary/scanner/portscan/tcp`), fuzzing, SNMP enumeration, and denial of service.
   * **Encoders (`encoder/`)**: Obfuscates shellcode (e.g., `x86/shikata_ga_nai`) to eliminate bad characters (`\x00` null bytes) that would break memory injection.
   * **Multi/Handler (`exploit/multi/handler`)**: A universal listener that catches incoming reverse network connections from staged payloads.

2. **Why Meterpreter is Preferred Over a Standard Command Shell**:
   * **Runs Purely in Memory (Reflective DLL Injection)**: Meterpreter executes entirely in RAM without writing binary files to disk, bypassing traditional signature-based antivirus and leaving minimal forensic artifacts.
   * **Encrypted Communication (TLS/SSL)**: All command-and-control (C2) communication between Meterpreter and the attacker is strongly encrypted, defeating network IDS/IPS inspection.
   * **Extensible Post-Exploitation APIs**: Provides built-in advanced operational commands unavailable in standard cmd/bash shells, such as `getsystem` (automated Windows privilege escalation), `keyscan_start` (in-memory keystroke logging), `screenshot`, `hashdump` (extracting SAM database hashes), and transparent network route pivoting.

---

## Answer 5.4 [8 Marks]
### Marking Scheme:
* **Part (a) NIST SP 800-61 Incident Response Lifecycle**: 5 Marks (1 mark diagram + 1 mark each for Phase 1, 2, 3, 4 with containment/eradication/recovery detail)
* **Part (b) Pivoting & Ethical Cleanup Mandates**: 3 Marks (1.5 marks pivoting + 1.5 marks cleanup rules)

### Model Answer:
#### Part (a) NIST SP 800-61 Incident Response Lifecycle

```
        ┌─────────────────────────────────────────────────────────┐
        ▼                                                         │
[ 1. PREPARATION ] ──► [ 2. DETECTION & ANALYSIS ] ──► [ 3. CONTAINMENT, ERADICATION ]
                                                              │   & RECOVERY
                                                              ▼
                                                   [ 4. POST-INCIDENT ACTIVITY ]
                                                         (Lessons Learned)
```

1. **Phase 1: Preparation**:
   * Establish the Computer Security Incident Response Team (CSIRT).
   * Maintain incident playbooks for ransomware, data exfiltration, and DDoS.
   * Deploy jump-bags, forensic software (EnCase, FTK), and isolated analysis networks.
2. **Phase 2: Detection and Analysis**:
   * Monitor and triage incoming alerts from SIEM, EDR, and network firewalls.
   * Identify Indicators of Compromise (IOCs: MD5/SHA256 hashes, C2 IP addresses).
   * Determine the scope, severity, and affected enterprise systems.
3. **Phase 3: Containment, Eradication, and Recovery**:
   * **Short-Term Containment**: Stop immediate lateral infection spread by logically isolating the affected host (pulling physical network cable, isolating via EDR agent) and blocking malicious C2 IPs at perimeter firewalls.
   * **Long-Term Containment**: Capture volatile RAM memory dumps and bit-stream forensic disk images while maintaining legal chain of custody.
   * **Eradication**: Eliminate the root cause: delete all malicious binaries, remove attacker backdoors, clean persistence registry keys/cron jobs, and patch the exploited vulnerability.
   * **Recovery**: Restore compromised hosts from clean, verified offline backups. Staged reconnect to production under enhanced logging and monitoring.
4. **Phase 4: Post-Incident Activity (Lessons Learned)**:
   * Conduct a formal post-mortem meeting with all stakeholders.
   * Review incident timeline, procedural shortcomings, and missed detection indicators.
   * Update defensive controls, correlation rules, and incident playbooks (closing the feedback loop to Phase 1).

---

#### Part (b) Pivoting & Ethical Cleanup Rules

1. **Concept of Pivoting**:
   * **Pivoting** is the post-exploitation technique of using a compromised machine (the pivot host) as a relay, bridge, or proxy to launch scans and attacks against internal network subnets that are physically or logically inaccessible to the attacker's initial network position.
   * *Example*: An ethical hacker compromises a public web server residing in the DMZ (`198.51.100.10`). The web server possesses a second internal interface (`10.0.1.5`). By routing Metasploit traffic through the web server (`route add 10.0.1.0 255.255.255.0`), the tester can probe internal databases residing inside the protected corporate LAN.

2. **Mandatory Ethical Cleanup Rules**:
   Professional ethical hacking contracts mandate complete restoration of the target environment:
   * **Remove All Testing Tools & Payloads**: Delete all uploaded webshells, compiled binaries, scripts, and temporary utilities.
   * **Remove Created Accounts**: Delete any local or Active Directory testing accounts created during privilege escalation testing.
   * **Revert Configurations**: Restore modified registry keys, SUID permissions, firewall rules, and scheduled tasks to their original states.
   * **Verify System Stability**: Confirm with client systems administrators that all operational services are functioning normally before final formal sign-off.

---

## Answer 5.5 [10 Marks]
### Marking Scheme:
* **Part (a.1) CVSS v3.1 Metric Breakdown**: 3.5 Marks (0.5 marks per metric: AV, AC, PR, UI, S, C, I, A)
* **Part (a.2) Vector String & Severity Band**: 1.5 Marks (Exact vector string + Critical score)
* **Part (b.1) VAPT Technical Finding Entry**: 3 Marks (Title, severity, asset, PoC, remediation)
* **Part (b.2) Security Hardening & Patch Management Lifecycle**: 2 Marks

### Model Answer:
#### Part (a) CVSS v3.1 Scoring & Risk Rating

1. **Evaluation of Base Metric Group**:
   * **Attack Vector (AV:N - Network)**: The vulnerability is remotely exploitable over public port 443 across the Internet without requiring local physical or intranet access.
   * **Attack Complexity (AC:L - Low)**: Exploitation is reliable and repeatable without specialized race conditions or environment dependencies.
   * **Privileges Required (PR:N - None)**: The attacker does not require prior authentication or valid user credentials.
   * **User Interaction (UI:N - None)**: The vulnerability is exploited automatically without requiring a victim to click a link or open a file.
   * **Scope (S:U - Unchanged)**: The impacted component and vulnerable component are within the same security authority boundary.
   * **Confidentiality Impact (C:H - High)**: Complete read access to all system files, database credentials, and operating system data.
   * **Integrity Impact (I:H - High)**: Total compromise of system integrity; arbitrary files can be modified or deleted.
   * **Availability Impact (A:H - High)**: The attacker can crash, terminate, or encrypt system processes, leading to total service denial.

2. **CVSS v3.1 Vector String & Final Score**:
   $$\text{CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H}$$
   * **Calculated Base Score**: **9.8**
   * **Qualitative Severity Band**: **CRITICAL** (Scale: 9.0 – 10.0).

---

#### Part (b.1) Professional VAPT Technical Finding Entry

```text
========================================================================================
FINDING ID: VULN-2026-01 | SEVERITY: CRITICAL (CVSS v3.1 Base Score: 9.8)
TITLE: Unauthenticated Remote Code Execution (RCE) in Image Processing Library
========================================================================================
AFFECTED ASSET:
https://portal.bank.com:443/api/v1/profile/upload-avatar

CVSS v3.1 VECTOR:
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H

VULNERABILITY DESCRIPTION:
The web application uses an outdated third-party image processing library (libImage v2.1)
to handle avatar uploads. The library fails to validate file buffer bounds when parsing
TIFF image headers, resulting in a heap-based buffer overflow that allows an unauthenticated
remote attacker to execute arbitrary shell commands with root privileges.

PROOF OF CONCEPT (PoC):
1. Attacker sends a crafted POST request containing shellcode in the image header:
   POST /api/v1/profile/upload-avatar HTTP/1.1
   Host: portal.bank.com
   Content-Type: image/tiff
   [... Payload injecting reverse shell connecting to 198.51.100.5:4444 ...]
2. Metasploit multi/handler catches root shell:
   meterpreter > getuid
   Server username: root (uid=0)

BUSINESS IMPACT:
Immediate, total compromise of the application server. Adversary can read backend customer
financial records, pivot into the internal core banking subnet, deploy ransomware, or take
the core banking portal offline.

REMEDIATION STEPS:
1. Immediate Action: Update libImage to patched version >= v2.4.1.
2. Temporary Mitigation: Disable TIFF image processing at the reverse proxy (WAF).
3. Privilege Hardening: Reconfigure the web service daemon to execute under an unprivileged
   dedicated user account (e.g., www-data) rather than root.
========================================================================================
```

---

#### Part (b.2) Enterprise Hardening & Patch Management Lifecycle

To structurally eliminate similar vulnerabilities across the enterprise, implement a 5-stage lifecycle:
1. **Asset & Dependency Inventory (SBOM)**: Maintain a centralized, automated Software Bill of Materials (SBOM) cataloging all third-party libraries, container images, and software packages deployed across the enterprise.
2. **Vulnerability Monitoring & Prioritization**: Integrate automated Software Composition Analysis (SCA) tools into CI/CD pipelines. Flag any dependency with a CVSS score $\ge 7.0$ for emergency review.
3. **Staging Environment Validation**: Test vendor-released security patches in an isolated staging environment matching production configurations to verify system stability and prevent regression bugs.
4. **Enforce SLAs for Patch Deployment**:
   * *Critical Vulnerabilities (CVSS 9.0 - 10.0)*: Remediate within **48 hours**.
   * *High Vulnerabilities (CVSS 7.0 - 8.9)*: Remediate within **7 days**.
5. **Least Privilege & Container Hardening**: Run all web workloads inside hardened, rootless containers or isolated DMZ virtual machines with AppArmor/SELinux profiles restricting outbound network connections.
