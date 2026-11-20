# EH Unit 1: Introduction to Ethical Hacking & Security Fundamentals
**Course**: Ethical Hacking (MD***) | **VIT Pune** | **Exam Date**: 27 Nov 2026

---

### 1. Essential Definitions & The CIA Triad
* **Vulnerability**: Weakness/flaw in system design, implementation, or configuration (CVE).
* **Threat**: Potential cause of an incident that may result in harm.
* **Exploit**: Code/technique taking advantage of a vulnerability to cause unintended behavior.
* **Risk**: Probability of a threat exploiting a vulnerability × Impact: `Risk = Threat × Vulnerability × Impact`.
* **Confidentiality**: Ensuring data access is restricted to authorized entities (AES, RSA, TLS).
* **Integrity**: Guaranteeing data has not been altered or tampered with (SHA-256, HMAC, digital signatures).
* **Availability**: Ensuring timely, reliable access to assets and services (redundancy, DDoS mitigation, RAID).
* **Non-Repudiation**: Inability of a sender/actor to deny authenticity of their action (Asymmetric PKI, Audit Logs).

---

### 2. Hacker Classifications & Colors
| Type / Hat | Authorization | Motive | Legality |
| :--- | :--- | :--- | :--- |
| **White Hat** | Full written authorization (Scope/RoE) | Secure posture, remediate flaws | 100% Legal |
| **Black Hat** | Zero authorization | Financial gain, espionage, malice | 100% Illegal |
| **Grey Hat** | No prior authorization | Curiosity, bug hunting, fame | Illegal (action without consent) |
| **Suicide Hacker** | Zero authorization | Cause max destruction; unconcerned about jail/death | Illegal |
| **Script Kiddie** | Zero authorization | Bragging, petty trolling (uses pre-built tools) | Illegal |
| **State-Sponsored / APT**| State sanction (hostile foreign nation) | Geopolitical disruption, intelligence theft | Cyberwarfare |

---

### 3. Security Teams (Red, Blue, Purple)
* **Red Team (Offensive)**: Adversarial attack simulations, covert access, lateral movement, physical/social testing.
* **Blue Team (Defensive)**: Continuous monitoring (SOC), SIEM alert triage, incident response, hardening, rule tuning.
* **Purple Team (Integrative)**: Real-time feedback loop between Red and Blue to rapidly tune detection rules against known TTPs.

---

### 4. 5-Phase CEH Ethical Hacking Methodology
$$\text{Reconnaissance} \longrightarrow \text{Scanning} \longrightarrow \text{Gaining Access} \longrightarrow \text{Maintaining Access} \longrightarrow \text{Clearing Tracks / Reporting}$$
1. **Reconnaissance**: Passive (OSINT, WHOIS, DNS) & Active (ping sweep, light port probes). Gather target intel.
2. **Scanning & Enumeration**: Port scanning (Nmap), service/OS detection, vulnerability scanning (Nessus).
3. **Gaining Access**: Exploitation phase (buffer overflows, web injection, default creds, Metasploit).
4. **Maintaining Access**: Persistence via backdoors, rootkits, webshells, cron jobs, new administrator accounts.
5. **Clearing Tracks / Reporting**: Black hat scrubs logs (`utmp`, `wtmp`, audit logs). White hat generates **VAPT Assessment Report** and restores system to clean state.

---

### 5. Lockheed Martin Cyber Kill Chain
1. **Reconnaissance** → 2. **Weaponization** (Exploit + Backdoor payload in file) → 3. **Delivery** (Phishing, web drop) → 4. **Exploitation** (Triggering vulnerability) → 5. **Installation** (Persistence / implant) → 6. **Command & Control (C2)** (Beaconing to external server) → 7. **Actions on Objectives** (Data exfiltration, ransomware encryption).

---

### 6. Indian IT Act 2000 (Key Penal Sections)
* **Section 43**: Penalty for damage to computer system / data tampering (Civil liability, compensation up to ₹1 Crore).
* **Section 66**: Computer-related offences (Hacking with criminal intent; imprisonment up to 3 years or fine up to ₹5 Lakhs).
* **Section 66B**: Dishonestly receiving stolen computer resource/data (Up to 3 years imprisonment).
* **Section 66C**: Identity theft (fraudulent use of electronic signature, password; up to 3 years imprisonment).
* **Section 66D**: Cheating by personation using computer resource (Phishing; up to 3 years imprisonment).
* **Section 66E**: Violation of privacy (Capturing/transmitting private images; up to 3 years imprisonment).
* **Section 66F**: **Cyber Terrorism** (Threatening unity, integrity, sovereignty of India; **Life Imprisonment**).
* **Section 72**: Breach of confidentiality and privacy by authorized personnel (Up to 2 years imprisonment).

---

### 7. VAPT Types & Assessment Models
* **Vulnerability Assessment (VA)**: Automated scanning to discover, list, and categorize known flaws (breadth-first).
* **Penetration Testing (PT)**: Active, authorized exploitation of flaws to measure real-world business impact (depth-first).
* **Black Box**: Zero prior knowledge of target internals (simulates external adversary).
* **White Box**: Complete disclosure of source code, architecture diagrams, credentials (comprehensive audit).
* **Grey Box**: Partial knowledge (e.g., standard low-privilege user account; simulates malicious insider/contractor).

---

### 8. Legal Prerequisites for Ethical Hackers
* **Scope Definition**: Whitelisted IP ranges, domains, exclusion lists (e.g., out-of-scope production databases).
* **Rules of Engagement (RoE)**: Permitted testing hours, banned attack vectors (DoS/DDoS strictly forbidden).
* **Non-Disclosure Agreement (NDA)**: Protecting proprietary corporate assets and discovered 0-day flaws.
* **Get-Out-of-Jail Card**: Formal signed letter of authorization on corporate letterhead carried at all times.
