# Ethical Hacking (EH) — Unit IV Examination Questions
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit IV**: Web Application and Wireless Security

---

### Section A: 4-Mark Questions (Short Answer / Conceptual Clarity)
*Estimated time: 7-8 minutes per question*

#### Q4.1 [4 Marks]
(a) Differentiate between Reflected XSS, Stored XSS, and DOM-Based XSS based on payload persistence, execution point, and server visibility. **[2 Marks]**  
(b) Explain the mechanics of a Cross-Site Request Forgery (CSRF) attack. How do Anti-CSRF Synchronizer Tokens and the `SameSite=Strict` cookie attribute defend against it? **[2 Marks]**

#### Q4.2 [4 Marks]
(a) Define Directory Traversal and distinguish between Local File Inclusion (LFI) and Remote File Inclusion (RFI) with practical PHP code examples. **[2 Marks]**  
(b) Explain the security functions of the following HTTP response headers: **Content-Security-Policy (CSP)**, **Strict-Transport-Security (HSTS)**, and **X-Frame-Options**. **[2 Marks]**

---

### Section B: 8-Mark Questions (Analytical / Methodological Depth)
*Estimated time: 15-18 minutes per question*

#### Q4.3 [8 Marks]
(a) Walk through the technical mechanics of **SQL Injection (SQLi)**:
1. Explain how a Tautology attack (`' OR '1'='1' --`) causes an authentication bypass.
2. Explain the step-by-step process of a **UNION-based SQL injection attack** to extract columns from a database table.
3. Compare Boolean-Based Blind SQLi with Time-Based Blind SQLi. **[5 Marks]**  
(b) Explain why input sanitization alone is insufficient to prevent SQL injection. Demonstrate how **Parameterized Prepared Statements** structurally eliminate SQL injection using code snippets in Python or Java. **[3 Marks]**

#### Q4.4 [8 Marks]
(a) Compare the four generations of 802.11 wireless security protocols: **WEP**, **WPA**, **WPA2**, and **WPA3** across encryption cipher, integrity mechanism, authentication model, and known vulnerabilities in a structured comparison table. **[4 Marks]**  
(b) Describe the **WPA2 4-Way Handshake** with a sequence diagram. Explain how an attacker utilizes a **Deauthentication Attack** to capture the handshake and execute an offline dictionary attack. How does **WPA3 Simultaneous Authentication of Equals (SAE)** resolve this flaw? **[4 Marks]**

---

### Section C: 10-Mark Questions (Comprehensive / Scenario-Based)
*Estimated time: 22-25 minutes*

#### Q4.5 [10 Marks]
You are hired as a Lead Security Consultant to perform a web application and wireless penetration test for a financial technology enterprise.
(a) **Web Application Security Architecture**:
1. Map out the **OWASP Top 10** vulnerabilities (2025/2021) focusing on **A01: Broken Access Control**, **A03: Injection**, and **A07: Identification and Authentication Failures**. For each category, describe the technical root cause and provide an enterprise mitigation control. **[4 Marks]**
2. Detail how an attacker chains a Cross-Site Scripting (XSS) vulnerability to steal session cookies, and explain how the `HttpOnly` flag combined with a strict Content Security Policy (CSP) neutralizes this attack chain. **[2 Marks]**  
(b) **Wireless Enterprise Threat Modeling**:
1. Walk through the execution of an **Evil Twin Access Point Attack** against corporate laptops. Describe how the attacker clones the network, forces clients to disconnect from the genuine AP, and harvests domain credentials via a captive portal. **[2.5 Marks]**
2. Design a comprehensive **Wireless Defense and Hardening Checklist** for an enterprise campus covering authentication (802.1X EAP-TLS), Protected Management Frames (PMF), rogue AP containment (WIPS), and guest network isolation. **[1.5 Marks]**
