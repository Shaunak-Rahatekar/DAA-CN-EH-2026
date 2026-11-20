# Computer Networks (CS3213) — Unit V: Transport Layer Comprehensive Notes

**Target Exam**: End-Semester Written Examination (26 November 2026, 11:00 AM – 1:00 PM)  
**Weightage**: ~16 to 20 Marks  
**Universal Notation Rule**: Unicode math symbols only (`≤`, `≥`, `×`, `→`, `∈`, `²`, `ⁿ`, `₁`). Clean ASCII diagrams for exam reproduction.

---

## 1. Transport Layer Fundamentals & Services

The Transport Layer (Layer 4) is the core of the Internet protocol architecture. While the Network Layer provides host-to-host delivery, the Transport Layer provides **process-to-process delivery** (logical communication between running application programs).

### 1.1 Addressing: Port Numbers & Sockets
- **Port Numbers**: 16-bit unsigned integers (range `0` to `65,535`) identifying individual application processes running on a host.
  - **Well-Known Ports (`0` – `1023`)**: Assigned by IANA for standard server services (e.g., HTTP `80`, HTTPS `443`, DNS `53`, SSH `22`, SMTP `25`, DHCP `67/68`).
  - **Registered Ports (`1024` – `49,151`)**: Used by third-party vendor applications (e.g., MySQL `3306`, PostgreSQL `5432`).
  - **Dynamic / Private / Ephemeral Ports (`49,152` – `65,535`)**: Temporary ports automatically assigned by the operating system to client processes.
- **Socket**: An endpoint for network communication formed by combining an IP address and a port number:  
  `Socket = (IP Address : Port Number)`  
  Example: `192.168.1.15:52431`.
- **Connection Identifier (Socket Pair 4-Tuple)**:  
  `Connection = (Source IP, Source Port, Destination IP, Destination Port)`

### 1.2 Multiplexing and Demultiplexing
- **Multiplexing (Sender Side)**: The transport layer gathers data chunks from multiple application processes (via distinct sockets), encapsulates each chunk with transport header containing source and destination port numbers, and passes them down to Layer 3.
- **Demultiplexing (Receiver Side)**: The transport layer inspects the destination port in incoming transport segments and directs the payload to the exact corresponding socket.
  - **Connectionless Demultiplexing (UDP)**: A UDP socket is identified solely by a 2-tuple: `(Destination IP, Destination Port)`. Two packets from different source IPs/ports destined for port 53 land in the same socket.
  - **Connection-Oriented Demultiplexing (TCP)**: A TCP socket is identified by the full 4-tuple: `(Source IP, Source Port, Destination IP, Destination Port)`. Even if 1000 clients connect to server port 80, each client is assigned a separate, dedicated socket.

### 1.3 Berkeley Sockets API
The standard programming interface for network communication (POSIX/UNIX):

```
       TCP Server (Passive)                          TCP Client (Active)
    +-------------------------+                   +-------------------------+
    | socket()                |                   |                         |
    | (Create communication   |                   |                         |
    |  endpoint)              |                   |                         |
    +------------+------------+                   |                         |
                 |                                |                         |
    +------------v------------+                   |                         |
    | bind()                  |                   |                         |
    | (Assign local IP:Port)  |                   |                         |
    +------------+------------+                   |                         |
                 |                                |                         |
    +------------v------------+                   |                         |
    | listen()                |                   | socket()                |
    | (Set passive mode &     |                   | (Create endpoint)       |
    |  backlog queue)         |                   +------------+------------+
    +------------+------------+                                |
                 |                                             |
    +------------v------------+                   +------------v------------+
    | accept()                | <=== 3-Way ====>  | connect()               |
    | (Blocks until client    |      Handshake    | (Initiate active open)  |
    |  connects; returns new  |                   +------------+------------+
    |  connected socket)      |                                |
    +------------+------------+                                |
                 |                                             |
    +------------v------------+                   +------------v------------+
    | recv() / read()         | <=== Data ======> | send() / write()        |
    | send() / write()        |      Transfer     | recv() / read()         |
    +------------+------------+                   +------------+------------+
                 |                                             |
    +------------v------------+                   +------------v------------+
    | close()                 | <=== 4-Way =====> | close()                 |
    | (Release socket)        |      Teardown     | (Release socket)        |
    +-------------------------+                   +-------------------------+
```

- **Exam Function Summary**:
  - `socket(domain, type, protocol)`: Allocates transport endpoint (`AF_INET`, `SOCK_STREAM` for TCP, `SOCK_DGRAM` for UDP).
  - `bind(sockfd, addr, addrlen)`: Binds socket to a specific local port and IP address.
  - `listen(sockfd, backlog)`: Marks socket as passive; defines pending connection queue length.
  - `accept(sockfd, client_addr, len)`: Blocks until a connection request arrives; creates a **brand-new socket descriptor** dedicated to this specific client connection, leaving the listening socket free for new incoming requests.
  - `connect(sockfd, server_addr, len)`: Client triggers TCP 3-way handshake to destination server.

---

## 2. User Datagram Protocol (UDP - RFC 768)

UDP is a minimal, lightweight, connectionless transport protocol providing "best-effort" datagram delivery.

### 2.1 Key Characteristics
1. **Connectionless**: No connection setup handshake (0-RTT transmission).
2. **Unreliable**: No acknowledgments, no retransmissions, no duplicate detection, no sequence numbers.
3. **Message-Oriented**: Preserves application message boundaries. If an application writes 500 bytes 3 times, UDP sends 3 distinct datagrams, and the receiver reads 500 bytes 3 times (unlike TCP's continuous stream).
4. **No Flow Control & No Congestion Control**: Sends data at whatever speed the application generates it, regardless of receiver buffer capacity or network congestion.

### 2.2 UDP Header Format (Fixed 8 Bytes)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |  (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |  (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Data Payload (Variable)                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
- **Source Port (16 bits)**: Port of sending process (optional; set to 0 if no reply is expected).
- **Destination Port (16 bits)**: Port of destination process (mandatory).
- **Length (16 bits)**: Total length of UDP datagram in bytes (Header + Payload). Minimum value = 8 bytes.
- **Checksum (16 bits)**: 1's complement sum covering a 12-byte **IP Pseudo-Header** (Source IP, Destination IP, Protocol=17, UDP Length), the 8-byte UDP header, and the data payload. Optional in IPv4 (set to 0 if unused), but **strictly mandatory in IPv6**.

### 2.3 Applications of UDP
- Real-time streaming (VoIP, video conferences) where low latency is critical and dropped packets are preferred over retransmission delay.
- Simple query-response protocols (DNS, DHCP, NTP) where connection setup overhead would exceed data transfer time.
- Network management (SNMP) and routing information exchange (RIP).

---

## 3. Transmission Control Protocol (TCP - RFC 793)

TCP is a connection-oriented, highly reliable, byte-stream oriented transport protocol.

### 3.1 Comprehensive TCP vs UDP Comparison Matrix

| Parameter | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Connection Nature** | Connection-Oriented (3-way handshake) | Connectionless (0 setup time) |
| **Data Stream Model** | **Byte-Stream Oriented** (no message boundaries) | **Message-Oriented** (preserves boundaries) |
| **Reliability** | Guaranteed (ACKs, retransmission, ordering) | Best-effort (packets may be lost/corrupted) |
| **Ordering** | Strictly in-order delivery via sequence numbers | Out-of-order delivery possible |
| **Header Size** | **20 to 60 Bytes** (Variable, options) | **Fixed 8 Bytes** |
| **Flow Control** | Yes (Sliding window via `rwnd`) | None (receiver can be overwhelmed) |
| **Congestion Control** | Yes (Slow Start, Congestion Avoidance, AIMD) | None (transmits blindly into congested links) |
| **Transmission Mode** | Full-Duplex Point-to-Point (Unicast only) | Unicast, Multicast, and Broadcast |
| **Handshake Latency** | 1 RTT connection setup + teardown | 0 RTT (immediate transmission) |
| **Use Cases** | Web (HTTP/HTTPS), Email (SMTP), File Transfer (FTP) | Real-time audio/video, DNS, DHCP, Gaming, QUIC |

---

### 3.2 TCP Segment Header Format (20 to 60 Bytes)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |  Row 0
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |  Row 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |  Row 2
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|DataOff| Reserv|C|E|U|A|P|R|S|F|          Window Size          |  Row 3
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |        Urgent Pointer         |  Row 4
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options + Padding (0 - 40 Bytes)           |  Row 5
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Field Explanations:
1. **Source Port & Destination Port (16 bits each)**: Identifies application processes.
2. **Sequence Number (32 bits)**: Specifies the byte sequence number of the first data byte in this segment. During connection setup (`SYN=1`), it contains the Initial Sequence Number (ISN).
3. **Acknowledgment Number (32 bits)**: Valid only if `ACK = 1`. Contains the sequence number of the **next byte the receiver expects to receive** (cumulative ACK).
4. **Data Offset / HLEN (4 bits)**: Number of 32-bit words in the TCP header. Minimum = 5 (5 × 4 = 20 Bytes); Maximum = 15 (15 × 4 = 60 Bytes).
5. **Reserved (4 bits)**: Set to zero for future use.
6. **Control Flags (8 bits)**:
   - `CWR (Congestion Window Reduced)`: Set by sender after reducing cwnd.
   - `ECE (ECN-Echo)`: Indicates network congestion detected via IP ECN bits.
   - `URG`: Urgent pointer field is valid.
   - `ACK`: Acknowledgment field is valid (set in all packets except initial SYN).
   - `PSH`: Push data immediately to application without waiting for buffer fill.
   - `RST`: Reset connection (abrupt termination upon error or rejected connection).
   - `SYN`: Synchronize sequence numbers; used during 3-way handshake.
   - `FIN`: Sender has finished transmitting data; initiates connection close.
7. **Window Size (16 bits)**: Flow control. Number of bytes the receiver is currently willing to accept (`rwnd`).
8. **Checksum (16 bits)**: Mandatory 1's complement sum covering TCP pseudo-header, TCP header, and data.
9. **Urgent Pointer (16 bits)**: Byte offset pointing to urgent data (valid when `URG = 1`).
10. **Options (0 – 40 Bytes)**:
    - **MSS (Maximum Segment Size)**: Maximum payload bytes TCP will accept in a single segment (typically 1460 Bytes on Ethernet).
    - **Window Scale**: Multiplies 16-bit Window Size by 2ⁿ (up to 1 GB window for high BDP networks).
    - **SACK (Selective Acknowledgment)**: Allows receiver to acknowledge non-contiguous blocks of received bytes.

---

### 3.3 TCP Connection Management

#### A. Connection Establishment: 3-Way Handshake
```
      Client (Active Open)                               Server (Passive Open)
           |                                                      |  LISTEN
           | --------- Step 1: SYN=1, Seq=x --------------------> |
  SYN_SENT |                                                      |  SYN_RCVD
           | <-------- Step 2: SYN=1, ACK=1, Seq=y, Ack=x+1 ------ |
ESTABLISHED|                                                      |
           | --------- Step 3: ACK=1, Seq=x+1, Ack=y+1 ---------> |
           |                                                      |  ESTABLISHED
```
- **Step 1 (Client ➔ Server)**: Client sends `SYN=1, ACK=0`, picks random Initial Sequence Number `Seq = x`. Consumes 1 sequence number.
- **Step 2 (Server ➔ Client)**: Server responds with `SYN=1, ACK=1`, sets its own `Seq = y`, and acknowledges client: `Ack = x + 1`. Consumes 1 sequence number.
- **Step 3 (Client ➔ Server)**: Client sends `ACK=1`, sets `Seq = x + 1`, and acknowledges server: `Ack = y + 1`. May carry initial application payload.
- **Why a 2-Way Handshake Fails (Vital Exam Question)**:
  - Suppose an old SYN packet from a previous, already-closed session was delayed in the network.
  - In a 2-way handshake, when this duplicate SYN finally arrives, the server would immediately send an ACK and transition to ESTABLISHED, allocating memory and socket buffers.
  - However, the client knows nothing about this old session and ignores the ACK, creating a **half-open zombie connection** on the server that wastes memory indefinitely.
  - In a 3-way handshake, the client receives an unexpected `SYN+ACK`, recognizes an invalid sequence number, and responds with a `RST` segment, terminating the half-open connection instantly.

#### B. Connection Teardown: 4-Way Handshake (Graceful Close)
```
      Client (Active Close)                              Server (Passive Close)
           |                                                      |  ESTABLISHED
           | --------- Step 1: FIN=1, Seq=u --------------------> |
FIN_WAIT_1 |                                                      |  CLOSE_WAIT
           | <-------- Step 2: ACK=1, Ack=u+1 ------------------- |  (Server can still
FIN_WAIT_2 |                                                      |   send data!)
           |                                                      |
           | <-------- Step 3: FIN=1, Seq=w, Ack=u+1 ------------ |  LAST_ACK
 TIME_WAIT |                                                      |
           | --------- Step 4: ACK=1, Ack=w+1 ------------------> |  CLOSED
(Wait 2*MSL|                                                      |
 ➔ CLOSED) v                                                      v
```
- **Half-Close (`CLOSE_WAIT` state)**: TCP is full-duplex. When the client sends `FIN`, it indicates: *"I have no more data to send."* The server acknowledges (`ACK`) and enters `CLOSE_WAIT`. The server can continue sending unacknowledged data to the client until it finishes and sends its own `FIN`.
- **The `TIME_WAIT` State and 2*MSL Timer**:
  - The active closer enters `TIME_WAIT` and starts a timer set to **2 × MSL (Maximum Segment Lifetime)** (RFC standard MSL = 1 to 2 minutes; timer = 60 to 120s).
  - **Two Reasons for 2*MSL**:
    1. **Guarantees Final ACK Delivery**: If the client's final ACK (Step 4) is lost, the server retransmits its FIN (Step 3). The client must remain in `TIME_WAIT` to resend the ACK; otherwise, it would reply with `RST`, causing an abnormal termination on the server.
    2. **Draining Old Segments**: Prevents "wandering duplicate segments" from the terminated connection from accidentally interfering with a newly opened incarnation using the exact same socket 4-tuple.

---

## 4. TCP Flow Control & Buffering

Flow control prevents a fast sender from overflowing a slow receiver's internal buffer.

### 4.1 Sliding Window Flow Control
- **Receiver Window (`rwnd`)**: The receiver advertises its available buffer space in the 16-bit **Window Size** header field of every ACK.
- **Governing Invariant**:
  `LastByteSent - LastByteAcked ≤ rwnd`
- **Sender Window Sliding**: As ACKs arrive, the left edge of the sender's window slides to the right (freeing acknowledged buffer space). The usable window expands or shrinks depending on the advertised `rwnd`.

### 4.2 Silly Window Syndrome (SWS) & Solutions
Occurs when data is generated or consumed in tiny increments (e.g., 1 byte at a time), causing the network to transport tiny data payloads encapsulated inside massive 40-byte headers (20B IP + 20B TCP).
- **Efficiency**: 1 Byte Data / 41 Bytes Packet = **2.4% network efficiency**!

#### Sender-Side Solution: Nagle's Algorithm
- **Rule**:
  1. Send the first piece of application data immediately, even if it is only 1 byte.
  2. After that, accumulate all incoming application data in a local buffer until:
     - The accumulated data reaches a **Full Maximum Segment Size (MSS)**, **OR**
     - An **ACK for the previously sent segment** is received.
- Eliminates tiny-grams on high-delay WAN links by coalescing data into full-sized segments.

#### Receiver-Side Solution: Clark's Solution & Delayed ACK
- **Clark's Solution**: Prevents the receiver from advertising small window increments. If free buffer space is small, the receiver advertises `rwnd = 0` until:
  - Free buffer space is at least **1 full MSS**, **OR**
  - Free buffer space is at least **half of the total receiver buffer capacity** (`Buffer_Total / 2`).
- **Delayed Acknowledgment**: The receiver delays sending an ACK (by up to 200 ms or until 2 full-sized segments arrive) to allow the local application time to consume data, enabling the receiver to advertise a much larger window.

---

## 5. TCP Timer Management

TCP employs four specialized timers to maintain reliability and prevent deadlocks:

### 5.1 Retransmission Timer (RTO)
Controls how long a sender waits for an ACK before retransmitting a segment.
- **Static RTO fails**: Internet delay is dynamic.
- **Jacobson's RTO Algorithm (Standard RFC 6298)**:
  1. Measure sample Round-Trip Time ($RTT_m$) for a segment.
  2. Compute Smoothed RTT ($RTT_s$):  
     `RTT_s = (1 - α) × RTT_s + α × RTT_m` (typically `α = 1/8 = 0.125`).
  3. Compute RTT Variation / Deviation ($D$):  
     `D = (1 - β) × D + β × |RTT_s - RTT_m|` (typically `β = 1/4 = 0.25`).
  4. Compute Retransmission Timeout ($RTO$):  
     **`RTO = RTT_s + 4 × D`**
- **Karn's Algorithm (Crucial Rule)**:
  - *Do not update $RTT_s$ or $D$ using retransmitted segments!* (Ambiguity: you cannot know whether an incoming ACK is for the original segment or the retransmitted segment).
  - On timeout retransmission, apply **Exponential Timer Back-off**:  
    `RTO_new = 2 × RTO_old`.

### 5.2 Persistence Timer
- **Zero-Window Deadlock**: When the receiver buffer fills completely, it advertises `rwnd = 0`. The sender stops transmitting. Later, the receiver application reads data and sends an ACK advertising `rwnd = 2000`. If this ACK is lost, the sender is waiting for a window update and the receiver is waiting for data ➔ **Permanent Deadlock!**
- **Solution**: When `rwnd = 0` is received, the sender starts a **Persistence Timer**. Upon expiration, it sends a **Probe Segment** (carrying 1 byte of dummy data). The receiver replies with an ACK showing its current `rwnd`.

### 5.3 Keep-Alive Timer
- Prevents a server from holding resources for a client that crashed silently or disconnected without closing.
- If a connection is completely idle for **2 hours**, the server sends a probe segment. If no reply is received after 9 to 10 probes (at 75-second intervals), the server terminates the connection and releases socket resources.

### 5.4 Time-Wait Timer
- Runs during the `TIME_WAIT` state for **2 × MSL** (typically 120 seconds) before fully transitioning to `CLOSED`.

---

## 6. TCP Congestion Control & AIMD

Congestion occurs when aggregate network traffic exceeds link capacity, causing router queue overflow and packet loss.

### 6.1 The Congestion Window (`cwnd`)
The sender maintains a dynamic variable called **`cwnd`** (Congestion Window in bytes or MSS).  
The maximum allowed outstanding (unacknowledged) data is strictly capped by:  
**`Allowed Window = min(cwnd, rwnd)`**

### 6.2 The Four Intertwined Congestion Algorithms

#### 1. Slow Start:
- Starts with `cwnd = 1 MSS` (or 2 to 4 MSS in modern TCP).
- For every valid ACK received:  
  `cwnd = cwnd + 1 MSS`
- **Result**: `cwnd` **doubles every RTT** (1, 2, 4, 8, 16...). Exponential growth!
- Ends when `cwnd ≥ ssthresh` (Slow Start Threshold), transitioning into Congestion Avoidance.

#### 2. Congestion Avoidance (Additive Increase):
- Probe link capacity cautiously without inducing congestion.
- For every RTT (or for every valid ACK: `cwnd = cwnd + 1/cwnd`):  
  `cwnd = cwnd + 1 MSS`
- **Result**: Linear growth (+1 MSS per RTT: 16, 17, 18, 19...).

#### 3. Handling Packet Loss (Multiplicative Decrease):
- **Scenario A: Retransmission Timeout (Severe Congestion / Link Down)**:
  - `ssthresh = max(cwnd / 2, 2 MSS)`.
  - `cwnd = 1 MSS`.
  - Resumes **Slow Start** from `cwnd = 1` up to new `ssthresh`. (Both Tahoe and Reno behave identically here).
- **Scenario B: Triple Duplicate ACKs (Mild Congestion / Isolated Packet Loss)**:
  - If 3 duplicate ACKs arrive for the same segment, the receiver is still receiving out-of-order packets! The network is not collapsed.
  - **Fast Retransmit**: Retransmits the missing segment immediately without waiting for RTO timeout.
  - **TCP Tahoe**: Sets `ssthresh = cwnd / 2`, drops `cwnd = 1 MSS`, and resumes Slow Start.
  - **TCP Reno (Fast Recovery)**:
    - Sets `ssthresh = max(cwnd / 2, 2 MSS)`.
    - Sets `cwnd = ssthresh` (or `ssthresh + 3 MSS`).
    - **Skips Slow Start completely** and enters Congestion Avoidance linearly!

### 6.3 Worked Congestion Control Numerical (High-Yield Exam Trace)
**Problem**: A TCP Reno connection has `ssthresh = 16 MSS` and starts with `cwnd = 1 MSS`.  
Show the `cwnd` values for transmission rounds 1 to 14 if:
- A triple duplicate ACK occurs at round 8.
- A timeout occurs at round 12.

**Trace Table**:

| Round | cwnd (MSS) | Phase | Events / Calculation |
| :---: | :---: | :--- | :--- |
| **1** | 1 | Slow Start | Initial `cwnd = 1`, doubles each RTT |
| **2** | 2 | Slow Start | 2 < 16 |
| **3** | 4 | Slow Start | 4 < 16 |
| **4** | 8 | Slow Start | 8 < 16 |
| **5** | 16 | Reached ssthresh | 16 = ssthresh ➔ switch to Congestion Avoidance |
| **6** | 17 | Congestion Avoidance | Linear increase (+1 MSS) |
| **7** | 18 | Congestion Avoidance | Linear increase (+1 MSS) |
| **8** | 19 | Congestion Avoidance | **Triple Duplicate ACK occurs at cwnd = 19!** |
| **9** | 9 | Fast Recovery (Reno) | `ssthresh = 19 / 2 = 9`. Reno sets `cwnd = ssthresh = 9`. |
| **10** | 10 | Congestion Avoidance | Linear increase (+1 MSS) |
| **11** | 11 | Congestion Avoidance | Linear increase (+1 MSS) |
| **12** | 12 | Congestion Avoidance | **Retransmission TIMEOUT occurs at cwnd = 12!** |
| **13** | 1 | Slow Start | `ssthresh = 12 / 2 = 6`. `cwnd` resets to **1 MSS**. |
| **14** | 2 | Slow Start | Doubles (2 < 6) |

---

## 7. The QUIC Protocol (Quick UDP Internet Connections - RFC 9000)

QUIC is a modern general-purpose transport protocol designed by Google and standardized by IETF as the foundation for **HTTP/3**.

### 7.1 Why Was QUIC Created? (TCP Limitations)
1. **OS Kernel Ossification**: TCP is implemented inside OS kernels. Deploying new TCP features requires updating millions of operating system kernels, taking decades.
2. **Middlebox Rigidity**: Firewalls and NAT routers inspect and drop TCP packets containing unexpected options.
3. **High Handshake Latency**: TCP (1 RTT) + TLS 1.3 (1 RTT) requires 2 to 3 round trips before the first byte of application data is sent.
4. **Transport-Layer Head-of-Line (HoL) Blocking**: In HTTP/2, multiple application streams are multiplexed over one single TCP connection. If a single packet belonging to Stream 1 is lost, TCP halts delivery of *all other streams* (Streams 2, 3, 4) until the lost packet is retransmitted.

### 7.2 Core Architectural Innovations of QUIC
1. **Built on Top of UDP in User Space**:
   - Packets look like standard UDP datagrams to intermediate routers and firewalls, bypassing middlebox interference and allowing user-space application upgrades (in browsers/servers) without OS updates.
2. **0-RTT and 1-RTT Handshake**:
   - Integrates transport connection establishment and TLS 1.3 cryptographic key exchange into a single step. Initial connections take **1 RTT**; returning clients achieve **0-RTT** data delivery!
3. **Elimination of Head-of-Line (HoL) Blocking**:
   - QUIC understands independent streams natively at Layer 4. Packet loss on Stream A stalls only Stream A; Streams B and C continue receiving data without delay.
4. **Connection Migration via 64-bit Connection ID (CID)**:
   - Traditional TCP identifies connections by the 4-tuple `(SrcIP, SrcPort, DstIP, DstPort)`. When a mobile user walks out of Wi-Fi range and switches to cellular 5G, the IP address changes, causing all active TCP connections to break.
   - QUIC binds connections to an opaque **Connection ID (CID)**. Even if IP address or port changes, the connection migrates seamlessly without dropping ongoing transfers or calls.
