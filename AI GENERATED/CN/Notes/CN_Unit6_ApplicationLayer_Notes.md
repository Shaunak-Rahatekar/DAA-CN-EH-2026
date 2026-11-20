# Computer Networks (CS3213) — Unit VI: Application Layer Comprehensive Notes

**Target Exam**: End-Semester Written Examination (26 November 2026, 11:00 AM – 1:00 PM)  
**Weightage**: ~16 to 20 Marks  
**Universal Notation Rule**: Unicode math symbols only (`≤`, `≥`, `×`, `→`, `∈`, `²`, `ⁿ`, `₁`). Clean ASCII diagrams for exam reproduction.

---

## 1. Domain Name System (DNS - RFC 1034 / 1035)

The Domain Name System (DNS) is an application-layer distributed, hierarchical database service that maps human-friendly hostnames (e.g., `www.vit.edu`) to machine-routable IP addresses (e.g., `115.240.10.2` or IPv6 `2001:db8::1`).

### 1.1 The DNS Hierarchical Namespace
Because no single server could handle global Internet traffic or store billions of records, DNS organizes names into an **inverted tree hierarchy**:
1. **Root DNS Servers (`"."`)**:
   - The root of the domain tree.
   - Managed by 13 distinct logical authorities (named `a.root-servers.net` through `m.root-servers.net`), replicated globally using Anycast routing across hundreds of physical server clusters.
   - Returns pointers to Top-Level Domain (TLD) servers.
2. **Top-Level Domain (TLD) Servers**:
   - **Generic TLDs (gTLDs)**: `.com`, `.edu`, `.org`, `.gov`, `.net`, `.io`.
   - **Country-Code TLDs (ccTLDs)**: `.in`, `.uk`, `.de`, `.jp`, `.us`.
   - Maintained by registries (e.g., Verisign for `.com`, Educause for `.edu`).
3. **Authoritative DNS Servers**:
   - Servers maintained by organizations (or their DNS hosting providers) that hold the official, definitive mappings for hostnames within that organization's domain (e.g., `vit.edu` or `google.com`).
4. **Local DNS Server (Default Resolver)**:
   - Operated by an ISP, university campus, or residential router.
   - Acts as the intermediary proxy: when a host generates a DNS query, it queries its configured Local DNS server.

---

### 1.2 DNS Query Resolution: Recursive vs Iterative

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

#### Step-by-Step Resolution Trace for `www.vit.edu`:
1. **Step 1 (Recursive)**: The client application passes `www.vit.edu` to the local resolver. The client sends a **Recursive Query** to its Local DNS Server.
2. **Step 2 (Iterative)**: The Local DNS server checks its cache. If absent, it queries a Root DNS Server (`"."`).
3. **Step 3**: The Root Server does not know the IP of `www.vit.edu`, but it returns an **iterative referral** containing the IP addresses of the `.edu` TLD name servers.
4. **Step 4 (Iterative)**: The Local DNS server queries the `.edu` TLD server.
5. **Step 5**: The `.edu` TLD server returns a referral containing the IP of the authoritative name server for `vit.edu`.
6. **Step 6 (Iterative)**: The Local DNS server queries the authoritative server for `vit.edu`.
7. **Step 7**: The authoritative server looks up its zone records and returns the definitive **A record** (`115.240.10.2`).
8. **Step 8**: The Local DNS server caches the mapping locally for the duration specified by the record's **TTL (Time to Live)** and delivers the final resolved IP address to the client host.

#### Recursive vs Iterative Comparison (Exam High-Yield):
- **Recursive Query**: The querying client places the entire burden of name resolution on the queried server. The queried server *must* return either the requested IP address or an error message (it cannot refer the client elsewhere). Used between Client ➔ Local DNS.
- **Iterative Query**: The queried server answers with the best answer it currently possesses: either the requested mapping or a referral pointer to another DNS server down the hierarchy. Used between Local DNS ➔ Root / TLD / Authoritative servers to prevent overloading root infrastructure.

---

### 1.3 DNS Resource Record (RR) Types
Every DNS response carries Resource Records formatted as:  
`[ Name, Value, Type, TTL ]`

| Record Type | Name Field | Value Field | Purpose / Exam Description |
| :---: | :--- | :--- | :--- |
| **`A`** | Hostname (e.g., `vit.edu`) | 32-bit IPv4 Address (`115.240.10.2`) | Maps hostname to an IPv4 address. |
| **`AAAA`** | Hostname | 128-bit IPv6 Address (`2001:db8::1`) | Maps hostname to an IPv6 address ("Quad-A"). |
| **`CNAME`** | Alias name (e.g., `www.vit.edu`) | Canonical real name (`server1.vit.edu`) | Canonical Name; aliases an alternate name to the true hostname. |
| **`MX`** | Domain name (e.g., `vit.edu`) | Mail server host + priority (`10 mail.vit.edu`) | Mail Exchange; routes incoming email to destination mail servers. |
| **`NS`** | Domain name (e.g., `vit.edu`) | Name server hostname (`ns1.vit.edu`) | Identifies authoritative name server for the zone. |
| **`PTR`** | Reversed IP (e.g., `2.10.240.115.in-addr.arpa`) | Hostname (`vit.edu`) | Pointer Record; used for Reverse DNS (IP ➔ Hostname). |
| **`TXT`** | Domain name | Arbitrary text string | Used for domain verification, SPF, DKIM, and DMARC security. |

- **DNS Transport Protocol**:
  - Uses **UDP Port 53** for standard client-server queries and responses where datagram size is `< 512 Bytes` (low latency, 0-RTT).
  - Uses **TCP Port 53** when response size exceeds 512 bytes (EDNS0), for **DNSSEC** cryptographic keys, and for **Zone Transfers** (AXFR/IXFR) between primary and secondary name servers requiring reliable delivery.

---

## 2. World Wide Web & Hypertext Transfer Protocol (HTTP)

HTTP is a stateless, application-level client-server protocol used to transfer hypertext documents and hypermedia across the Internet.

### 2.1 HTTP Architecture & URI/URL
- **URL Syntax**: `protocol://hostname:port/path?query#fragment`  
  Example: `https://www.vit.edu:443/academics/syllabus.pdf?year=2026`
- **Stateless Nature**: HTTP servers maintain no memory or session state across consecutive client requests. State is maintained artificially using **Cookies** or **Authorization Tokens (JWT)**.

### 2.2 HTTP Request Methods & Status Codes
- **Key Methods**:
  - `GET`: Retrieve resource specified by URI (Safe & Idempotent).
  - `POST`: Submit data to server to create a resource or execute processing (Non-idempotent).
  - `PUT`: Replace or create the resource specified by URI with request body (Idempotent).
  - `DELETE`: Remove resource at URI (Idempotent).
  - `HEAD`: Same as GET but server returns *headers only* (no payload body); used for checking caching and resource existence.
  - `OPTIONS`: Inquires which HTTP methods the server supports for that resource (CORS preflight).
- **HTTP Status Code Classes**:
  - **`1xx (Informational)`**: Request received, continuing process (e.g., `101 Switching Protocols`).
  - **`2xx (Success)`**: Action received, understood, and accepted.
    - `200 OK`: Standard successful response.
    - `201 Created`: Resource successfully created (via POST).
    - `204 No Content`: Action succeeded; response body is empty.
  - **`3xx (Redirection)`**: Further action needed to complete request.
    - `301 Moved Permanently`: Resource permanently relocated to new URL.
    - `302 Found`: Temporary redirection.
    - `304 Not Modified`: Cached copy at client is still fresh (Conditional GET).
  - **`4xx (Client Error)`**: Client caused an error.
    - `400 Bad Request`: Malformed syntax.
    - `401 Unauthorized`: Authentication required.
    - `403 Forbidden`: Authenticated, but client lacks permission.
    - `404 Not Found`: Requested resource does not exist.
  - **`5xx (Server Error)`**: Server failed to fulfill a valid request.
    - `500 Internal Server Error`: Generic server-side crash.
    - `502 Bad Gateway`: Invalid response received from upstream proxy.
    - `503 Service Unavailable`: Server overloaded or undergoing maintenance.
    - `504 Gateway Timeout`: Upstream server timed out.

---

### 2.3 Comprehensive HTTP Evolution: 1.0 vs 1.1 vs 2.0 vs 3.0

| Feature | HTTP/1.0 (RFC 1945) | HTTP/1.1 (RFC 2616 / 7230) | HTTP/2 (RFC 7540) | HTTP/3 (RFC 9114) |
| :--- | :--- | :--- | :--- | :--- |
| **Underlying Transport**| TCP | TCP | TCP | **QUIC (over UDP)** |
| **Connection Model** | **Non-Persistent**: 1 TCP connection per object. | **Persistent**: Reuses TCP connection (`keep-alive`). | **Multiplexed**: Single TCP connection per origin. | **Multiplexed**: Single QUIC connection per origin. |
| **Handshake Latency** | 2 RTTs per object (1 RTT TCP + 1 RTT Get). | 1 RTT per subsequent object. | 1 RTT per subsequent object. | **0-RTT to 1-RTT** total (Built-in TLS 1.3). |
| **Framing Format** | Plain text ASCII | Plain text ASCII | **Binary Framing Layer** (Frames & Streams) | **Binary Framing** (QUIC streams) |
| **Head-of-Line (HoL) Blocking** | Full HoL blocking at connection level. | **Application-Level HoL Blocking**: Requests pipelined, but responses must arrive in order! | **TCP-Level HoL Blocking**: 1 dropped packet stalls ALL interleaved streams! | **NO HoL Blocking**: Streams are fully independent in QUIC transport! |
| **Header Handling** | Plain text headers repeated every request. | Plain text headers repeated. | **HPACK Compression** (Huffman + Dynamic tables). | **QPACK Compression** (Out-of-order safe). |
| **Server Capabilities**| Basic pull | Basic pull | **Server Push** (proactively sends CSS/JS) | Server Push supported |
| **Connection Migration**| None (IP change drops TCP) | None (IP change drops TCP) | None (IP change drops TCP) | **Seamless Migration via 64-bit CID** |

---

### 2.4 Web Services: REST vs SOAP
- **REST (Representational State Transfer)**:
  - Architectural style based on stateless, standardized HTTP verbs (`GET`, `POST`, `PUT`, `DELETE`).
  - Data representation typically lightweight **JSON** or XML.
  - Cacheable, scalable, decoupled, universally adopted for web and mobile APIs.
- **SOAP (Simple Object Access Protocol)**:
  - Rigid, standardized protocol requiring strict **XML envelope** formatting and **WSDL (Web Services Description Language)** contracts.
  - Transport independent (can run over HTTP, SMTP, TCP).
  - Built-in enterprise security (WS-Security) and ACID transaction support, but verbose and heavyweight.

---

## 3. Electronic Mail (E-mail Architecture & Protocols)

Email architecture consists of three primary software components:
1. **Mail User Agent (MUA)**: Client application used by end users to compose, send, and read email (e.g., Microsoft Outlook, Mozilla Thunderbird, Gmail web app).
2. **Mail Transfer Agent (MTA)**: Server daemon that relays email messages across networks using SMTP (e.g., Postfix, Sendmail, Exim).
3. **Message Access Agent (MAA)**: Server daemon that allows client MUAs to retrieve stored email from mailboxes using POP3 or IMAP.

```
  Sender Host                  Sender Mail Server            Receiver Mail Server          Receiver Host
 +-----------+                +------------------+          +--------------------+        +---------------+
 |  Sender   | -- SMTP Push ->| Sender MTA /     |-- SMTP ->| Receiver MTA /     |<- Pull-| Receiver      |
 |  MUA      |   (Port 587)   | Spool Queue      | (Port 25)| Mailbox Storage    |(POP3/  | MUA           |
 +-----------+                +------------------+          +--------------------+ IMAP)  +---------------+
```

### 3.1 Simple Mail Transfer Protocol (SMTP - RFC 5321)
- **Role**: A **Push Protocol** used to push email from sender MUA ➔ sender MTA, and from sender MTA ➔ receiver MTA.
- **Ports**:
  - **Port 25 (TCP)**: Traditional unencrypted server-to-server relay.
  - **Port 587 (TCP)**: Standard modern client submission port (requires authentication & STARTTLS encryption).
  - **Port 465 (TCP)**: Deprecated legacy SMTPS (Implicit SSL/TLS).
- **Core ASCII Command Flow**:
  1. `HELO` / `EHLO`: Handshake identification.
  2. `MAIL FROM: <sender@vit.edu>`: Identifies sender envelope address.
  3. `RCPT TO: <receiver@gmail.com>`: Identifies recipient envelope address.
  4. `DATA`: Signals start of mail content (headers + body), terminated by a single line containing only a period (`<CRLF>.<CRLF>`).
  5. `QUIT`: Closes connection.
- **Major Limitation**: Historically restricted to **7-bit ASCII text only**. Cannot natively transmit binary files, images, PDFs, or non-Latin alphabets.

---

### 3.2 Multipurpose Internet Mail Extensions (MIME - RFC 2045)
MIME is a set of extensions that transforms non-ASCII and binary data into 7-bit ASCII text so it can safely pass through legacy SMTP relays.
- **Key MIME Headers**:
  - `MIME-Version: 1.0`
  - `Content-Type: text/html` or `image/jpeg` or `multipart/mixed; boundary="xyz"`
  - `Content-Transfer-Encoding: base64`
- **Base64 Encoding Algorithm (High-Yield Numerical)**:
  - Takes **3 bytes (24 bits)** of binary input and splits them into **four 6-bit chunks**.
  - Each 6-bit chunk (values 0 to 63) is mapped to a printable ASCII character from the 64-character alphabet:  
    `A-Z (0-25), a-z (26-51), 0-9 (52-61), + (62), / (63)`.
  - **Padding**: If the binary data length is not divisible by 3, `=` characters are appended as padding:
    - 1 byte leftover ➔ 2 Base64 characters + `==`.
    - 2 bytes leftover ➔ 3 Base64 characters + `=`.
  - **Size Overhead**: Base64 increases data size by exactly **33.3%** (`4 / 3 = 1.333`).

---

### 3.3 Message Access Protocols: POP3 vs IMAP4

| Parameter | POP3 (Post Office Protocol v3) | IMAP4 (Internet Message Access Protocol v4) |
| :--- | :--- | :--- |
| **Port Numbers** | **Port 110** (Plaintext) / **Port 995** (SSL/TLS) | **Port 143** (Plaintext) / **Port 993** (SSL/TLS) |
| **Storage Model** | **Client-centric**: Downloads messages to local disk and deletes from server by default. | **Server-centric**: Emails remain stored permanently on mail server. |
| **Multi-Device Sync** | **Fails completely**: One device downloads mail; other devices cannot see it. | **Seamless synchronization**: Read/unread status, sent mail, and folders stay synchronized across all devices. |
| **Folder Organization**| Local folders on client only; no server folder support. | Hierarchical folders created and managed directly on server. |
| **Message Retrieval** | Must download entire message (including heavy attachments) before viewing. | Supports **partial retrieval**: downloads message headers first; downloads attachments only on user request. |
| **Resource Usage** | Minimal server storage; high client storage. | Requires substantial, scalable server storage and CPU. |

---

## 4. File Transfer Protocol (FTP - RFC 959)

FTP is a client-server protocol used to transfer files reliably between a client and a remote server over TCP.

### 4.1 Dual-Connection Architecture (Out-of-Band Signaling)
Unlike HTTP (where control headers and data travel on the same connection), FTP separates control and data into **two distinct parallel TCP connections**:
1. **Control Connection (TCP Port 21)**:
   - Used exclusively for administrative commands (`USER`, `PASS`, `LIST`, `CWD`, `PWD`, `RETR`, `STOR`, `QUIT`) and 3-digit server status responses.
   - Remains open and active throughout the entire FTP session.
2. **Data Connection (TCP Port 20 or Ephemeral Port)**:
   - Used exclusively for transferring file payloads and directory listings.
   - Dynamically opened when a transfer begins and automatically closed when the file transfer finishes (EOF).

### 4.2 Active Mode vs Passive Mode (`PASV`)

```
      [ FTP Active Mode ]                                   [ FTP Passive Mode (PASV) ]

    Client                  Server                        Client                  Server
      |                       |                             |                       |
      | -- Port N ➔ Port 21 ->| Control                     | -- Port N ➔ Port 21 ->| Control
      | -- PORT N+1 --------->|                             | -- PASV ------------->|
      |                       |                             | <– Port P Info -------|
      | <- Port 20 ➔ Port N+1-| Data (Server Initiates!)    | -- Port N+1 ➔ Port P->| Data (Client Initiates!)
      |    (BLOCKED BY        |                             |    (PERMITTED BY      |
      |     CLIENT FIREWALL!) |                             |     CLIENT FIREWALL!) |
      v                       v                             v                       v
```

- **Active FTP Mode**:
  1. Client connects from ephemeral port `N` to server Port `21` (Control).
  2. Client sends command `PORT ClientIP, N+1`, instructing the server which client port is waiting for data.
  3. The **Server initiates an inbound connection from Port 20 to Client Port `N+1`**.
  4. **The Firewall Problem**: Modern home/enterprise firewalls and NAT gateways block all unsolicited incoming connections. The server's connection attempt to Port `N+1` is dropped, causing active FTP to fail.
- **Passive FTP Mode (`PASV`)**:
  1. Client connects to server Port `21` and sends command `PASV`.
  2. The server opens an unprivileged ephemeral port `P` (e.g., `51234`) and replies with `227 Entering Passive Mode (ServerIP, P)`.
  3. The **Client initiates the data connection from its own port `N+1` to Server Port `P`**.
  4. **Firewall Friendly**: Client-side firewalls permit outbound connections to Server Port `P`, allowing seamless file transfer. Passive mode is standard in modern networks.

---

## 5. Dynamic Host Configuration Protocol (DHCP - RFC 2131)

DHCP automatically provides network configuration parameters to newly booted or roaming hosts, eliminating manual IP configuration errors.

### 5.1 Automated Configuration Parameters
When a DHCP client binds to a server, it receives:
1. **Assigned IPv4 Address**
2. **Subnet Mask**
3. **Default Gateway Router IP Address**
4. **Primary and Secondary DNS Server IP Addresses**
5. **IP Lease Duration (in seconds)**

### 5.2 Transport & The DORA Process
DHCP runs over **UDP**:
- **DHCP Server** listens on **Port 67**.
- **DHCP Client** listens on **Port 68**.

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

1. **D — DHCP DISCOVER**:
   - The unconfigured host has no IP address.
   - It broadcasts a UDP packet with `Source IP = 0.0.0.0`, `Destination IP = 255.255.255.255` (Limited Broadcast), `Source Port = 68`, `Destination Port = 67`.
   - Carries the client's hardware MAC address and a random 32-bit Transaction ID (`xid`).
2. **O — DHCP OFFER**:
   - All DHCP servers on the subnet receive the broadcast.
   - Available servers reserve an unused IP address from their configured pool and send a `DHCPOFFER` packet containing: Offered IP, Subnet Mask, Lease Time, Server Identifier IP, and the matching Transaction ID.
3. **R — DHCP REQUEST**:
   - The client selects one offer (typically the first received) and **broadcasts** a `DHCPREQUEST` to `255.255.255.255`.
   - *Why broadcast?* To explicitly inform the chosen server that its offer is accepted, while simultaneously notifying all other servers that their offers were rejected, allowing them to release their reserved IPs back into their free pools!
4. **A — DHCP ACK**:
   - The selected server acknowledges with `DHCPACK`, finalizing the lease.
   - The client verifies the IP using Gratuitous ARP and configures its NIC.

---

### 5.3 Lease Renewal Timers (T1 and T2)
DHCP assigns IP addresses on a temporary lease rather than permanent ownership:
1. **T1 Timer (Renewal Timer — 50% of Lease Time)**:
   - When 50% of the lease duration expires, the client sends a **unicast `DHCPREQUEST`** directly to the original leasing server.
   - If the server replies with `DHCPACK`, the lease timer resets to 100%.
2. **T2 Timer (Rebinding Timer — 87.5% of Lease Time)**:
   - If the original server crashed or is unreachable at T1, the client continues using the IP until 87.5% of lease expires.
   - At T2, the client **broadcasts a `DHCPREQUEST`** to *any* available DHCP server on the network.
3. **Lease Expiration (100%)**:
   - If no server acknowledges by 100% lease expiration, the client must immediately release the IP address and restart the DORA process from `DHCPDISCOVER`.

### 5.4 DHCP Relay Agent (RFC 3046)
- Routers do not forward Layer 2 broadcasts (`255.255.255.255`).
- Without a relay, an organization would need a physical DHCP server on every single subnet.
- **Operation**: A router interface configured as a **DHCP Relay Agent** intercepts the client's broadcast `DHCPDISCOVER` on Port 67, encapsulates it into a standard **unicast IP packet**, and forwards it across the WAN directly to a centralized corporate DHCP server.
