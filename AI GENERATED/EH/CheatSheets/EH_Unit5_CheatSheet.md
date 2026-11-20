# EH Unit 5: Penetration Testing, Reporting and Defense
**Course**: Ethical Hacking (MD***) | **VIT Pune** | **Exam Date**: 27 Nov 2026

---

### 1. Penetration Testing Frameworks (PTES & NIST)
* **PTES (7 Phases)**: Pre-engagement $\rightarrow$ Intelligence Gathering $\rightarrow$ Threat Modeling $\rightarrow$ Vulnerability Analysis $\rightarrow$ Exploitation $\rightarrow$ Post-Exploitation $\rightarrow$ Reporting.
* **NIST SP 800-115**: Technical testing guide: Planning $\rightarrow$ Discovery $\rightarrow$ Attack $\rightarrow$ Reporting.
* **OSSTMM**: Scientifically quantifiable operational security metrics.
* **OWASP WSTG**: Web security testing standard covering 12 assessment categories.

---

### 2. Metasploit Architecture & Modules
* **Exploits**: Code that triggers a specific vulnerability (e.g., `ms17_010_eternalblue`).
* **Payloads**: Shellcode executed post-exploit.
  * *Singles (Inline)*: Standalone commands.
  * *Stagers*: Small downloaders fetching Stage payload into memory.
  * *Meterpreter*: Reflective in-memory staged payload; encrypted C2, keylogger, `getsystem`.
* **Auxiliaries**: Scanners, fuzzers, port probes, protocol enumerators (no payload execution).
* **Encoders**: Obfuscates shellcode to bypass bad characters (e.g., `shikata_ga_nai`).
* **Multi/Handler**: Listener configured to catch reverse TCP/HTTP shells (`exploit/multi/handler`).

---

### 3. Post-Exploitation & Pivoting
* **Objective**: Measure business impact; identify accessible databases, PII, and financial records.
* **Privilege Escalation**: Upgrading from initial compromised shell to Domain Admin.
* **Pivoting**: Routing traffic through a compromised DMZ host to scan and attack unreachable internal subnets.
* **Cleanup Mandate**: Testers **never leave backdoors**; all tools, accounts, and artifacts must be cleanly scrubbed!

---

### 4. SIEM Log Analysis & Threat Detection
* **SIEM**: Centralized log collection, normalization, correlation, and alerting (Splunk, Wazuh).
* **High-Yield Windows Security Event IDs**:
  * `4624`: Successful Account Logon.
  * `4625`: **Failed Account Logon** (detects brute force and password spraying).
  * `4720`: User Account Created (detects unauthorized persistence).
  * `7045`: New System Service Installed (detects priv-esc / persistence).

---

### 5. NIST Incident Response Lifecycle (SP 800-61)
1. **Preparation**: Form CSIRT, author playbooks, acquire forensic tools, train staff.
2. **Detection & Analysis**: Triage SIEM alerts, validate IOCs, determine incident severity.
3. **Containment, Eradication & Recovery**:
   * *Containment*: Isolate host from LAN; block C2 IP on firewall.
   * *Eradication*: Delete malware, remove backdoors, patch exploited vulnerability.
   * *Recovery*: Restore from verified clean backups; staged production re-introduction.
4. **Post-Incident Activity**: Post-mortem lessons learned meeting; feed gaps back into Preparation.

---

### 6. Security Hardening & High-Profile Case Studies
* **CIS Benchmarks**: Predefined baseline hardening rules (disable unused services, enforce PoLP).
* **Patch Management**: Systematic cycle: Inventory $\rightarrow$ Prioritize $\rightarrow$ Test $\rightarrow$ Deploy $\rightarrow$ Verify.
* **WannaCry (2017)**: Cryptoworm exploiting unpatched SMBv1 (`EternalBlue` MS17-010). *Lesson*: Patch promptly, disable SMBv1, segment subnets.
* **Equifax (2017)**: 147M records breached via unpatched Apache Struts (`CVE-2017-5638`). *Lesson*: Maintain SBOM and patch web frameworks.
* **SolarWinds (2020)**: Supply-chain attack inserting `SUNBURST` backdoor into signed Orion updates. *Lesson*: Build pipeline integrity & zero trust.

---

### 7. VAPT Report Structure & Risk Rating
* **Executive Summary**: Written for C-suite/Board; non-technical, overall risk posture, business impact.
* **Scope & RoE**: Tested IP subnets, URLs, constraints, and legal signatories.
* **Technical Findings**: Title, CVSS score & vector, affected asset, description, PoC screenshot, remediation.
* **Action Plan**: Prioritized patch matrix (Critical $\rightarrow$ High $\rightarrow$ Medium).

---

### 8. CVSS v3.1 Scoring Framework
$$\text{Base Metrics} \implies \text{Score: 0.0 to 10.0}$$
* **Exploitability**: Attack Vector (`AV:N/A/L/P`), Attack Complexity (`AC:L/H`), Privileges Required (`PR:N/L/H`), User Interaction (`UI:N/R`).
* **Scope**: Unchanged (`S:U`) vs Changed (`S:C` - VM breakout / XSS).
* **Impact (CIA Triad)**: Confidentiality (`C:N/L/H`), Integrity (`I:N/L/H`), Availability (`A:N/L/H`).
* **Severity Bands**:
  * **None**: `0.0` | **Low**: `0.1 - 3.9` | **Medium**: `4.0 - 6.9` | **High**: `7.0 - 8.9` | **Critical**: `9.0 - 10.0`
* **Exam Vector**: `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` $\implies$ **9.8 (CRITICAL)**.
