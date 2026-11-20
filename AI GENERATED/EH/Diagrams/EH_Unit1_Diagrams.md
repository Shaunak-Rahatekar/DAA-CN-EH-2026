# Ethical Hacking (MD***) — Unit I: Introduction to Ethical Hacking Diagrams

This document contains high-resolution reference diagrams, exam visual breakdowns, and hand-drawable ASCII blueprints for fast reproduction during the 2-hour written examination.

---

## Diagram 1: The CIA Triad & Extended Security Attributes

![The CIA Triad](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit1_Diagram1_CIA_Triad.png)

### Hand-Drawn Exam Redraw Guide (ASCII Blueprint)
```
                          [ CONFIDENTIALITY ]
                      Only authorized access to data
                      Threats: Eavesdropping, Snooping
                      Defenses: AES-256 Encryption, RBAC
                                   /    \
                                  /      \
                                 /  SECURE\
                                /   ASSETS \
                               /            \
           [ INTEGRITY ] -------------------- [ AVAILABILITY ]
       Data untampered & accurate        Systems operational when needed
       Threats: MiTM, Tampering, SQLi    Threats: DoS/DDoS, Ransomware
       Defenses: SHA-256 Hashing, MACs   Defenses: Redundancy, RAID, Backups

   Extended Attributes:
   - Authenticity: Verifying claimed identity (MFA, PKI certificates).
   - Non-Repudiation: Proving message origination; sender cannot deny (Digital Signatures).
   - Accountability: Audit trails linking every action to an identity (SIEM logs).
```

### Core Exam Scoring Points:
1. **Confidentiality**: Preserving authorized restrictions on access and disclosure. Example: Encrypting patient records in transit (TLS) and at rest (AES-256).
2. **Integrity**: Guarding against improper information modification or destruction. Example: Verifying software downloads using SHA-256 hash checksums.
3. **Availability**: Ensuring timely and reliable access to and use of information. Example: Configuring dual power supplies, load balancers, and Cloudflare DDoS protection.

---

## Diagram 2: Hacker Taxonomy & Enterprise Security Teams

![Hacker Taxonomy and Teams](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit1_Diagram2_Hacker_Types_Teams.png)

### Hand-Drawn Exam Redraw Guide
```
  [ Hacker Classifications ]                      [ Security Teams Model ]
  +-------------------------------------+      +----------------+    +----------------+
  | White Hat: Authorized, defensive    |      |    RED TEAM    |    |   BLUE TEAM    |
  | Black Hat: Unauthorized, malicious  |      |   (Offensive)  |    |   (Defensive)  |
  | Grey Hat:  Unauthorized, non-malice |      | Penetration    |    | SOC Monitoring |
  | Script Kid:Unskilled tool runner    |      | Exploitation   |    | Incident Resp. |
  | Hacktivist:Politically motivated    |      +-------+--------+    +--------+-------+
  | APT:       Nation-state funded      |              \                     /
  +-------------------------------------+               \                   /
                                                         v                 v
                                                      +-----------------------+
                                                      |      PURPLE TEAM      |
                                                      | Real-time feedback &  |
                                                      | detection tuning loop |
                                                      +-----------------------+
```

### High-Yield Exam Defense:
- **Red Team vs Blue Team vs Purple Team**:
  - **Red Team**: Offensive operators who simulate real-world cyber attacks against the organization to test defense resilience.
  - **Blue Team**: Defensive operators (SOC analysts, incident responders) who monitor logs, detect anomalies, and block intrusion attempts.
  - **Purple Team**: A cooperative, integrated exercise where Red demonstrates attack vectors step-by-step and Blue immediately tunes SIEM/EDR detection rules to verify coverage.

---

## Diagram 3: The 5-Phase Ethical Hacking Methodology

![Ethical Hacking Methodology](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit1_Diagram3_Ethical_Hacking_Methodology.png)

### Hand-Drawn Exam Redraw Guide
```
 [ 1. Reconnaissance ] ──► [ 2. Scanning ] ──► [ 3. Gaining Access ] ──► [ 4. Maintaining ] ──► [ 5. Reporting ]
  - Passive: OSINT,         - Host discovery    - Launch exploits         - Privilege            - Clean tools
    WHOIS, DNS records       (ping sweep)       - Password attacks         Escalation            - Erase traces
  - Active: Port query,     - Port scan (Nmap)  - SQLi / XSS              - Backdoors/C2         - Executive Summary
    Google Dorks            - Vulnerabilities   - Buffer overflow         - Lateral Pivot        - CVSS Remediation
```

### The Legal Distinction (Crucial Exam Point):
- An Ethical Hacker **MUST obtain formal, written authorization (Rules of Engagement - RoE)** signed by system owners before initiating Phase 1.
- Without written consent, any probing or testing constitutes an unauthorized intrusion punishable under **Sections 43 and 66 of the Indian Information Technology Act 2000**.

---

## Diagram 4: Lockheed Martin Cyber Kill Chain & Defense-in-Depth

![Cyber Kill Chain](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit1_Diagram4_Cyber_Kill_Chain.png)

### Hand-Drawn Exam Redraw Guide
```
  Adversary Kill Chain Stage                       Defensive Countermeasure Gate
  +-----------------------------------------+      +-----------------------------------------+
  | 1. Reconnaissance: Harvest target info  | ───► | Threat intel, Shodan scans, info hygiene|
  +-----------------------------------------+      +-----------------------------------------+
  | 2. Weaponization: Pair exploit + malware| ───► | NIDS/NIPS signatures, secure supply chain|
  +-----------------------------------------+      +-----------------------------------------+
  | 3. Delivery: Phishing / Port 445 SMB    | ───► | Spam filters, SPF/DKIM, block SMB ports |
  +-----------------------------------------+      +-----------------------------------------+
  | 4. Exploitation: Trigger software bug   | ───► | Patch management (MS17-010), DEP, ASLR  |
  +-----------------------------------------+      +-----------------------------------------+
  | 5. Installation: Implant backdoor / C2  | ───► | EDR, HIDS, App Whitelisting, Antivirus |
  +-----------------------------------------+      +-----------------------------------------+
  | 6. Command & Control (C2): Beacon home  | ───► | Egress firewall, DNS sinkholing, proxies|
  +-----------------------------------------+      +-----------------------------------------+
  | 7. Actions on Objectives: Encrypt/Exfil | ───► | Offline backups, Least Privilege, DLP   |
  +-----------------------------------------+      +-----------------------------------------+
```

---

## Diagram 5: VAPT Assessment Models & Testing Taxonomy

![VAPT Models](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit1_Diagram5_VAPT_Assessment_Models.png)

### Hand-Drawn Exam Redraw Guide
```
      [ Assessment Testing Models ]                     [ Vulnerability Assessment vs Pen Testing ]
  +---------------------------------------+         +-----------------------+-----------------------+
  | Black Box (Zero Knowledge):           |         | Vulnerability Assess  | Penetration Testing   |
  | Simulates outside external hacker     |         +-----------------------+-----------------------+
  +---------------------------------------+         | Broad asset catalog   | Deep targeted exploit |
  | Grey Box (Partial Knowledge):         |         | Automated (Nessus)    | Manual + tools        |
  | Simulates authenticated insider/user  |         | Non-intrusive (no exp)| Actively exploits bug |
  +---------------------------------------+         | Potential false pos.  | Verified proof of PoC |
  | White Box (Full Knowledge):           |         | Prioritized bug list  | Detailed exec report  |
  | Source code, architecture, audit      |         +-----------------------+-----------------------+
  +---------------------------------------+
```
