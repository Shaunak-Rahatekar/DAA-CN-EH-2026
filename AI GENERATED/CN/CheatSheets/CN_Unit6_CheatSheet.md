# CN Unit VI: Application Layer — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: DNS Hierarchy & Resolution, HTTP Evolution (1.0/1.1/2.0/3.0), REST vs SOAP, Email (SMTP, MIME, POP3, IMAP), FTP Dual Connections, and DHCP DORA Process

---

### 1. DNS Architecture & Record Types
- **Hierarchy**: Root Servers (`"."`, 13 logical Anycast clusters) ➔ TLDs (`.com`, `.edu`, `.in`) ➔ Authoritative Servers ➔ Local DNS (ISP Resolver).
- **Resolution Modes**:
  - **Recursive Query**: Client ➔ Local DNS (server *must* return IP or error).
  - **Iterative Query**: Local DNS ➔ Root/TLD/Auth (server returns best referral answer).
- **Core Record Types**: `A` (Hostname ➔ IPv4), `AAAA` (Hostname ➔ IPv6), `CNAME` (Alias ➔ Canonical Name), `MX` (Mail server + priority), `NS` (Name server for zone), `PTR` (Reverse IP ➔ Hostname).
- **Port**: UDP Port 53 (< 512B queries); TCP Port 53 (> 512B, DNSSEC, Zone Transfers).

---

### 2. HTTP Evolution Quick Matrix

| Parameter | HTTP/1.0 | HTTP/1.1 | HTTP/2 | HTTP/3 |
| :--- | :--- | :--- | :--- | :--- |
| **Transport** | TCP | TCP | TCP | **QUIC (UDP)** |
| **Connection Model**| Non-Persistent (1 per obj)| Persistent (`keep-alive`) | Multiplexed (1 per origin) | Multiplexed (1 per origin) |
| **Framing** | Plain text ASCII | Plain text ASCII | **Binary Framing Layer** | **Binary (QUIC streams)** |
| **HoL Blocking** | Full connection HoL | **App-level HoL blocking** | **TCP-level HoL blocking** | **Zero HoL blocking** |
| **Header Handling** | Plain text repeated | Plain text repeated | **HPACK compression** | **QPACK compression** |
| **Handshake RTTs** | 2 RTTs per object | 1 RTT per subsequent object| 1 RTT per subsequent object| **0-RTT to 1-RTT** (TLS 1.3) |

---

### 3. Electronic Mail: SMTP, MIME, POP3, IMAP
- **SMTP (RFC 5321)**: Push protocol over TCP Port 25 (server relay) or Port 587 (client submission with STARTTLS). Strictly **7-bit ASCII text only**.
  - Commands: `HELO/EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`, `QUIT`.
- **MIME (RFC 2045)**: Extends SMTP for binary files/multimedia. Base64 encodes 3 binary bytes (24 bits) into 4 printable ASCII characters (values 0-63). Increases size by **33.3%**.
- **POP3 (Port 110 / 995 SSL)**: Pull protocol. Downloads and deletes from server by default; offline-centric; fails multi-device synchronization.
- **IMAP4 (Port 143 / 993 SSL)**: Pull protocol. Server-centric storage; folder and read/unread flags synchronized across multiple devices; partial message retrieval.

---

### 4. File Transfer Protocol (FTP): Dual Connection Architecture
- **Control Connection (TCP Port 21)**: Admin commands (`USER`, `PASS`, `LIST`, `RETR`, `STOR`, `QUIT`). Stays open throughout entire session.
- **Data Connection**: Dynamically created per file transfer; closes on EOF.
  - **Active Mode**: Server initiates connection from Port 20 to Client Port N+1 (**blocked by client firewalls/NAT!**).
  - **Passive Mode (`PASV`)**: Client initiates data connection from Port N+1 to Server Ephemeral Port P (**100% firewall friendly!**).

---

### 5. DHCP DORA Process & Lease Renewal Timers
- **UDP Ports**: Server = **Port 67**, Client = **Port 68**.
- **The DORA Sequence**:
  1. **Discover (D)**: Client broadcasts `Src: 0.0.0.0:68 ➔ Dst: 255.255.255.255:67`.
  2. **Offer (O)**: Servers offer IP, mask, gateway, DNS, lease duration.
  3. **Request (R)**: Client broadcasts acceptance of chosen server's offer (releases others).
  4. **Acknowledge (A)**: Chosen server confirms lease binding.
- **Lease Renewal Timers**:
  - **T1 (50% of lease)**: Client sends unicast `DHCPREQUEST` to original server.
  - **T2 (87.5% of lease)**: If T1 fails, client broadcasts `DHCPREQUEST` to any server.
  - **Expiration (100%)**: Release IP immediately; restart DORA.
  - **DHCP Relay Agent**: Intercepts broadcast and unicasts across router boundaries.
