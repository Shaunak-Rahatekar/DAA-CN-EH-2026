# Ethical Hacking (EH) — Unit III Exam Diagrams & Visual Redraw Guides
**Course**: Ethical Hacking (MD***) | **VIT Pune**  
**Unit III**: System Hacking and Network Security

---

## Diagram 1: Password Attack Taxonomy & Hash Cracking Pipeline

### Standalone Image Reference
![Password Attack Taxonomy](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit3_Diagram1_Password_Attack_Taxonomy.png)

### ASCII Redraw Sketch (For 30-Second Exam Answer Sheet Reproduction)
```
+---------------------------------------------------------------------------------+
|                        PASSWORD ATTACK TAXONOMY & FLOW                          |
+-----------------------------------+---------------------------------------------+
| ONLINE ATTACKS (Live Network)     | OFFLINE ATTACKS (Stolen Hashes)             |
| * Brute Force / Dictionary        | * Target: /etc/shadow, SAM Database         |
| * Password Spraying (1 pass->many)| * Advantage: No lockout, GPU speed          |
| * Credential Stuffing             | * Tools: John the Ripper (CPU), Hashcat(GPU)|
+-----------------------------------+---------------------------------------------+
| CRACKING PIPELINE:                                                              |
| [Candidate Word] + [Stolen Salt] ──> [Slow Hash: bcrypt / Argon2]               |
|                                                     │                           |
|                                                     ▼                           |
| [Stolen Target Hash] ────────── Comparator ─────────┤                           |
|                                      │                                          |
|                          Match? ─────┴─────> YES: Password Cracked!             |
|                                      └─────> NO: Next Candidate                 |
+---------------------------------------------------------------------------------+
```

### Mermaid Diagram
```mermaid
flowchart TD
    subgraph Attacks ["Password Attack Taxonomy"]
        P1["Online Attacks"] --> D1["Dictionary Attack"]
        P1 --> D2["Password Spraying (Bypasses Lockout)"]
        P1 --> D3["Credential Stuffing"]
        P2["Offline Attacks"] --> O1["Hash Cracking (/etc/shadow)"]
        O1 --> O2["John the Ripper / Hashcat GPU"]
    end

    subgraph Pipeline ["Offline Hash Cracking Pipeline"]
        W["Candidate: 'secret123'"] --> H["Hashing Engine\nHash(Candidate + Salt)"]
        S["Stolen Salt from Shadow"] --> H
        H --> C{"Compare Digest with\nTarget Stored Hash"}
        C -->|Match| SUCCESS["SUCCESS: Plaintext Recovered!"]
        C -->|Mismatch| NEXT["Try Next Word / Permutation"]
    end
```

### 30-Second Exam Redraw Guide
* Draw a 2-column box: Left = **Online** (Password Spraying, Credential Stuffing, lockout risk); Right = **Offline** (Stolen hashes, John/Hashcat, no lockout).
* Underneath, draw the comparator: `Candidate` + `Salt` $\rightarrow$ `Hash Engine` $\rightarrow$ Match box comparing with `Stored Hash` $\rightarrow$ Yes/No.

---

## Diagram 2: Vertical vs Horizontal Privilege Escalation

### Standalone Image Reference
![Privilege Escalation Architectures](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit3_Diagram2_Privilege_Escalation.png)

### ASCII Redraw Sketch
```
+------------------------------------+    +------------------------------------+
|   VERTICAL PRIVILEGE ESCALATION    |    |  HORIZONTAL PRIVILEGE ESCALATION   |
|     (Privilege Level Elevation)    |    |      (Lateral Identity Shift)      |
+------------------------------------+    +------------------------------------+
|                                    |    |                                    |
|   [ ROOT / ADMINISTRATOR ]         |    |   [ USER A ] ──── IDOR ────> [ USER B ]
|   (Full Kernel / SYSTEM Rights)    |    |   (Attacker)                 (Victim)  |
|                 ▲                  |    |   ID: #10052                 ID: #10053|
|                 │ EXPLOITATION:    |    |                                    |
|                 │ * Kernel Bug     |    |   * Same Privilege Level           |
|                 │ * SUID Escapes   |    |   * Broken Object Level Auth       |
|                 │ * Sudoers Flaw   |    |   * Direct Object Reference Tamper |
|   [ STANDARD USER (www-data) ]     |    |                                    |
+------------------------------------+    +------------------------------------+
```

### Mermaid Diagram
```mermaid
flowchart TD
    subgraph Vertical ["Vertical Privilege Escalation"]
        U1["Standard User\n(Low Privilege: uid=1001)"] -->|"Exploit Kernel (Dirty COW)\nOr SUID Shell Escape"| ROOT["Root / Administrator\n(Full System Control: uid=0)"]
    end

    subgraph Horizontal ["Horizontal Privilege Escalation"]
        UA["User A (Customer)\nAccount #10052"] -->|"Tamper Parameter in URL:\nGET /statement?id=10053"| UB["User B (Customer)\nAccount #10053 (Victim Data Exposed!)"]
    end
```

### 30-Second Exam Redraw Guide
* On the left, draw an upward vertical arrow from a box `Standard User` to `Root/Admin`. Label the arrow: `Kernel exploits, SUID binaries, Insecure sudo`.
* On the right, draw a horizontal arrow between two boxes at the same level: `User A (#101)` $\rightarrow$ `User B (#102)`. Label: `IDOR / Broken Object Level Auth`.

---

## Diagram 3: ARP Spoofing & Man-in-the-Middle (MITM) Flow

### Standalone Image Reference
![ARP Spoofing MITM](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit3_Diagram3_ARP_Spoofing_MITM.png)

### ASCII Redraw Sketch
```
                      +-----------------------------+
                      |   ATTACKER (192.168.1.50)   |
                      |     MAC: CC:CC:CC:CC:CC:CC  |
                      +-----------------------------+
                        ▲                         ▲
       Forged ARP Reply |                         | Forged ARP Reply
  "192.168.1.1 is at CC"|                         | "192.168.1.10 is at CC"
                        ▼                         ▼
+-----------------------------+             +-----------------------------+
|    VICTIM (192.168.1.10)    |             |   GATEWAY (192.168.1.1)     |
|     MAC: AA:AA:AA:AA:AA:AA  |             |     MAC: BB:BB:BB:BB:BB:BB  |
+-----------------------------+             +-----------------------------+
| Poisoned Cache:             |             | Poisoned Cache:             |
| 192.168.1.1 -> CC:CC:CC     |             | 192.168.1.10 -> CC:CC:CC    |
+-----------------------------+             +-----------------------------+
      (Outbound Traffic) ────► [Attacker] ────► (Forwarded to Gateway)
```

### Mermaid Diagram
```mermaid
sequenceDiagram
    autonumber
    actor V as Victim (192.168.1.10, MAC: AA)
    actor A as Attacker (192.168.1.50, MAC: CC)
    actor G as Gateway Router (192.168.1.1, MAC: BB)

    Note over A,V: 1. Poisoning Victim's ARP Table
    A->>V: Gratuitous ARP: "192.168.1.1 is at MAC CC"
    Note over V: Victim caches: 192.168.1.1 -> CC

    Note over A,G: 2. Poisoning Gateway's ARP Table
    A->>G: Gratuitous ARP: "192.168.1.10 is at MAC CC"
    Note over G: Gateway caches: 192.168.1.10 -> CC

    Note over V,G: 3. Intercepted MITM Traffic Flow
    V->>A: Internet Request Frame sent to MAC CC
    A->>A: Sniffs / Modifies Packet (Wireshark)
    A->>G: Forwards packet to genuine Gateway MAC BB
```

### 30-Second Exam Redraw Guide
* Draw a triangle with `Attacker` at top, `Victim` at bottom-left, and `Gateway` at bottom-right.
* Draw dashed arrows from `Attacker` to both Victim and Gateway labeled: `Forged Gratuitous ARP Reply`.
* Show Victim's ARP table mapping the gateway IP to the attacker's MAC address (`CC:CC:CC`).
* Write the primary switch defense below: `Dynamic ARP Inspection (DAI) + DHCP Snooping`.

---

## Diagram 4: Firewall Generations & DMZ Architecture

### Standalone Image Reference
![Firewall Generations and DMZ](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit3_Diagram4_Firewall_Architectures.png)

### ASCII Redraw Sketch
```
                                [ INTERNET ]
                                     │
                                     ▼
                            [ External Firewall ]
                                     │
                ┌────────────────────┴────────────────────┐
                ▼                                         ▼
        [ DMZ Subnet ]                         [ Internal Firewall ]
   (Web, Mail, Public DNS)                                │
   * Accessible to Internet                               ▼
   * No access to internal DB                   [ Trusted Corporate LAN ]
                                                (Workstations, DBs, AD)
```

### Mermaid Diagram
```mermaid
flowchart TD
    INET["Internet (Untrusted Zone)"] --> FW1["External Firewall\n(Blocks Unsolicited Traffic)"]
    FW1 --> DMZ["DMZ Subnet\n(Web Server, Mail Server, Public DNS)"]
    FW1 --> FW2["Internal Firewall\n(Strict Filtering & Least Privilege)"]
    FW2 --> LAN["Internal Corporate LAN\n(Workstations, Database Servers, Active Directory)"]
    DMZ -.->|"STRICTLY BLOCKED: Inbound Access to LAN"| LAN
```

### 30-Second Exam Redraw Guide
* Draw `Internet` $\rightarrow$ `External Firewall`.
* Branch down into two zones: Left = `DMZ Subnet` (Web/Mail/DNS servers); Right = `Internal Firewall` $\rightarrow$ `Internal LAN` (Workstations, Databases).
* Put an `X` on any direct line from DMZ into the Internal LAN to show network isolation.

---

## Diagram 5: IDS vs IPS Deployment & Detection Paradigms

### Standalone Image Reference
![IDS vs IPS Deployment](file:///D:/2026%20EXAM/AI%20GENERATED/EH/Diagrams/EH_Unit3_Diagram5_IDS_vs_IPS_Deployment.png)

### ASCII Redraw Sketch
```
1. IDS (Passive, Out-of-Band)               2. IPS (Active, In-Line)
                                            
   [Router] ────── Live Traffic ────> [Switch]  [Router] ──> [ INLINE IPS ] ──> [Switch]
                                         │                       │
                                   SPAN /│ (Mirror Copy)         ▼ (Threat Detected!)
                                   TAP   ▼                  DROPS PACKET &
                                   [ PASSIVE IDS ]          RESETS TCP (RST)
                                         │
                                         ▼
                                   [ SIEM Alert ]
                                   (Zero packet drops!)
```

### Mermaid Diagram
```mermaid
flowchart LR
    subgraph IDS ["1. Intrusion Detection System (Passive)"]
        R1["Router"] -->|"Live Traffic"| SW1["Switch"]
        SW1 -.->|"SPAN / Mirror Port Copy"| IDS_BOX["Passive IDS\n(Snort / Zeek)"]
        IDS_BOX -->|"Alert Triggered"| SIEM["SIEM / SOC Console\n(Traffic is NOT interrupted)"]
    end

    subgraph IPS ["2. Intrusion Prevention System (Inline)"]
        R2["Router"] -->|"Physical Wire"| IPS_BOX["Inline IPS"]
        IPS_BOX -->|"Clean Traffic"| SW2["Internal Switch"]
        IPS_BOX -.->|"Malicious Signature Detected"| DROP["PACKET DROPPED!\nSends TCP RST"]
    end
```

### 30-Second Exam Redraw Guide
* Draw side-by-side sketches:
  * Left: `Router` $\rightarrow$ `Switch`. A dashed arrow branches down from `Switch (SPAN port)` to `IDS` $\rightarrow$ `Alert to SIEM`. Note: "Out-of-band, zero latency".
  * Right: `Router` $\rightarrow$ `Inline IPS` $\rightarrow$ `Switch`. Note: "In-line physical path, drops packets, sends TCP RST".
