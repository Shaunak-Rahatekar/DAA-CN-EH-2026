# Computer Networks (CS3213) — Unit VI: Application Layer Model Answers & Marking Scheme

**Target Exam**: End-Semester Written Examination  
**Scope**: Model Solutions with Examiner Marking Rubric for 4M, 8M, and 10M Questions

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. Explain DNS Resolution for `www.vit.edu`. Distinguish Recursive vs Iterative Queries.
**Model Answer**:

#### Part 1: Step-by-Step Resolution Trace & Diagram (2 Marks)
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
         |                          | <--- 7. IP: 115.240.10.2 ------- |
         |                          |
         | <--- 8. Final IP Return - |
         v                          v
```

#### Part 2: Recursive vs Iterative Comparison (2 Marks)
- **Recursive Query (Client ➔ Local DNS)**:
  - The querying party offloads the entire burden of name resolution to the queried server.
  - The queried server *must* either return the definitive IP address or an error message (it is not allowed to return a referral).
  - Used between end-user clients and their configured Local DNS Server (ISP/Resolver). *(1 Mark)*
- **Iterative Query (Local DNS ➔ Root / TLD / Authoritative)**:
  - The queried server returns the best direct answer it has in its local zone; if it does not know the IP address, it returns a **referral pointer** to the next authoritative server down the tree ("I don't know, go ask server X").
  - Used by Local DNS servers when traversing the global hierarchy, preventing central Root servers from being overwhelmed with recursive state tracking. *(1 Mark)*

**Marking Scheme**:
- 2 Marks for resolution trace diagram and 8-step chronological explanation.
- 1 Mark for Recursive query definition and usage.
- 1 Mark for Iterative query definition and usage = **Total 4 Marks**.

---

### Q2. Differentiate between HTTP/1.0 and HTTP/1.1.
**Model Answer**:

| Parameter | HTTP/1.0 (RFC 1945) | HTTP/1.1 (RFC 2616 / 7230) |
| :--- | :--- | :--- |
| **1. Connection Persistence** | **Non-Persistent by default**: Closes TCP connection immediately after delivering one object. | **Persistent by default**: Keeps TCP connection open (`Connection: keep-alive`) for multiple consecutive requests. |
| **2. Round-Trip Latency** | **2 RTTs per object**: 1 RTT for TCP 3-way handshake + 1 RTT for HTTP GET/response. Delivering 10 images takes 20 RTTs! | **1 RTT per subsequent object**: TCP connection stays alive; transfers occur at 1 RTT per object without repeating handshakes. |
| **3. Pipelining Support** | **No pipelining**: Client must wait for the response of Object 1 before sending request for Object 2. | **Pipelining supported**: Client can send multiple GET requests back-to-back without waiting for earlier replies. |
| **4. Virtual Hosting** | No `Host` header required; server cannot host multiple domain names on a single IP address. | **`Host:` header is strictly mandatory**; enables multiple virtual domains (e.g., `siteA.com` and `siteB.com`) on one single server IP. |

**Marking Scheme**:
- 1 Mark per correctly contrasted parameter across all 4 parameters = **Total 4 Marks**.

---

### Q3. Architecture of Electronic Mail: SMTP, POP3, and IMAP.
**Model Answer**:
1. **Email Architecture Components**:
   - **Mail User Agent (MUA)**: Client software used to compose and read email (Outlook, Thunderbird).
   - **Mail Transfer Agent (MTA)**: Server daemon that routes and relays email across networks using SMTP.
   - **Message Access Agent (MAA)**: Server daemon allowing users to retrieve stored email from their mailbox using POP3 or IMAP. *(1 Mark)*
2. **Comparison Matrix**:

| Feature | SMTP (Simple Mail Transfer Protocol) | POP3 (Post Office Protocol v3) | IMAP4 (Internet Message Access Protocol v4) |
| :--- | :--- | :--- | :--- |
| **Protocol Type** | **PUSH Protocol**: Pushes email from MUA to MTA and between MTAs. | **PULL Protocol**: Pulls stored email from mail server to client MUA. | **PULL Protocol**: Pulls email and manages mailbox on mail server. |
| **Default Ports** | **Port 25** (Server relay), **Port 587** (Client submission + TLS). | **Port 110** (Plaintext), **Port 995** (SSL/TLS). | **Port 143** (Plaintext), **Port 993** (SSL/TLS). |
| **Storage Model** | Spool queue in transit. | **Client-centric**: Downloads and deletes from server by default. | **Server-centric**: Emails stay permanently stored on the server. |
| **Multi-Device Sync**| N/A | **Fails completely**: Mail downloaded on a PC disappears from mobile phone. | **Seamless synchronization**: Read/unread flags, sent mail, and folders sync across all devices. |

**Marking Scheme**:
- 1 Mark for Email components (MUA, MTA, MAA).
- 3 Marks for contrasting SMTP (Push, 25/587), POP3 (Pull, 110/995, client-centric), and IMAP (Pull, 143/993, server sync) = **Total 4 Marks**.

---

### Q4. MIME and the Base64 Encoding Algorithm.
**Model Answer**:
1. **What is MIME (Multipurpose Internet Mail Extensions)**:
   - SMTP is historically restricted to transporting **7-bit ASCII text only**. It cannot natively transmit binary files (images, audio, executables, PDFs) or non-English alphabets.
   - MIME defines structured header fields (`MIME-Version`, `Content-Type`, `Content-Transfer-Encoding`) and encoding rules that transform 8-bit binary data into safe, printable 7-bit ASCII text that travels seamlessly through SMTP. *(1.5 Marks)*
2. **Base64 Encoding Algorithm**:
   - Takes **3 bytes (24 bits)** of binary input data.
   - Splits the 24 bits into **four 6-bit chunks** (since 2⁶ = 64 distinct values, ranging from 0 to 63).
   - Maps each 6-bit chunk to a printable ASCII character using the 64-character alphabet:  
     `A-Z (0-25), a-z (26-51), 0-9 (52-61), + (62), / (63)`.
   - **Padding**: If the input bytes are not a multiple of 3:
     - 1 byte leftover (8 bits) ➔ 2 Base64 characters + `==` padding.
     - 2 bytes leftover (16 bits) ➔ 3 Base64 characters + `=` padding. *(1.5 Marks)*
3. **Percentage Size Overhead**:
   - Every 3 bytes of raw binary data expand into 4 bytes of ASCII text:  
     `Overhead = (4 - 3) / 3 = 1 / 3 = 33.33% size expansion`! *(1 Mark)*

**Marking Scheme**:
- 1.5 Marks for MIME purpose and 7-bit ASCII limitation.
- 1.5 Marks for Base64 3-to-4 byte grouping algorithm and padding rules.
- 1 Mark for exact 33.33% overhead calculation = **Total 4 Marks**.

---

### Q5. FTP Dual-Connection Architecture: Active vs Passive Mode.
**Model Answer**:
1. **Why FTP Uses Two Separate Connections (Out-of-Band Signaling)**:
   - FTP separates administrative control from data payload transfer.
   - **Control Connection (TCP Port 21)**: Transmits user credentials (`USER`, `PASS`), navigation commands (`CWD`, `PWD`), and transfer requests (`RETR`, `STOR`). Stays open continuously throughout the entire session.
   - **Data Connection**: Created dynamically whenever a file or directory listing is transferred, and closed immediately upon completion (EOF). Prevents command interpretation delays during large file transfers. *(1.5 Marks)*
2. **Active Mode vs Passive Mode (`PASV`)**:
   - **Active Mode**:
     - Client opens control connection from ephemeral port `N` to server Port `21`.
     - Client sends command `PORT ClientIP, N+1`.
     - **The Server initiates an inbound data connection from Port 20 to Client Port `N+1`**.
     - **Why it fails behind firewalls/NAT**: Modern firewalls and NAT routers drop all unsolicited incoming connection attempts from the Internet. The server's attempt to connect to client port `N+1` is blocked, breaking active FTP! *(1.25 Marks)*
   - **Passive Mode (`PASV`)**:
     - Client sends command `PASV` over the control connection.
     - Server opens an unprivileged ephemeral port `P` (e.g., `51234`) and tells the client.
     - **The Client initiates the outbound data connection from Port `N+1` to Server Port `P`**.
     - **Why it succeeds**: Client-side firewalls permit outbound traffic, enabling seamless traversal without firewall interference. *(1.25 Marks)*

**Marking Scheme**:
- 1.5 Marks for dual-connection (out-of-band) explanation.
- 1.25 Marks for Active mode mechanics and firewall failure cause.
- 1.25 Marks for Passive mode (`PASV`) and firewall resolution = **Total 4 Marks**.

---

### Q6. The DHCP DORA Process and Broadcast Justification.
**Model Answer**:
1. **The Four Chronological DORA Steps**:
   - **D — DHCP DISCOVER**: Client broadcasts `Src: 0.0.0.0:68 ➔ Dst: 255.255.255.255:67` containing its MAC address and Transaction ID to locate available DHCP servers.
   - **O — DHCP OFFER**: Servers with available pools reserve an IP and broadcast/unicast `DHCPOFFER` with offered IP, subnet mask, gateway, DNS, and lease time.
   - **R — DHCP REQUEST**: Client selects one offer and broadcasts `DHCPREQUEST` to `255.255.255.255` containing the Server Identifier of the chosen server.
   - **A — DHCP ACK**: The selected server responds with `DHCPACK`, finalizing the lease binding. Client configures its network interface. *(2.5 Marks)*
2. **Why the `DHCPREQUEST` Packet is Broadcast**:
   - In a network with **multiple redundant DHCP servers**, all servers generated and sent a `DHCPOFFER`, temporarily reserving distinct IP addresses in their local pools.
   - If the client sent a *unicast* request to only Server 1, Server 2 would never know whether its offer was accepted or declined! Server 2 would hold its reserved IP in limbo until lease timeout.
   - By **broadcasting** the `DHCPREQUEST` to `255.255.255.255`, the client achieves two goals simultaneously:
     1. It explicitly informs Server 1 that its offer is accepted.
     2. It implicitly notifies Server 2, Server 3, etc., that their offers were declined, allowing them to immediately release their reserved IP addresses back into their available pools. *(1.5 Marks)*

**Marking Scheme**:
- 2.5 Marks for DORA 4-step walkthrough with ports and packet types.
- 1.5 Marks for broadcast justification (multiple server notification and unreserved pool release) = **Total 4 Marks**.

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. Exhaustive Comparative Analysis: HTTP/1.1 vs HTTP/2 vs HTTP/3
**Model Answer**:

| Parameter | HTTP/1.1 (RFC 7230) | HTTP/2 (RFC 7540) | HTTP/3 (RFC 9114) |
| :--- | :--- | :--- | :--- |
| **1. Transport Protocol** | TCP (Layer 4) | TCP (Layer 4) | **QUIC (over UDP Port 443)** |
| **2. Connection Model** | Persistent sequential connection (`keep-alive`). | **Multiplexed single TCP connection** per origin domain. | **Multiplexed single QUIC connection** per origin domain. |
| **3. Framing Format** | **Plain text ASCII**: Headers and body sent as raw text strings. | **Binary Framing Layer**: Messages split into binary Frames (HEADERS, DATA). | **Binary Framing Layer** over native QUIC streams. |
| **4. Head-of-Line (HoL) Blocking** | **Application-Level HoL Blocking**: Responses must return in exact requested order. | **TCP-Level HoL Blocking**: 1 dropped TCP segment halts ALL interleaved streams! | **ZERO Head-of-Line Blocking**: Streams are fully independent in Layer 4 QUIC. |
| **5. Header Compression** | **None**: Repetitive headers (Cookies, User-Agent) sent in full text every request. | **HPACK**: Uses static table, dynamic table, and Huffman encoding. | **QPACK**: Optimized for out-of-order delivery over independent UDP streams. |
| **6. Server Capabilities** | Basic request-response pull. | **Server Push**: Proactively pushes dependent assets (CSS/JS) before requested. | **Server Push** supported. |
| **7. Handshake Latency** | **2 RTTs** (1 RTT TCP + 1 RTT TLS 1.3) + 1 RTT HTTP GET. | **2 RTTs** (1 RTT TCP + 1 RTT TLS 1.3) + 1 RTT HTTP GET. | **0-RTT to 1-RTT** total (Integrated TLS 1.3 handshake inside QUIC). |
| **8. Connection Migration** | None; switching from Wi-Fi to 5G breaks TCP socket 4-tuple. | None; switching IP severs TCP connection. | **Seamless Migration**: Bound to 64-bit Connection ID (CID); survives IP change! |

**Marking Scheme**:
- 1 Mark per correctly contrasted parameter across all 8 rows = **Total 8 Marks**.

---

### Q8. Base64 MIME Encoding Numerical & DNS Resource Records
**Model Answer**:

#### Part 1: Base64 Encoding Numerical (4 Marks)
- **Given**: File size = `12,000 Bytes`.
- **Step 1: Check Divisibility by 3**:
  `12,000 / 3 = 4,000` (exact integer; no remainder).
- **Step 2: Number of 6-bit Base64 Chunks**:
  Each 3 bytes (24 bits) produce 4 Base64 characters.
  `Total Chunks = 4,000 × 4 =` **16,000 characters**. *(1.5 Marks)*
- **Step 3: Number of Padding Characters**:
  Since `12,000 % 3 == 0`, **ZERO (`0`) padding `=` characters** are required. *(1 Mark)*
- **Step 4: Final Size and Percentage Expansion**:
  - Encoded size = `16,000 Bytes` (in ASCII).
  - Expansion = `(16,000 - 12,000) / 12,000 = 4,000 / 12,000 =` **33.33% increase**. *(1.5 Marks)*

#### Part 2: DNS Resource Records & TCP/UDP Switching (4 Marks)
1. **Resource Records (2.5 Marks — 0.5 Mark each)**:
   - **`A`**: `[Hostname ➔ IPv4 Address]` (e.g., `vit.edu ➔ 115.240.10.2`).
   - **`AAAA`**: `[Hostname ➔ 128-bit IPv6 Address]` (e.g., `vit.edu ➔ 2001:db8::1`).
   - **`CNAME`**: `[Alias ➔ Canonical Name]` (e.g., `www.vit.edu ➔ webserver1.vit.edu`).
   - **`MX`**: `[Domain ➔ Mail Server Host + Priority]` (e.g., `vit.edu ➔ 10 mail.vit.edu`).
   - **`PTR`**: `[Reversed IP ➔ Hostname]` (e.g., `2.10.240.115.in-addr.arpa ➔ vit.edu`).
2. **When DNS Switches from UDP to TCP Port 53 (1.5 Marks)**:
   - **Standard queries use UDP Port 53** because they are small and fast.
   - **Switches to TCP Port 53 under two conditions**:
     1. **Response Exceeds 512 Bytes**: When standard UDP response size exceeds 512 bytes (or EDNS0 buffer limit) or carries large **DNSSEC** cryptographic signatures, the server sets the **TC (Truncation) flag = 1**, forcing the client to re-issue the query over **TCP Port 53**.
     2. **Zone Transfers (AXFR / IXFR)**: When a secondary DNS server replicates an entire zone database from the primary DNS server, requiring guaranteed, sequenced delivery.

**Marking Scheme**:
- 4 Marks for Part 1 numerical (1.5M chunks, 1M padding, 1.5M size & expansion).
- 4 Marks for Part 2 (2.5M for 5 RR types, 1.5M for TCP Port 53 conditions) = **Total 8 Marks**.

---

### Q9. DHCP DORA Message Sequence Chart & Lease Timers
**Model Answer**:

#### Part 1: Message Sequence Chart with Headers (4 Marks)
```
      DHCP Client (MAC: aa:bb:cc:dd:ee:ff)              DHCP Server (IP: 192.168.1.1)
           |                                                      |
           | ---------- 1. DHCP DISCOVER (Broadcast) ------------> |
           |    Src IP: 0.0.0.0, Dst IP: 255.255.255.255          |
           |    Src Port: 68, Dst Port: 67, xid: 0x4A1B           |
           |                                                      |
           | <--------- 2. DHCP OFFER (Broadcast/Unicast) ------- |
           |    Src IP: 192.168.1.1, Dst IP: 255.255.255.255      |
           |    Offered IP: 192.168.1.50, Mask: 255.255.255.0     |
           |    GW: 192.168.1.1, DNS: 8.8.8.8, Lease: 86400s      |
           |                                                      |
           | ---------- 3. DHCP REQUEST (Broadcast) ------------> |
           |    Src IP: 0.0.0.0, Dst IP: 255.255.255.255          |
           |    Accepted IP: 192.168.1.50, Server ID: 192.168.1.1 |
           |                                                      |
           | <--------- 4. DHCP ACK (Broadcast/Unicast) --------- |
           |    Src IP: 192.168.1.1, Dst IP: 255.255.255.255      |
           |    Lease finalized; Client configures interface       |
           v                                                      v
```

#### Part 2: Lease Renewal Timers & DHCP Relay Agent (4 Marks)
1. **Lease Timers (2 Marks)**:
   - **T1 Renewal Timer (50% of Lease Time, e.g., 43,200s for a 24-hr lease)**: Client sends a **unicast `DHCPREQUEST`** directly to the original leasing server. If the server replies with `DHCPACK`, the lease is renewed back to 100%.
   - **T2 Rebinding Timer (87.5% of Lease Time, e.g., 75,600s)**: If the original server failed to respond at T1, the client waits until T2, then **broadcasts a `DHCPREQUEST`** to `255.255.255.255` asking *any* available server on the network to extend the lease.
   - **Expiration (100%)**: If no ACK arrives by 100%, the client must immediately release the IP address and restart DORA.
2. **DHCP Relay Agent (2 Marks)**:
   - Routers by default block Layer 2 broadcast packets (`255.255.255.255`).
   - A **DHCP Relay Agent** (RFC 3046) configured on a router interface listens for client broadcasts on UDP Port 67. It intercepts the broadcast `DHCPDISCOVER`, encapsulates it inside a standard **unicast IP packet** (`GIADDR = Relay Interface IP`), and forwards it across WAN links directly to a centralized corporate DHCP server, eliminating the need for a separate physical DHCP server on every subnet.

**Marking Scheme**:
- 4 Marks for complete DORA chart with IPs, ports, and xid.
- 2 Marks for T1 (50%) and T2 (87.5%) timers.
- 2 Marks for DHCP Relay Agent operation = **Total 8 Marks**.

---

### Q10. RESTful Web Services vs SOAP
**Model Answer**:

| Parameter | REST (Representational State Transfer) | SOAP (Simple Object Access Protocol) |
| :--- | :--- | :--- |
| **1. Architecture vs Protocol** | **Architectural style**: Architectural guidelines leveraging native HTTP features. | **Formal rigid protocol**: Standardized by W3C with strict XML schema definitions. |
| **2. Transport Protocol** | Exclusively tied to **HTTP / HTTPS**. | **Transport independent**: Can run over HTTP, SMTP, TCP, JMS. |
| **3. Data Formats** | Multi-format: **JSON** (predominant), XML, YAML, plain text. | **Strictly XML only**: Wrapped in SOAP `<Envelope>`, `<Header>`, and `<Body>`. |
| **4. Operations / Interface** | Standard HTTP verbs: `GET` (read), `POST` (create), `PUT` (update), `DELETE` (remove). | Operations defined in **WSDL (Web Services Description Language)** contract via Remote Procedure Calls. |
| **5. Caching & Performance** | **Highly cacheable** (`GET` responses cached via HTTP cache headers); lightweight and fast. | **Cannot be cached** (all requests sent via HTTP POST); heavy XML parsing overhead. |
| **6. Security Standards** | Uses standard web security: **HTTPS (TLS) + OAuth 2.0 / JWT**. | Built-in enterprise security specifications: **WS-Security**, WS-AtomicTransaction. |

#### Sample Snippets:
- **REST Request (Lightweight JSON)**:
  ```http
  GET /api/students/101 HTTP/1.1
  Host: api.vit.edu
  Accept: application/json

  // Response:
  {"id": 101, "name": "Shaunak", "branch": "CE"}
  ```
- **SOAP Request (Verbose XML Envelope)**:
  ```xml
  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
      <getStudentDetailsRequest>
        <studentId>101</studentId>
      </getStudentDetailsRequest>
    </soapenv:Body>
  </soapenv:Envelope>
  ```

**Marking Scheme**:
- 6 Marks for contrasting table across all 6 parameters (1M each).
- 2 Marks for REST JSON vs SOAP XML sample request/response snippets = **Total 8 Marks**.

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. End-to-End Web Page Request Journey & HTTP Status Codes
**Model Answer**:

#### Part 1: Chronological End-to-End Protocol Journey for `https://www.vit.edu/index.html` (7 Marks)
1. **Phase 1: Dynamic Network Configuration (DHCP - Layer 7 / UDP)**:
   - Laptop connects to campus Wi-Fi/Ethernet. It broadcasts a `DHCPDISCOVER` packet on UDP Port 67.
   - Campus DHCP server replies through the DORA process (`Offer`, `Request`, `Ack`), assigning the laptop its IP address (`192.168.1.100`), subnet mask (`255.255.255.0`), default gateway router IP (`192.168.1.1`), and Local DNS Server IP (`8.8.8.8`). *(1.5 Marks)*
2. **Phase 2: Default Gateway Hardware Resolution (ARP - Layer 2/3)**:
   - The laptop needs to send DNS packets outside the local subnet to `8.8.8.8`.
   - It checks its local ARP cache for the default gateway IP `192.168.1.1`.
   - If missing, it broadcasts an **ARP Request** (`FF:FF:FF:FF:FF:FF`) asking *"Who has 192.168.1.1?"*
   - The gateway router responds with an **ARP Reply** containing its hardware MAC address. *(1 Mark)*
3. **Phase 3: Domain Name Resolution (DNS - Layer 7 / UDP Port 53)**:
   - Browser generates a recursive query for `www.vit.edu` sent to Local DNS `8.8.8.8` over UDP Port 53.
   - Local DNS traverses Root (`.`) ➔ TLD (`.edu`) ➔ Authoritative (`vit.edu`) servers iteratively, caching and returning the A record: `115.240.10.2`. *(1.5 Marks)*
4. **Phase 4: TCP Connection & TLS 1.3 Handshake (Layer 4 TCP / Layer 7 TLS)**:
   - Browser initiates a TCP 3-way handshake to `115.240.10.2:443`: `SYN` ➔ `SYN+ACK` ➔ `ACK`.
   - Client and server perform the TLS 1.3 cryptographic handshake: ClientHello with KeyShare, ServerHello, Certificate exchange, cipher negotiation, generating symmetric encryption keys. *(1.5 Marks)*
5. **Phase 5: HTTP Request & Page Rendering (Layer 7 HTTP/2)**:
   - Browser transmits encrypted HTTP request: `GET /index.html HTTP/2`.
   - Server returns `200 OK` with HTML payload.
   - Browser parses HTML, discovers referenced resources (CSS, JavaScript, images), and fetches them concurrently over multiplexed HTTP/2 streams (or receives them via Server Push), rendering the final page on screen. *(1.5 Marks)*

#### Part 2: HTTP Status Codes Explanation (3 Marks — ~0.4 Mark each)
- **`200 OK` (2xx Success)**: Request succeeded; requested entity is delivered in payload body.
- **`301 Moved Permanently` (3xx Redirection)**: The resource has permanently relocated to the URL in the `Location` header; client updates bookmarks.
- **`304 Not Modified` (3xx Redirection)**: Cached copy at client is still valid (in response to conditional `If-Modified-Since` request); payload omitted to save bandwidth.
- **`400 Bad Request` (4xx Client Error)**: Server cannot process request due to malformed client syntax or invalid framing.
- **`403 Forbidden` (4xx Client Error)**: Server understood authentication, but refuses authorization; client lacks access rights.
- **`404 Not Found` (4xx Client Error)**: Server cannot locate any resource matching the requested URI.
- **`502 Bad Gateway` (5xx Server Error)**: A proxy/gateway received an invalid or corrupt response from the upstream origin server.
- **`503 Service Unavailable` (5xx Server Error)**: Server is currently unable to handle request due to temporary overloading or maintenance downtime.

**Marking Scheme**:
- 7 Marks for Part 1 (DHCP 1.5M, ARP 1M, DNS 1.5M, TCP/TLS 1.5M, HTTP fetch 1.5M).
- 3 Marks for Part 2 (correct category and operational description of all 8 codes) = **Total 10 Marks**.

---

### Q12. End-to-End Email Delivery Journey & FTP Active vs Passive Modes
**Model Answer**:

#### Part 1: End-to-End Email Journey (`alice@vit.edu` to `bob@gmail.com`) (6 Marks)
1. **Step 1: Alice MUA ➔ vit.edu Mail Server (SMTP Submission on Port 587)**:
   - Alice composes email and clicks send. Her MUA establishes a TCP connection to `mail.vit.edu:587`.
   - After TLS negotiation, MUA issues standard SMTP commands:
     - `EHLO client.vit.edu`
     - `AUTH LOGIN` (Alice authenticates credentials)
     - `MAIL FROM: <alice@vit.edu>`
     - `RCPT TO: <bob@gmail.com>`
     - `DATA`
     - `From: Alice <alice@vit.edu>`
     - `To: Bob <bob@gmail.com>`
     - `Subject: Endsem Exam Prep`
     - `Hello Bob, preparing for CN Unit VI.`
     - `<CRLF>.<CRLF>` (Terminating period)
     - `QUIT`
   - The `vit.edu` server enqueues the email in its local spool queue. *(2 Marks)*
2. **Step 2: DNS MX Lookup by vit.edu MTA**:
   - The `vit.edu` MTA extracts the destination domain `gmail.com`.
   - It performs a DNS query for the **`MX` (Mail Exchange)** record of `gmail.com`.
   - DNS returns Google's mail exchangers with priority ranks (e.g., `10 gmail-smtp-in.l.google.com`).
   - vit.edu resolves that hostname to its public IP via an `A` record query. *(1.5 Marks)*
3. **Step 3: Server-to-Server SMTP Relay Across Internet (Port 25)**:
   - The `vit.edu` MTA opens a TCP connection to Google's MTA on **Port 25**.
   - It repeats the SMTP command dialogue (`EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`, `<CRLF>.<CRLF>`).
   - Google's MTA accepts the message, verifies SPF/DKIM authentication records, and deposits the email into Bob's mailbox spool storage on Google servers. *(1.5 Marks)*
4. **Step 4: Bob Retrieves Email via IMAP4 (Port 993 SSL)**:
   - Bob opens his Gmail smartphone app (MUA). The app connects to `imap.gmail.com:993` using TLS.
   - App authenticates Bob and issues IMAP commands (`FETCH`, `SELECT INBOX`).
   - Google sends headers first; when Bob opens the email, body payload and attachments are downloaded while remaining permanently synchronized on the server. *(1 Mark)*

#### Part 2: FTP Active vs Passive Mode with Packet Flow (4 Marks)
- **Active Mode**:
  1. Client connects from ephemeral port `1050` to Server Port `21` (Control).
  2. Client issues `PORT 192.168.1.10, 1051`.
  3. **Server initiates connection from Port 20 to Client Port `1051` (Data)**.
  4. **Why it fails**: Client's NAT router/firewall drops unsolicited inbound connection from server port 20, causing active mode to fail. *(2 Marks)*
- **Passive Mode (`PASV`)**:
  1. Client connects from port `1050` to Server Port `21` (Control).
  2. Client issues `PASV`.
  3. Server opens random port `52000` and replies: `227 Entering Passive Mode (ServerIP, 52000)`.
  4. **Client initiates outbound connection from Port `1051` to Server Port `52000` (Data)**.
  5. **Why it succeeds**: Client NAT/firewall permits outbound connections without issue. Modern standard across all FTP software. *(2 Marks)*

**Marking Scheme**:
- 6 Marks for Part 1 (Submission 2M, DNS MX 1.5M, Relay 1.5M, IMAP 1M).
- 4 Marks for Part 2 (Active mode flow & firewall flaw 2M, Passive mode flow & resolution 2M) = **Total 10 Marks**.
