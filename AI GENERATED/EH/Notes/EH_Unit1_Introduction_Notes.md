# Ethical Hacking (MD***) — Unit I: Introduction to Ethical Hacking Comprehensive Notes

**Target Exam**: End-Semester Written Examination (27 November 2026, 11:00 AM – 1:00 PM)  
**Weightage**: ~16 to 20 Marks  
**Universal Notation Rule**: Unicode symbols only (`≤`, `≥`, `×`, `→`, `∈`, `²`, `ⁿ`, `₁`). Clean ASCII diagrams for exam reproduction.

---

## 1. Information Security Fundamentals & Cybersecurity Concepts

Information security focuses on protecting electronic data and information systems from unauthorized access, use, disclosure, disruption, modification, or destruction.

### 1.1 What is Ethical Hacking?
- **Definition**: Ethical hacking involves the **authorized, systematic, and scoped testing** of computer systems, networks, and applications to discover security vulnerabilities that a malicious adversary could exploit.
- **The Core Differentiator (Authorization & Intent)**:
  - What separates an ethical hacker from a cyber criminal is **formal written authorization (Rules of Engagement - RoE)** from the system owner, a strictly defined scope, and defensive intent.
  - Ethical hackers adhere to strict codes of conduct: they never steal data, never cause denial of service unless explicitly scoped, and provide actionable remediation guidance.

### 1.2 The CIA Triad & Extended Security Attributes
The foundation of information security rests on the **CIA Triad**:
1. **Confidentiality**:
   - Ensures that sensitive information is accessible only to authorized entities and protected from unauthorized disclosure or eavesdropping.
   - *Threats*: Packet sniffing, unauthorized data exfiltration, man-in-the-middle attacks, shoulder surfing.
   - *Defenses*: Strong symmetric/asymmetric encryption (AES-256, RSA), Role-Based Access Control (RBAC), data masking, multi-factor authentication (MFA).
2. **Integrity**:
   - Guarantees the accuracy, completeness, and trustworthiness of data over its entire lifecycle; ensures data has not been modified, tampered with, or corrupted in transit or storage.
   - *Threats*: Unauthorized alteration, database SQL injection, replay attacks, malware modification.
   - *Defenses*: Cryptographic hash functions (SHA-256), Digital Signatures, Message Authentication Codes (HMAC), database transaction logs.
3. **Availability**:
   - Guarantees that computing systems, network services, and information assets are accessible to authorized users whenever required.
   - *Threats*: Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) attacks, ransomware encrypting disks, power outages, hardware failures.
   - *Defenses*: High availability clustering, hardware redundancy (RAID), load balancing, offsite automated backups, disaster recovery plans.

#### The Extended Security Attributes:
- **Authenticity**: Verification that a user or system is genuinely who they claim to be (e.g., PKI X.509 certificates, cryptographic handshakes).
- **Non-Repudiation**: Guarantees that the sender of a message cannot deny having sent it, and the receiver cannot deny having received it (achieved via asymmetric Digital Signatures and trusted timestamping).
- **Accountability**: Ensures that every security-relevant action on a system can be traced back uniquely to a specific authenticated user (auditing, centralized logging via SIEM).

---

### 1.3 Vulnerability, Threat, Exploit, and Risk
Understanding these four inter-related concepts is mandatory for the exam:
1. **Asset**: Any valuable item of data, hardware, intellectual property, or process owned by an organization (e.g., customer database, proprietary source code).
2. **Vulnerability**: A flaw, bug, weakness, or misconfiguration in software, hardware, or human processes that can be exploited by an attacker (e.g., unpatched SMBv1 buffer overflow, default root password).
3. **Threat**: Any potential circumstance, event, or entity that has the capability to exploit a vulnerability and cause harm or financial loss to an asset (e.g., a ransomware gang, an insider employee).
4. **Exploit**: A defined, engineered method, script, or software payload that takes advantage of a specific vulnerability to breach a system (e.g., the *EternalBlue* exploit script).
5. **Risk**: The probability and potential business impact resulting from a threat agent exploiting a vulnerability.
   - **Governing Mathematical Formulation**:  
     `Risk = Threat × Vulnerability × Impact (Asset Value)`
   - **Risk Management Treatment Strategies**:
     - *Risk Mitigation*: Applying security controls or patches to reduce risk to an acceptable level.
     - *Risk Avoidance*: Discontinuing the risky activity altogether (e.g., shutting down a legacy insecure server).
     - *Risk Transfer*: Shifting the financial liability to a third party (e.g., buying cyber insurance, outsourcing to a secure cloud provider).
     - *Risk Acceptance*: Acknowledging low-level residual risk where mitigation cost exceeds potential asset damage.

---

## 2. Cyber Threat Landscape & Types of Hackers

### 2.1 Hacker Classifications (Threat Actors)
1. **White Hat Hackers (Ethical Hackers / Security Researchers)**:
   - Fully authorized by written contract.
   - Use identical tools and methodologies as malicious attackers, but their goal is purely defensive: finding vulnerabilities, assessing risk, and reporting remediation steps.
2. **Black Hat Hackers (Crackers / Cyber Criminals)**:
   - Malicious, unauthorized actors who breach networks for illegal personal gain, financial theft, data extortion, espionage, or malicious destruction.
3. **Grey Hat Hackers**:
   - Act without prior authorization from the target organization, but without explicitly malicious intent. They discover a security flaw and notify the vendor, often publicly disclosing the bug or demanding a bug bounty reward.
4. **Script Kiddies**:
   - Inexperienced attackers who lack deep knowledge of networking, operating systems, or programming. They execute pre-written scripts and automated tools (e.g., LOIC, ready-made Metasploit modules) without understanding how they work.
5. **Hacktivists**:
   - Individuals or collectives motivated by political, religious, environmental, or social agendas (e.g., Anonymous). Their primary tactics include website defacement, sensitive document leaks, and DDoS attacks.
6. **State-Sponsored Actors / Advanced Persistent Threats (APTs)**:
   - Highly skilled, heavily funded teams backed by national intelligence or military agencies. They execute prolonged, stealthy cyber espionage campaigns targeting critical national infrastructure (power grids, defense contractors, government databases).
7. **Insider Threats**:
   - Current or former employees, contractors, or business partners who possess legitimate credentials and inside knowledge of company networks, abusing privileges for sabotage or IP theft.

---

### 2.2 Security Operations Teams: Red, Blue, and Purple
- **Red Team (Offensive Security)**:
  - Simulates a real-world adversary attempting to penetrate an organization's defenses without prior notification to defenders.
  - Performs physical intrusion, phishing, weaponized exploits, lateral movement, and privilege escalation to measure how resilient the organization is.
- **Blue Team (Defensive Security)**:
  - The internal security operations center (SOC) analysts and incident response teams.
  - Manages firewalls, monitors SIEM event logs, analyzes network traffic, patches vulnerabilities, and contains security breaches.
- **Purple Team (Collaborative Model)**:
  - A collaborative structure where Red and Blue teams work together in a real-time feedback loop.
  - The Red Team performs an attack technique step-by-step; the Blue Team verifies if their security logging and EDR alerts fired; if detection failed, the Blue Team tunes its detection rules immediately with Red Team guidance.

---

## 3. Ethical Hacking Methodology & Penetration Testing Lifecycle

### 3.1 The 5-Phase Ethical Hacking Methodology (CEH Framework)

```
 [ 1. Reconnaissance ] ──► [ 2. Scanning ] ──► [ 3. Gaining Access ] ──► [ 4. Maintaining ] ──► [ 5. Reporting ]
```

1. **Phase 1: Reconnaissance (Footprinting)**:
   - Information gathering phase to map the target's public footprint without touching critical internal systems.
   - *Passive Reconnaissance*: Gathering intelligence from publicly available sources without sending direct packets to the target (WHOIS lookups, DNS querying, Google Dorking, OSINT via Shodan/theHarvester). Completely undetectable by target logs.
   - *Active Reconnaissance*: Interacting directly with target systems (ping sweeps, banner querying). Detectable in firewall and IDS logs.
2. **Phase 2: Scanning & Enumeration**:
   - Probing the target network to discover live hosts, open ports, running services, operating system versions, and known vulnerabilities.
   - Tools: Nmap (TCP SYN scan, service detection `-sV`), Nessus / OpenVAS (vulnerability scanning).
3. **Phase 3: Gaining Access (Exploitation)**:
   - Launching exploits against identified vulnerabilities to breach the target perimeter and establish a foothold (shell access).
   - Involves password cracking (Hydra, John the Ripper), exploitation frameworks (Metasploit), web application exploits (SQL injection, XSS), or buffer overflows.
4. **Phase 4: Maintaining Access (Post-Exploitation)**:
   - Establishing persistent control over compromised systems so the tester does not lose access upon system reboot.
   - Involves privilege escalation (horizontal and vertical), installing backdoors/implants, and lateral movement (pivoting through internal subnets).
5. **Phase 5: Covering Tracks & Comprehensive Reporting**:
   - *Covering Tracks*: In a real attack, malicious hackers erase logs (`/var/log/auth.log`, Windows Event Viewer) to evade detection. In an ethical test, the tester safely cleans up all uploaded tools, scripts, and temporary user accounts created during the assessment.
   - *Reporting*: Preparing a formal Penetration Testing Report containing an Executive Summary for management, a technical vulnerability breakdown categorized by CVSS severity, proof-of-concept evidence, and prioritized remediation recommendations.

---

### 3.2 Penetration Testing Assessment Models

| Parameter | Black Box Testing (Zero Knowledge) | Grey Box Testing (Partial Knowledge) | White Box Testing (Full Knowledge) |
| :--- | :--- | :--- | :--- |
| **Tester Knowledge** | **Zero prior knowledge**; tester receives only target domain or IP range. | **Partial knowledge**; tester receives limited credentials or architecture overview. | **Complete knowledge**; tester receives full source code, network diagrams, and configs. |
| **Simulated Adversary**| Realistic external Internet hacker. | Disgruntled insider employee, contractor, or authenticated partner. | Malicious developer, internal administrator, or complete code auditor. |
| **Depth & Focus** | Broad perimeter attack surface; unauthenticated testing. | Internal privilege escalation, API vulnerabilities, business logic bugs. | Exhaustive structural code review, edge-case vulnerability detection. |
| **Time & Cost** | Time-consuming; high effort on reconnaissance. | Balanced effort and high Return on Investment (ROI). | Fast execution per asset; highest thoroughness and depth. |

---

### 3.3 Vulnerability Assessment (VA) vs Penetration Testing (PT)

| Parameter | Vulnerability Assessment (VA) | Penetration Testing (PT) |
| :--- | :--- | :--- |
| **Primary Goal** | Identify, catalog, and rank **all known vulnerabilities** across assets. | Prove **exploitability** and demonstrate real-world business impact. |
| **Scope** | Broad enterprise-wide scan across hundreds of systems. | Narrow, deep, and goal-oriented (e.g., compromise domain controller). |
| **Execution Method** | **Automated** vulnerability scanners (Nessus, Qualys, OpenVAS). | **Manual testing** augmented by specialized tools (Burp Suite, Metasploit). |
| **Exploitation** | **Non-intrusive**: Never actively launches exploits. | **Active exploitation**: Executes payloads to obtain shells and extract proof data. |
| **False Positives** | **High**: Scanners rely on version banners without verifying if a bug is exploitable. | **Near zero**: A vulnerability is reported only if successfully exploited. |
| **Deliverable** | Prioritized list of vulnerabilities sorted by CVSS scores. | Comprehensive narrative report with step-by-step attack chains and remediation. |

---

## 4. The Lockheed Martin Cyber Kill Chain

The Cyber Kill Chain is a 7-stage framework modeling the sequential phases an external adversary must execute to compromise an organization and achieve their objective.  
**Core Principle**: Breaking any single link in the chain stops the attack entirely!

```
 1. Recon ──► 2. Weaponize ──► 3. Deliver ──► 4. Exploit ──► 5. Install ──► 6. C2 ──► 7. Actions
```

### 4.1 Detailed 7-Stage Walkthrough & Defensive Countermeasures

1. **Reconnaissance**:
   - *Adversary*: Researches target using OSINT, LinkedIn, Shodan, harvesting employee emails, mapping IP ranges, and identifying software versions.
   - *Defenses*: Threat intelligence monitoring, security awareness training, minimizing public digital footprints, scanning external-facing assets for exposed ports.
2. **Weaponization**:
   - *Adversary*: Pairs an exploit targeting a known vulnerability with a malicious payload (e.g., coupling the *EternalBlue* exploit with the WannaCry ransomware payload inside an executable package).
   - *Defenses*: Threat hunting, threat intelligence feeds, secure software supply chain validation. (Takes place on attacker infrastructure; difficult to detect directly).
3. **Delivery**:
   - *Adversary*: Transmits the weaponized payload to the target environment via spear-phishing emails with attachments, malicious links, infected USB drives, or open internet ports (e.g., SMB Port 445).
   - *Defenses*: Email gateway spam filters, SPF/DKIM/DMARC authentication, disabling legacy insecure protocols (SMBv1), blocking untrusted attachments (.exe, .scr).
4. **Exploitation**:
   - *Adversary*: The delivered payload triggers the software vulnerability (e.g., executing a buffer overflow in unpatched Windows SMB service `MS17-010`) to execute arbitrary malicious code on the victim system.
   - *Defenses*: Rigorous patch management (promptly applying security updates), Data Execution Prevention (DEP), Address Space Layout Randomization (ASLR), Endpoint Detection and Response (EDR).
5. **Installation**:
   - *Adversary*: Installs a persistent backdoor, rootkit, or implant (e.g., the *DoublePulsar* backdoor) allowing long-term access across reboots.
   - *Defenses*: Host-based Intrusion Detection (HIDS), File Integrity Monitoring (FIM), application whitelisting, limiting administrative privileges.
6. **Command and Control (C2 / C&C)**:
   - *Adversary*: The compromised host opens an outbound communication beacon back to an attacker-controlled server (over HTTP, HTTPS, DNS tunneling, or Tor) to receive commands.
   - *Defenses*: Strict outbound firewall egress filtering, DNS sinkholing, deep packet TLS inspection, network intrusion prevention systems (NIPS).
7. **Actions on Objectives**:
   - *Adversary*: Accomplishes original malicious intent: encrypting files for ransom (WannaCry), exfiltrating confidential intellectual property, stealing customer databases, or destroying systems.
   - *Defenses*: Immutable offline backups, Data Loss Prevention (DLP), network segmentation, Least Privilege access policies.

---

## 5. Legal, Ethical Frameworks & Responsible Disclosure

### 5.1 Cyber Laws in India: The Information Technology Act (IT Act 2000 & 2008 Amendment)
Every ethical hacker operating in India must understand the legal boundary between authorized testing and criminal computer crime under the IT Act 2000:
- **Section 43**: Penalty for unauthorized access, downloading or copying data, introducing computer viruses, damaging computer systems, or disrupting access without permission. Liable to pay civil compensation to affected victims.
- **Section 66 (Computer-Related Offences / Hacking)**:
  - If any person, dishonestly or fraudulently, commits any act referred to in Section 43.
  - *Penalty*: Imprisonment up to **3 years**, or fine up to **5 lakh rupees**, or both.
- **Section 66C (Identity Theft)**: Dishonestly using another person's digital signature, password, or unique identification feature. Penalty: Imprisonment up to 3 years and fine up to 1 lakh rupees.
- **Section 66D (Cheating by Impersonation)**: Cheating by impersonating someone using any computer resource. Penalty: Imprisonment up to 3 years and fine up to 1 lakh rupees.
- **Section 66E (Violation of Privacy)**: Intentionally capturing, publishing, or transmitting images of a private area of any person without consent.
- **Section 66F (Cyber Terrorism)**:
  - Intentionally accessing a computer resource without authorization or exceeding authorization to threaten the unity, integrity, security, or sovereignty of India, or to strike terror in people, or causing damage to critical infrastructure.
  - *Penalty*: **Imprisonment for life**.
- **Section 72 & 72A (Breach of Confidentiality)**: Penalty for disclosure of information in breach of a lawful contract without consent (imprisonment up to 3 years or fine up to 5 lakh rupees).
- **Digital Personal Data Protection Act (DPDP Act 2023)**: Governs personal data processing, requiring explicit consent, strict data fiduciary obligations, and imposing penalties up to 250 crore rupees for data breaches.

#### International Cyber Legislation:
- **United States**: Computer Fraud and Abuse Act (CFAA - 18 U.S.C. § 1030) prohibits accessing a protected computer without authorization or exceeding authorized access.
- **United Kingdom**: Computer Misuse Act 1990 penalizes unauthorized access, unauthorized access with intent to commit serious crimes, and unauthorized acts impairing computer operation.

---

### 5.2 Vulnerability Disclosure Models & Bug Bounties
1. **Responsible Disclosure (Coordinated Vulnerability Disclosure - CVD)**:
   - The ethical security standard.
   - The security researcher privately reports the vulnerability to the affected vendor.
   - The researcher grants the vendor a reasonable remediation window (typically **60 to 90 days**) to develop, test, and release a security patch before making any details public.
2. **Full Disclosure**:
   - The researcher immediately releases all vulnerability details, technical proof-of-concept, and exploit code to the public without giving the vendor prior notice. Leaves users completely defenseless against immediate attack until a patch is developed.
3. **Zero-Day Disclosure (0-Day)**:
   - A vulnerability that is actively exploited or disclosed when no vendor patch exists.
4. **Bug Bounty Programs**:
   - Formalized programs offered by organizations (e.g., HackerOne, Bugcrowd, Google Vulnerability Reward Program) where ethical hackers are invited to test scoped assets under a formal **Safe Harbor** policy, earning financial rewards (bounties) for responsibly disclosed valid vulnerabilities.

---

### 5.3 Vulnerability Management Standards: CVE & CVSS
- **CVE (Common Vulnerabilities and Exposures)**: A standardized, publicly accessible catalog of known cybersecurity vulnerabilities maintained by MITRE (e.g., `CVE-2017-0144` for EternalBlue, `CVE-2021-44228` for Log4Shell).
- **CVSS (Common Vulnerability Scoring System v3.1 / v4.0)**:
  - An open industry standard providing a numerical severity rating from **0.0 to 10.0**.
  - **Metric Groups**:
    1. *Base Metrics*: Intrinsic qualities of a vulnerability (Attack Vector AV, Attack Complexity AC, Privileges Required PR, User Interaction UI, Scope S, and Impact on C, I, A).
    2. *Temporal Metrics*: Factors that change over time (Exploit Code Maturity, Remediation Level).
    3. *Environmental Metrics*: Custom characteristics specific to an organization's internal environment.
  - **CVSS Qualitative Severity Ratings**:
    - **None**: `0.0`
    - **Low**: `0.1 – 3.9`
    - **Medium**: `4.0 – 6.9`
    - **High**: `7.0 – 8.9`
    - **Critical**: `9.0 – 10.0`
