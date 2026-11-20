# Ethical Hacking (EH) — Unit V Examination Questions
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit V**: Penetration Testing, Reporting and Defense

---

### Section A: 4-Mark Questions (Short Answer / Conceptual Clarity)
*Estimated time: 7-8 minutes per question*

#### Q5.1 [4 Marks]
(a) Contrast the **Executive Summary** with the **Detailed Technical Findings** section of a VAPT report based on target audience, technical depth, content focus, and strategic objective. **[2 Marks]**  
(b) Explain the role of Centralized Log Analysis using SIEM in detecting cyber incidents. Identify 3 critical Windows Security Event IDs monitored by a SOC analyst and their technical meanings. **[2 Marks]**

#### Q5.2 [4 Marks]
Analyze the **WannaCry (2017)** and **Equifax (2017)** cyber incidents:
1. Identify the specific software vulnerability and protocol exploited in each case.
2. Formulate 2 critical enterprise defense and patch management lessons derived from each incident. **[4 Marks]**

---

### Section B: 8-Mark Questions (Analytical / Methodological Depth)
*Estimated time: 15-18 minutes per question*

#### Q5.3 [8 Marks]
(a) Walk through the 7 phases of the **Penetration Testing Execution Standard (PTES)** in sequential order. Explain the primary objective and key deliverables of each phase. **[4 Marks]**  
(b) Explain the modular architecture of the **Metasploit Framework**. Contrast **Exploits**, **Payloads** (Singles vs Staged vs Meterpreter), **Auxiliaries**, and **Multi/Handler**. Highlight why Meterpreter is preferred over a standard command shell during post-exploitation. **[4 Marks]**

#### Q5.4 [8 Marks]
(a) Walk through the 4 phases of the **NIST SP 800-61 Incident Response Lifecycle** with a cyclical architecture diagram. Detail the specific activities conducted during Short-Term Containment, Eradication, and Recovery. **[5 Marks]**  
(b) Explain the concept of **Pivoting** and lateral movement during post-exploitation. State the mandatory ethical cleanup rules a penetration tester must follow prior to final sign-off. **[3 Marks]**

---

### Section C: 10-Mark Questions (Comprehensive / Scenario-Based)
*Estimated time: 22-25 minutes*

#### Q5.5 [10 Marks]
During an authorized external penetration test against a banking web application, you uncover an unauthenticated Remote Code Execution (RCE) vulnerability in an image processing library listening on public port 443 that allows an attacker to execute commands as `root` without user interaction.
(a) **CVSS v3.1 Scoring & Risk Rating**:
1. Evaluate the **CVSS v3.1 Base Metric Group** for this vulnerability across Exploitability metrics (AV, AC, PR, UI), Scope (S), and Impact metrics (C, I, A).
2. Construct the exact CVSS v3.1 Vector String and determine the qualitative severity rating (None, Low, Medium, High, Critical) based on the standard CVSS severity bands. **[5 Marks]**  
(b) **VAPT Report Finding Documentation & Remediation**:
1. Draft the complete **Technical Finding Entry** for this vulnerability following professional VAPT reporting standards (Title, Severity, Affected Asset, Description, Impact, and Step-by-Step Remediation).
2. Formulate an enterprise **Security Hardening and Patch Management Lifecycle** to prevent future occurrences of similar vulnerabilities. **[5 Marks]**
