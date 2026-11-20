# Ethical Hacking (EH) — Unit II Examination Questions
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit II**: Footprinting, Reconnaissance and Scanning

---

### Section A: 4-Mark Questions (Short Answer / Conceptual Clarity)
*Estimated time: 7-8 minutes per question*

#### Q2.1 [4 Marks]
(a) Differentiate between Passive Reconnaissance and Active Reconnaissance across 4 parameters: Target Contact, Detectability, Tooling, and Legal Risk. **[2 Marks]**  
(b) Explain the technical purpose of WHOIS lookup and DNS Enumeration in building a target profile. List 4 critical DNS resource records ethical hackers inspect. **[2 Marks]**

#### Q2.2 [4 Marks]
Explain the concept of Google Dorking. Provide 4 concrete search operators with realistic attack examples demonstrating how exposed credentials, directory listings, or database backups are uncovered. Discuss why relying on `robots.txt` for security is a dangerous misconception. **[4 Marks]**

---

### Section B: 8-Mark Questions (Analytical / Methodological Depth)
*Estimated time: 15-18 minutes per question*

#### Q2.3 [8 Marks]
(a) Describe the mechanism of a **DNS Zone Transfer (AXFR)**. Explain why an unrestricted zone transfer poses a severe threat to an enterprise. Provide the exact CLI command used to test for this vulnerability and specify two hardening measures in BIND DNS. **[4 Marks]**  
(b) Compare the packet exchange mechanics of **TCP Connect Scan (`-sT`)** versus **TCP SYN Stealth Scan (`-sS`)** using sequence diagrams. Detail why a SYN scan avoids detection in application server access logs, and specify the operating system privilege required for each. **[4 Marks]**

#### Q2.4 [8 Marks]
(a) Analyze the three primary Nmap port states: **Open**, **Closed**, and **Filtered**. For each state, describe the packet received by the scanner when probing with a TCP SYN packet, and explain how a firewall induces the "Filtered" state. **[4 Marks]**  
(b) Explain the working principle of RFC 793 Inverse Scans (FIN `-sF`, Null `-sN`, and Xmas Tree `-sX`). Explain how they determine port states, why they can bypass stateless packet-filtering firewalls, and why they fail completely against Microsoft Windows hosts. **[4 Marks]**

---

### Section C: 10-Mark Questions (Comprehensive / Scenario-Based)
*Estimated time: 22-25 minutes*

#### Q2.5 [10 Marks]
An enterprise security team hires you to conduct external and internal reconnaissance and scanning on their corporate subnet `192.168.10.0/24`.
(a) **Port Scanning & Service Enumeration**: Design a step-by-step Nmap command strategy covering:
1. Fast host discovery while bypassing ping blocking.
2. Full TCP port sweep with service versioning and OS fingerprinting.
3. Vulnerability scanning using the Nmap Scripting Engine (NSE).
4. Explain how banner grabbing is conducted on HTTP (port 80) and SMTP (port 25), and identify two configuration settings to sanitize banners. **[5 Marks]**  
(b) **Vulnerability Assessment Architecture**:
1. Walk through the 5-phase lifecycle of a Vulnerability Assessment (VA).
2. Compare **Credentialed (Authenticated)** vs **Non-Credentialed (Unauthenticated)** vulnerability scans across perspective, data depth, system overhead, and False Positive rates. Name two industry-standard automated vulnerability scanners. **[5 Marks]**
