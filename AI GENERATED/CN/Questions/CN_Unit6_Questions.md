# Computer Networks (CS3213) — Unit VI: Application Layer Exam Questions

**Target Exam**: End-Semester Written Examination  
**Pattern**: 4 Marks (Short Answer), 8 Marks (Analytical / Numerical), 10 Marks (Comprehensive Long Answer)

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Explain the DNS resolution process for a client querying `www.vit.edu`. Draw a diagram contrasting a **Recursive Query** with an **Iterative Query**, and explain where each query mode is utilized in the DNS hierarchy.

### Q2. [4 Marks]
Differentiate between HTTP/1.0 and HTTP/1.1 with respect to:
1. Connection persistence (non-persistent vs persistent `keep-alive`)
2. Latency overhead per embedded web object (RTTs)
3. Pipelining support
4. Virtual hosting support (`Host` header requirement)

### Q3. [4 Marks]
Explain the architecture of Electronic Mail. Distinguish between SMTP, POP3, and IMAP with respect to operational role (Push vs Pull), default transport port numbers, and multi-device mailbox synchronization.

### Q4. [4 Marks]
What is MIME (Multipurpose Internet Mail Extensions)? Explain how the **Base64 encoding algorithm** converts 8-bit binary data into safe 7-bit ASCII characters. State the exact percentage size overhead introduced by Base64.

### Q5. [4 Marks]
Explain why FTP uses a **dual-connection architecture** (out-of-band signaling). Differentiate between FTP Active Mode and FTP Passive Mode (`PASV`), explaining why Active Mode fails when the client is located behind a NAT firewall.

### Q6. [4 Marks]
Describe the four chronological steps of the **DHCP DORA Process** (Discover, Offer, Request, Acknowledge). Why is the `DHCPREQUEST` packet broadcast to `255.255.255.255` instead of being unicast directly to the server whose offer was chosen?

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. [8 Marks]
Provide an exhaustive comparative analysis between **HTTP/1.1**, **HTTP/2**, and **HTTP/3** across the following parameters:
1. Underlying transport layer protocol
2. Connection model and multi-stream multiplexing
3. Data framing format (Plain text vs Binary)
4. Head-of-Line (HoL) blocking behavior (Application-level vs TCP-level vs Zero HoL)
5. Header compression mechanisms (HPACK vs QPACK)
6. Initial connection handshake latency and security integration

### Q8. [8 Marks]
1. A binary image file of size **12,000 bytes** must be transmitted as an email attachment using Base64 MIME encoding. Calculate:
   - The total number of 6-bit Base64 character chunks generated
   - The number of padding `=` characters required (if any)
   - The final encoded attachment size in bytes and the total percentage size expansion. *(4 Marks)*
2. Explain the syntax and purpose of five fundamental DNS Resource Record (RR) types: `A`, `AAAA`, `CNAME`, `MX`, and `PTR`. Under what specific operational conditions does DNS switch from UDP Port 53 to TCP Port 53? *(4 Marks)*

### Q9. [8 Marks]
1. Draw the complete message sequence chart of the **DHCP DORA Process**, indicating the Source/Destination IP addresses, Source/Destination UDP port numbers, and Transaction IDs at each of the 4 steps. *(4 Marks)*
2. Explain the operation of the **T1 (Renewal)** and **T2 (Rebinding)** lease renewal timers. Explain why a **DHCP Relay Agent** is required in multi-subnet enterprise networks and how it operates across router boundaries. *(4 Marks)*

### Q10. [8 Marks]
Compare **RESTful Web Services** and **SOAP (Simple Object Access Protocol)** across:
1. Architectural style vs formal protocol specification
2. Underlying transport protocol constraints
3. Payload data representations (JSON vs XML)
4. State management and caching
5. Standards, contracts, and security mechanisms (WSDL/WS-* vs OpenAPI/HTTPS/OAuth)
6. Lightweight performance and mobile API suitability.  
Provide a concise sample HTTP request/response snippet for each.

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. [10 Marks]
1. Trace in exhaustive chronological detail the end-to-end network journey that occurs when a student powers on a laptop connected to a campus LAN and navigates to `https://www.vit.edu/index.html` in a web browser. Detail the exact protocols invoked, their layer, and their functions across:
   - Automatic host configuration via DHCP (Layer 7)
   - Gateway hardware address resolution via ARP (Layer 2/3)
   - Hostname resolution via hierarchical DNS (Layer 7 / Layer 4 UDP)
   - Secure TCP / TLS connection establishment (Layer 4 TCP / Layer 7 TLS)
   - HTTP/2 or HTTP/3 page fetch and embedded object rendering. *(7 Marks)*
2. Explain HTTP response status codes. State the numerical category and exact operational meaning of: `200 OK`, `301 Moved Permanently`, `304 Not Modified`, `400 Bad Request`, `403 Forbidden`, `404 Not Found`, `502 Bad Gateway`, and `503 Service Unavailable`. *(3 Marks)*

### Q12. [10 Marks]
1. Draw the complete architectural model of electronic mail delivery. Trace the end-to-end path of an email composed by `alice@vit.edu` and sent to `bob@gmail.com`. Detail:
   - The SMTP command sequence (`HELO/EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`, `QUIT`) exchanged between Alice's MUA and the `vit.edu` mail server.
   - The DNS `MX` lookup performed by `vit.edu`'s MTA to discover Gmail's incoming mail exchange servers.
   - The SMTP relay across the public Internet to Google's MTA.
   - The IMAP4 retrieval workflow executed by Bob's smartphone MUA. *(6 Marks)*
2. Explain the design motivation behind FTP's dual-connection (out-of-band) architecture. Contrast **FTP Active Mode** with **FTP Passive Mode (`PASV`)** using complete packet sequence diagrams showing IP addresses, port numbers, and direction of connection establishment. Clearly explain why Active FTP fails when the client is behind a NAT router or firewall, and how Passive FTP overcomes this problem. *(4 Marks)*
