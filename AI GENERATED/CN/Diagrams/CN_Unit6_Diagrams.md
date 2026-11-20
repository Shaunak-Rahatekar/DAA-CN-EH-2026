# Computer Networks (CS3213) — Unit VI: Application Layer Diagrams

This document contains high-resolution reference diagrams, exam visual breakdowns, and hand-drawable ASCII blueprints for fast reproduction during the 2-hour written examination.

---

## Diagram 1: DNS Hierarchical Tree & Resolution Workflow

![DNS Resolution](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit6_Diagram1_DNS_Resolution.png)

### Hand-Drawn Exam Redraw Guide (Iterative vs Recursive)
```
      Client                    Local DNS                      Root DNS (".")
         |                          |                                 |
         | --- 1. Query (Recur) --> |                                 |
         |     "www.vit.edu"        | --- 2. Query (Iterative) -----> |
         |                          | <--- 3. Referral to .edu ------- |
         |                          |
         |                          |                               TLD DNS (".edu")
         |                          |                                 |
         |                          | --- 4. Query (Iterative) -----> |
         |                          | <--- 5. Referral to vit.edu ---- |
         |                          |
         |                          |                             Authoritative DNS
         |                          |                                 |
         |                          | --- 6. Query (Iterative) -----> |
         |                          | <--- 7. IP Address (A record) -- |
         |                          |
         | <--- 8. Final IP Return - |
         v                          v
```

### Core Exam Scoring Points:
1. **Recursive Query**: Client places full resolution burden on Local DNS. Server *must* return IP address or error.
2. **Iterative Query**: Local DNS queries Root/TLD/Auth servers. Queried server returns the best referral answer it knows ("I don't know the IP, go ask server X").
3. **DNS Record Types**:
   - `A`: Hostname ➔ IPv4 Address
   - `AAAA`: Hostname ➔ 128-bit IPv6 Address
   - `CNAME`: Canonical Name (Alias ➔ Official Name)
   - `MX`: Mail Exchange server hostname + priority
   - `NS`: Name Server authoritative for zone
   - `PTR`: Pointer for reverse DNS (IP ➔ Hostname)

---

## Diagram 2: Evolution of HTTP (1.0 vs 1.1 vs 2.0 vs 3.0)

![HTTP Evolution](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit6_Diagram2_HTTP_Evolution.png)

### Hand-Drawn Exam Redraw Guide
```
  [ HTTP/1.0 ]              [ HTTP/1.1 ]              [ HTTP/2 ]                [ HTTP/3 ]
 (Non-Persistent)          (Persistent)              (Multiplexed)             (QUIC over UDP)

 TCP SYN ➔ ACK             TCP SYN ➔ ACK             TCP SYN ➔ ACK             QUIC 0-RTT / 1-RTT
 GET Obj1 ➔ Data           GET Obj1 ➔ Data           +-----------------------+ +-----------------------+
 TCP Close                 GET Obj2 ➔ Data           | Stream 1 (HTML)       | | Stream 1 (HTML)       |
                           GET Obj3 ➔ Data           | Stream 2 (CSS) Inter- | | Stream 2 (CSS) Indep- |
 (New TCP for Obj2!)                                 | Stream 3 (IMG) leaved | | Stream 3 (IMG) endent |
                                                     +-----------------------+ +-----------------------+
 2 RTTs per object         1 RTT per object          1 TCP connection;         Runs over UDP;
 Overhead: Massive         Flaw: Application HoL     HPACK compression;        Zero HoL blocking;
 Socket exhaustion         blocking!                 Flaw: TCP-level HoL block Connection Migration!
```

---

## Diagram 3: Electronic Mail Architecture (SMTP, MIME, POP3, IMAP)

![Email Architecture](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit6_Diagram3_Email_Architecture.png)

### Hand-Drawn Exam Redraw Guide
```
  Sender Host                  Sender Mail Server            Receiver Mail Server          Receiver Host
 +-----------+                +------------------+          +--------------------+        +---------------+
 |  Sender   | -- SMTP Push ->| Sender MTA /     |-- SMTP ->| Receiver MTA /     |<- Pull-| Receiver      |
 |  MUA      |   (Port 587)   | Spool Queue      | (Port 25)| Mailbox Storage    |(POP3/  | MUA           |
 +-----------+                +------------------+          +--------------------+ IMAP)  +---------------+
```

### Essential Exam Distinctions:
- **SMTP**: Push protocol; transfers mail from sender MUA ➔ sender MTA, and sender MTA ➔ receiver MTA. Plain 7-bit ASCII text only.
- **MIME**: Encodes binary attachments (PDF, images) into 7-bit ASCII text via Base64 encoding.
- **POP3 (Port 110)**: Pull protocol; downloads and deletes email from server by default. Single-device oriented.
- **IMAP (Port 143)**: Pull protocol; keeps emails permanently stored on the server; synchronizes read/unread flags and folders across multiple devices.

---

## Diagram 4: FTP Dual-Channel Architecture (Active vs Passive)

![FTP Architecture](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit6_Diagram4_FTP_DualConnection.png)

### Hand-Drawn Exam Redraw Guide
```
      [ FTP Active Mode ]                                   [ FTP Passive Mode (PASV) ]

    Client                  Server                        Client                  Server
      |                       |                             |                       |
      | -- Port N ➔ Port 21 ->| Control (Session)           | -- Port N ➔ Port 21 ->| Control (Session)
      | -- PORT N+1 --------->|                             | -- PASV ------------->|
      |                       |                             | <– Port P Info -------|
      | <- Port 20 ➔ Port N+1-| Data (Server Initiates!)    | -- Port N+1 ➔ Port P->| Data (Client Initiates!)
      |    (BLOCKED BY        |                             |    (PERMITTED BY      |
      |     CLIENT FIREWALL!) |                             |     CLIENT FIREWALL!) |
      v                       v                             v                       v
```

### Key Differences:
- **Control Connection**: Runs on Port 21 for user authentication, directory browsing, and commands. Remains open throughout the entire session.
- **Data Connection**: Created dynamically for each file transfer; closes on EOF.
  - **Active Mode**: Server initiates data connection from Port 20 to Client Port N+1 (fails under NAT/Firewall).
  - **Passive Mode (`PASV`)**: Client initiates data connection to random server Ephemeral Port P (firewall-friendly).

---

## Diagram 5: DHCP DORA Process & Lease Renewal Timers

![DHCP DORA Process](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit6_Diagram5_DHCP_DORA_Process.png)

### Hand-Drawn Exam Redraw Guide
```
      DHCP Client (0.0.0.0 : 68)                         DHCP Server (IP : 67)
                  |                                                 |
                  | ------- 1. DHCP DISCOVER (Broadcast) ---------> |
                  |         Dst: 255.255.255.255, Src: 0.0.0.0      |
                  |                                                 |
                  | <------ 2. DHCP OFFER (Offered IP, Mask, GW) --- |
                  |                                                 |
                  | ------- 3. DHCP REQUEST (Broadcast acceptance)-> |
                  |                                                 |
                  | <------ 4. DHCP ACK (Lease Finalized) --------- |
                  v                                                 v
```

### Lease Timers:
- **T1 Timer (50% of Lease Duration)**: Client attempts unicast lease renewal to the original DHCP server.
- **T2 Timer (87.5% of Lease Duration)**: If original server fails to answer, client broadcasts lease renewal to *any* DHCP server.
- **Lease Expiration (100%)**: If unrenewed, client releases IP immediately and reverts to 0.0.0.0.
