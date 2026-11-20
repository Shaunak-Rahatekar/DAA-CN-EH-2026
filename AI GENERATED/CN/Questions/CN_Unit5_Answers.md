# Computer Networks (CS3213) — Unit V: Transport Layer Model Answers & Marking Scheme

**Target Exam**: End-Semester Written Examination  
**Scope**: Model Solutions with Examiner Marking Rubric for 4M, 8M, and 10M Questions

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. Differentiate between TCP and UDP.
**Model Answer**:

| Parameter | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Connection Model** | Connection-Oriented (3-way handshake SYN/ACK before data). | Connectionless (transmits immediately, 0-RTT setup). |
| **Data Stream Paradigm** | **Byte-Stream Oriented** (continuous stream; no application message boundaries). | **Message-Oriented** (preserves exact application packet/message boundaries). |
| **Reliability & Ordering** | Guaranteed delivery; in-order sequencing; duplicate removal via ACKs & retransmissions. | Best-effort; no ACKs, no retransmissions; packets may be lost or arrive out-of-order. |
| **Header Overhead** | **20 to 60 Bytes** (variable, 10+ fields, optional extensions). | **Fixed 8 Bytes** (Source Port, Dest Port, Length, Checksum). |
| **Flow & Congestion Control**| Yes; end-to-end sliding window (`rwnd`) and dynamic AIMD (`cwnd`). | None; sends blindly at whatever rate the application provides. |
| **Applications** | Web browsing (HTTP/HTTPS), Secure Shell (SSH), File Transfer (FTP), Email (SMTP). | Real-time voice/video (VoIP), DNS queries (Port 53), DHCP, Streaming, QUIC. |

**Marking Scheme**:
- 1 Mark per distinct, technically accurate contrasted parameter with real-world examples = **Total 4 Marks**.

---

### Q2. Explain the Berkeley Sockets API lifecycle on the server side and the role of `accept()`.
**Model Answer**:
1. **Server-Side API Lifecycle Sequence**:
   - `socket()`: Creates an unbound network transport endpoint, returning a socket file descriptor.
   - `bind()`: Associates the socket with a specific local IP address and port number (e.g., port 80).
   - `listen()`: Puts the socket into passive mode to listen for incoming connections; sets the maximum backlog queue size.
   - `accept()`: Blocks the server execution until a client connection request completes the 3-way handshake.
   - `recv() / read()` & `send() / write()`: Reads incoming data and transmits responses.
   - `close()`: Releases the socket descriptor and triggers 4-way connection teardown. *(2 Marks)*
2. **The Return Value of `accept()` and Socket Separation**:
   - The `accept()` system call returns a **brand-new, independent socket file descriptor** representing the active, dedicated full-duplex connection to that specific client (identified by the unique 4-tuple: `Client IP, Client Port, Server IP, Server Port`).
   - The **original listening socket is NOT closed or reused for data transfer**. It remains in the `LISTEN` state on the well-known port to continuously accept future connection requests from other clients, enabling multi-client concurrency. *(2 Marks)*

**Marking Scheme**:
- 2 Marks for the chronological socket lifecycle functions.
- 2 Marks for explaining the new descriptor returned by `accept()` and preserving the listening socket = **Total 4 Marks**.

---

### Q3. Draw the TCP 3-way handshake and explain why a 2-way handshake fails.
**Model Answer**:

#### Part 1: Timing Sequence Diagram (2 Marks)
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

#### Part 2: Why a 2-Way Handshake Fails (2 Marks)
- In a packet-switched network, segments can be delayed in intermediate router buffers for arbitrary durations.
- **Failure Scenario**:
  1. A client sends a `SYN` packet to initiate a connection, but the packet is heavily delayed in the network.
  2. The client times out, gives up, and eventually closes.
  3. Much later, the delayed duplicate `SYN` finally arrives at the server.
  4. In a hypothetical **2-way handshake**, the server would believe this is a genuine new request, send an `ACK`, immediately transition to `ESTABLISHED`, and allocate socket buffers and transmission state memory.
  5. However, the client knows nothing about this obsolete session and ignores the server's ACK.
  6. The server is left with a **half-open zombie connection** that ties up resources indefinitely.
- In a **3-way handshake**, when the client receives the server's unexpected `SYN+ACK` (Step 2), it detects an invalid sequence number and immediately sends a `RST` (Reset) packet, forcing the server to abort the bogus half-open connection.

**Marking Scheme**:
- 2 Marks for accurate timing diagram with flags and sequence numbers.
- 2 Marks for delayed duplicate scenario and half-open zombie connection defense = **Total 4 Marks**.

---

### Q4. Explain the four TCP timers and the rationale for `2 × MSL`.
**Model Answer**:
1. **Retransmission Timer (RTO)**: Measures elapsed time for an outstanding unacknowledged segment. If no ACK arrives before RTO expires, the sender assumes packet loss and retransmits the segment. Dynamically updated using Jacobson's formula. *(1 Mark)*
2. **Persistence Timer**: Prevents deadlock when an ACK opening a closed window (`rwnd > 0`) is lost in transit. Periodically sends a 1-byte "probe segment" to force the receiver to re-advertise its current buffer window. *(1 Mark)*
3. **Keep-Alive Timer**: Detects crashed or disconnected peers during prolonged idle silence. If no traffic is exchanged for 2 hours, the server sends probe segments; if 10 probes go unanswered, the dead connection is automatically terminated. *(1 Mark)*
4. **Time-Wait Timer (`2 × MSL`)**: Runs during the `TIME_WAIT` state at the active closer for twice the Maximum Segment Lifetime (typically 60 to 120 seconds).
   - **Two Critical Reasons**:
     1. **Guarantees Final ACK Delivery**: If the client's final ACK (Step 4 of teardown) is lost, the server will time out and retransmit its FIN. The client must remain alive in `TIME_WAIT` to resend the ACK; otherwise, it would send RST.
     2. **Flushes Wandering Segments**: Ensures all duplicate or delayed segments from the old connection completely drain and die in the network before a new connection reuses the same socket pair. *(1 Mark)*

**Marking Scheme**:
- 1 Mark each for RTO, Persistence, Keep-Alive, and Time-Wait (with 2*MSL rationale) = **Total 4 Marks**.

---

### Q5. Explain Silly Window Syndrome (SWS) and its solutions.
**Model Answer**:
1. **The Silly Window Syndrome Problem**:
   - Occurs when data is generated by the sender application or consumed by the receiver application in tiny increments (e.g., 1 byte at a time).
   - **Catastrophic Efficiency**: To transmit 1 byte of payload, TCP attaches a 20-byte TCP header and IP attaches a 20-byte IP header. A 41-byte datagram is sent over the wire:  
     `Efficiency = 1 Byte Data / 41 Bytes Packet = 2.4%`!  
     The network becomes overwhelmed with header overhead, causing massive congestion. *(1.5 Marks)*
2. **Sender-Side Solution: Nagle's Algorithm**:
   - Transmit the first chunk of application data immediately, even if it is only 1 byte.
   - After that, buffer all incoming application data locally until:
     - The buffer accumulates a **full Maximum Segment Size (MSS)**, **OR**
     - An **ACK for the previously sent segment** is received from the receiver.
   - Prevents the sender from injecting multiple tiny-grams onto the link. *(1.25 Marks)*
3. **Receiver-Side Solution: Clark's Solution & Delayed ACK**:
   - **Clark's Solution**: Prevents the receiver from advertising small window increments. If buffer space opens by tiny amounts, the receiver advertises `rwnd = 0` until:
     - Free buffer space reaches at least **1 full MSS**, **OR**
     - Free buffer space reaches at least **half of the total receiver buffer capacity** (`Buffer_Total / 2`).
   - **Delayed ACK**: The receiver delays sending an ACK by up to 200 ms to give the local application time to read more data, allowing the receiver to advertise a substantially larger window. *(1.25 Marks)*

**Marking Scheme**:
- 1.5 Marks for SWS problem and 2.4% efficiency calculation.
- 1.25 Marks for Nagle's algorithm.
- 1.25 Marks for Clark's solution and Delayed ACK = **Total 4 Marks**.

---

### Q6. Explain Jacobson's RTO algorithm and Karn's rule for retransmissions.
**Model Answer**:
1. **Jacobson's Dynamic RTO Algorithm**:
   - Static timeouts fail because Internet queue delays fluctuate dynamically.
   - For every acknowledged segment, TCP measures sample Round-Trip Time ($RTT_m$).
   - **Step 1: Compute Smoothed RTT ($RTT_s$)**:  
     `RTT_s = (1 - α) × RTT_s + α × RTT_m` (where standard `α = 0.125`).
   - **Step 2: Compute RTT Deviation / Variance ($D$)**:  
     `D = (1 - β) × D + β × |RTT_s - RTT_m|` (where standard `β = 0.25`).
   - **Step 3: Compute Retransmission Timeout ($RTO$)**:  
     **`RTO = RTT_s + 4 × D`**  
     *(Adding 4 times deviation provides a robust safety margin against sudden jitter).* *(2.5 Marks)*
2. **Karn's Algorithm Rule**:
   - **Rule**: *Never measure sample $RTT_m$ or update $RTT_s$ / $D$ for retransmitted segments!*
   - **Justification (The Retransmission Ambiguity Problem)**: When an ACK arrives after a retransmission, the sender cannot tell whether the ACK was generated by the receiver in response to the *original transmission* (which was simply delayed) or in response to the *retransmitted segment*. Measuring RTT on retransmissions severely corrupts the RTO estimate.
   - **Action on Timeout**: Apply **Exponential Timer Back-off**:  
     `RTO_new = 2 × RTO_old` (doubles timer upon every successive timeout). *(1.5 Marks)*

**Marking Scheme**:
- 2.5 Marks for Jacobson's formulas ($RTT_s$, $D$, $RTO = RTT_s + 4D$).
- 1.5 Marks for Karn's rule, retransmission ambiguity defense, and exponential back-off = **Total 4 Marks**.

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. TCP Segment Header Format & Field Significance
**Model Answer**:

#### Part 1: Header Format Diagram (4 Marks)
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

#### Part 2: Field Explanations (4 Marks)
1. **Sequence Number (32 bits)**: Tracks byte position in the continuous stream. Contains the byte sequence number of the first data byte carried in this segment.
2. **Acknowledgment Number (32 bits)**: Cumulative acknowledgment. Contains the sequence number of the *next byte expected* from the peer host.
3. **Window Size (`rwnd`) (16 bits)**: Flow control. Specifies the number of data bytes the receiving process can currently accept in its buffer.
4. **Control Flags**:
   - `SYN`: Synchronizes Initial Sequence Numbers during 3-way handshake.
   - `FIN`: Signals sender has finished data transmission; initiates graceful teardown.
   - `RST`: Resets connection abruptly upon irrecoverable error or rejected port.
   - `PSH`: Instructs receiving TCP to bypass buffers and push data directly to application immediately.
   - `URG`: Indicates that the 16-bit Urgent Pointer field is valid, pointing to high-priority out-of-band data (e.g., Ctrl+C interrupt).

**Marking Scheme**:
- 4 Marks for accurate 32-bit aligned diagram with all bit widths.
- 4 Marks for thorough explanation of Seq, Ack, Window Size, and 5 control flags = **Total 8 Marks**.

---

### Q8. TCP Congestion Control Trace: Tahoe vs Reno
**Model Answer**:

#### Complete Trace Table (Rounds 1 to 14):

| Round | cwnd (MSS) | Phase | Operational Events & Mathematical Calculations |
| :---: | :---: | :--- | :--- |
| **1** | 1 | Slow Start | Initial `cwnd = 1`. Doubles each RTT (`cwnd < ssthresh`). |
| **2** | 2 | Slow Start | `2 < 16` |
| **3** | 4 | Slow Start | `4 < 16` |
| **4** | 8 | Slow Start | `8 < 16` |
| **5** | 16 | Reached ssthresh | `cwnd = 16 = ssthresh`. Transitions to **Congestion Avoidance**. |
| **6** | 17 | Congestion Avoidance | Additive Increase: `cwnd = cwnd + 1 MSS`. |
| **7** | 18 | Congestion Avoidance | Additive Increase: `cwnd = cwnd + 1 MSS`. |
| **8** | 19 | Congestion Avoidance | **Event 1: Triple Duplicate ACK arrives at cwnd = 19!** |
| **9** | **9 (Reno)**<br>*(1 in Tahoe)* | **Fast Recovery (Reno)**<br>*(Slow Start in Tahoe)* | `ssthresh = 19 / 2 = 9.5 ➔ 9 MSS`.<br>**TCP Reno**: Sets `cwnd = ssthresh = 9`, stays in Congestion Avoidance.<br>**TCP Tahoe**: Resets `cwnd = 1 MSS`, enters Slow Start. |
| **10** | 10 (Reno)<br>*(2 in Tahoe)* | Congestion Avoidance (Reno)<br>*(Slow Start in Tahoe)* | Reno: Linear increase (`9 + 1 = 10`).<br>Tahoe: Doubles (`1 × 2 = 2`). |
| **11** | 11 (Reno)<br>*(4 in Tahoe)* | Congestion Avoidance (Reno)<br>*(Slow Start in Tahoe)* | Reno: Linear increase (`10 + 1 = 11`).<br>Tahoe: Doubles (`2 × 2 = 4`). |
| **12** | 12 (Reno)<br>*(8 in Tahoe)* | Congestion Avoidance | **Event 2: Retransmission Timeout (RTO) occurs at Round 12!** |
| **13** | **1** | **Slow Start (Both)** | `ssthresh = 12 / 2 = 6 MSS`.<br>**Both Tahoe and Reno reset cwnd = 1 MSS** and resume Slow Start! |
| **14** | 2 | Slow Start (Both) | Doubles each RTT (`1 × 2 = 2 MSS`, since `2 < 6`). |

#### Key Algorithmic Contrast:
- **At Round 8 (Triple Duplicate ACK)**:
  - **TCP Tahoe**: Treats 3 duplicate ACKs the same as a timeout. Drops `cwnd` back to **1 MSS**, requiring slow start recovery.
  - **TCP Reno (Fast Retransmit & Fast Recovery)**: Recognizes that duplicate ACKs prove subsequent packets are still reaching the receiver. Retransmits missing segment immediately, cuts `ssthresh` in half (`9 MSS`), sets `cwnd = 9 MSS`, and continues Congestion Avoidance linearly without dropping to 1.

**Marking Scheme**:
- 2 Marks for Slow Start phase (Rounds 1-5).
- 2 Marks for Congestion Avoidance phase (Rounds 6-8).
- 2 Marks for handling Triple Duplicate ACK and contrasting Tahoe vs Reno (Rounds 9-11).
- 2 Marks for handling Timeout at Round 12 and post-timeout Slow Start (Rounds 12-14) = **Total 8 Marks**.

---

### Q9. Sliding Window Flow Control & Usable Window Numerical
**Model Answer**:

#### Part 1: Sliding Window Flow Control Principles (4 Marks)
- **Mechanism**: TCP assigns an integer sequence number to every byte in the data stream. The receiver maintains a fixed-size buffer.
- **Receive Window (`rwnd`)**: The receiver advertises its available buffer space in the Window Size field of every ACK segment.
- **Difference between `rwnd` and `cwnd`**:
  - `rwnd` (Receive Window) is dictated by the **receiver's physical memory buffer** to prevent buffer overflow (Flow Control).
  - `cwnd` (Congestion Window) is dynamically calculated by the **sender** based on network delay, ACK arrivals, and drops to prevent overloading intermediate router links (Congestion Control).
- **Sender Boundary**: The sender's outstanding unacknowledged data is capped strictly by the minimum of both:  
  `Allowed Window = min(cwnd, rwnd)` *(4 Marks)*

#### Part 2: Numerical Calculation (4 Marks)
- **Given Parameters**:
  - `cwnd = 8000 Bytes`
  - `rwnd = 5000 Bytes`
  - `LastByteSent = 12,000`
  - `LastByteAcked = 9,000`
- **Step 1: Calculate Outstanding Unacknowledged Data in Flight**:  
  `FlightSize = LastByteSent - LastByteAcked = 12,000 - 9,000 =` **3,000 Bytes**. *(1.5 Marks)*
- **Step 2: Determine Maximum Allowed Transmission Window**:  
  `MaxAllowedWindow = min(cwnd, rwnd) = min(8000, 5000) =` **5,000 Bytes**.  
  *(Capped by receiver's advertised window `rwnd`)*. *(1 Mark)*
- **Step 3: Calculate Effective Usable Send Window**:  
  `UsableWindow = MaxAllowedWindow - FlightSize`  
  `UsableWindow = 5,000 - 3,000 =` **2,000 Bytes**. *(1.5 Marks)*
- **Interpretation**: The sender can transmit exactly **2,000 additional data bytes** immediately before it must pause and wait for incoming ACKs.

**Marking Scheme**:
- 4 Marks for Flow Control theory and `rwnd` vs `cwnd` distinction.
- 1.5 Marks for FlightSize (3000 B), 1 Mark for MaxAllowedWindow (5000 B), 1.5 Marks for final Usable Window (2000 B) = **Total 8 Marks**.

---

### Q10. TCP Connection Teardown (4-Way Handshake) & Half-Close
**Model Answer**:

#### Part 1: Timing Sequence Diagram & State Transitions (5 Marks)
```
      Client (Active Close)                              Server (Passive Close)
           |                                                      |  ESTABLISHED
           | --------- Step 1: FIN=1, Seq=u --------------------> |
FIN_WAIT_1 |                                                      |  CLOSE_WAIT
           | <-------- Step 2: ACK=1, Ack=u+1 ------------------- |  (Half-Closed;
FIN_WAIT_2 |                                                      |   Server sends data!)
           |                                                      |
           | <-------- Step 3: FIN=1, Seq=w, Ack=u+1 ------------ |  LAST_ACK
 TIME_WAIT |                                                      |
           | --------- Step 4: ACK=1, Ack=w+1 ------------------> |  CLOSED
(Wait 2*MSL|                                                      |
 ➔ CLOSED) v                                                      v
```
- **State Progression**:
  - **Client**: `ESTABLISHED` ➔ (Sends FIN) ➔ `FIN_WAIT_1` ➔ (Receives ACK) ➔ `FIN_WAIT_2` ➔ (Receives Server FIN) ➔ `TIME_WAIT` ➔ (2*MSL timeout) ➔ `CLOSED`.
  - **Server**: `ESTABLISHED` ➔ (Receives FIN, sends ACK) ➔ `CLOSE_WAIT` ➔ (Sends FIN) ➔ `LAST_ACK` ➔ (Receives final ACK) ➔ `CLOSED`.

#### Part 2: Half-Closed Connection Concept (`CLOSE_WAIT`) (3 Marks)
- TCP is an inherently **full-duplex** protocol comprising two independent, symmetric simplex streams.
- When the client application calls `close()` or `shutdown(SHUT_WR)`, it issues a `FIN` segment. This does **NOT** terminate the entire connection immediately!
- It indicates only that the **client has finished sending data**. The client can no longer transmit new payload.
- However, the server enters the **`CLOSE_WAIT`** state. The communication channel from Server to Client remains 100% active and open!
- **Practical Use Case**: In HTTP file downloads or remote database queries, a client sends its request and immediately half-closes its transmit channel. The server continues streaming massive megabytes of query results back to the client. Only when the server completes its transfer does it send its own `FIN` (Step 3) to initiate final closure.

**Marking Scheme**:
- 5 Marks for complete 4-way timing diagram with all states labeled accurately.
- 3 Marks for half-closed explanation, `CLOSE_WAIT` significance, and practical use case = **Total 8 Marks**.

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. Traditional TCP Limitations and the QUIC Protocol
**Model Answer**:

#### Part 1: Limitations of Traditional TCP/TLS/HTTP/2 Stack (5 Marks)
1. **OS Kernel Ossification**: TCP is baked into operating system kernels. Introducing improvements or modifying congestion algorithms requires updating billions of end-user device kernels globally, creating an adoption latency of 10 to 15 years.
2. **Middlebox Rigidity**: Firewalls, NAT gateways, and deep packet inspection middleboxes routinely drop or corrupt TCP segments containing novel TCP options (e.g., Multipath TCP, fast open), rendering TCP non-extensible.
3. **High Connection Handshake Latency**:
   - Establishing a secure web session requires separate sequential handshakes:
     - TCP Handshake = 1 RTT (`SYN` ➔ `SYN+ACK` ➔ `ACK`).
     - TLS 1.3 Cryptographic Handshake = 1 RTT (Key exchange, certificate validation).
   - Total latency = **2 RTTs** (or 3 RTTs in TLS 1.2) before sending the first HTTP request.
4. **Transport-Layer Head-of-Line (HoL) Blocking**:
   - In HTTP/2, dozens of logical streams (HTML, CSS, images, JS) are multiplexed over a single TCP byte stream.
   - Because TCP enforces strictly contiguous byte-stream ordering, if **one single packet belonging to Stream 1 is lost**, TCP stalls delivery of *all subsequent received packets across Streams 2, 3, and 4* until the lost packet is retransmitted and acknowledged!

#### Part 2: QUIC Architecture & Three Core Innovations (5 Marks)
```
      [ Traditional Web Stack ]                     [ Modern HTTP/3 Stack ]
     +-------------------------+                   +-------------------------+
     |   HTTP/2 (Multiplexing) |                   |         HTTP/3          |
     +-------------------------+                   +-------------------------+
     |   TLS 1.2 / 1.3         |                   |  QUIC (Stream Mux,      |
     +-------------------------+                   |  Built-in TLS 1.3,      |
     |   TCP (Byte Stream)     |                   |  Congestion Control)    |
     +-------------------------+                   +-------------------------+
     |   IP (Network)          |                   |   UDP (User Datagram)   |
     +-------------------------+                   +-------------------------+
                                                   |   IP (Network)          |
                                                   +-------------------------+
```

1. **0-RTT and 1-RTT Handshake**:
   - QUIC integrates the transport connection setup and TLS 1.3 cryptographic negotiation into a single unified handshake.
   - First-time connection takes **1 RTT**.
   - Returning clients use cached session tickets to achieve **0-RTT data delivery**, sending encrypted HTTP/3 requests in the very first datagram!
2. **Elimination of Head-of-Line (HoL) Blocking**:
   - QUIC implements independent streams natively at Layer 4.
   - If a packet carrying data for Stream A is lost, only Stream A is paused. Packets for Streams B and C are delivered to the application immediately without delay.
3. **Connection Migration via 64-Bit Connection ID (CID)**:
   - TCP identifies connections by the 4-tuple `(SrcIP, SrcPort, DstIP, DstPort)`. When a smartphone moves from Wi-Fi to cellular data, its IP address changes, abruptly severing all TCP connections.
   - QUIC identifies connections by an opaque **64-bit Connection ID (CID)** independent of IP addresses. A client can change networks, IP addresses, or NAT ports seamlessly without tearing down the connection.

**Marking Scheme**:
- 5 Marks for TCP limitations (Ossification, Middlebox, Latency, HoL blocking).
- 2 Marks for comparative stack diagram.
- 3 Marks for QUIC innovations (0-RTT, No HoL blocking, CID Migration) = **Total 10 Marks**.

---

### Q12. Jacobson's RTO Numerical Calculation & AIMD Convergence
**Model Answer**:

#### Part 1: Step-by-Step RTO Calculation (6 Marks)
- **Given Formulas**:
  - `RTT_s = (1 - α) × RTT_s + α × RTT_m = 0.875 × RTT_s + 0.125 × RTT_m`
  - `D = (1 - β) × D + β × |RTT_s_old - RTT_m| = 0.75 × D + 0.25 × |RTT_s_old - RTT_m|`
  - `RTO = RTT_s + 4 × D`
- **Initial State**: `RTT_s = 40 ms`, `D = 5 ms`, `RTO = 40 + 4(5) = 60 ms`.

**Calculations per Round**:
- **Round 1 (`RTT_m = 48 ms`)**:
  - Error: `|40 - 48| = 8 ms`.
  - `RTT_s = (0.875 × 40) + (0.125 × 48) = 35 + 6 =` **41.0 ms**.
  - `D = (0.75 × 5) + (0.25 × 8) = 3.75 + 2.0 =` **5.75 ms**.
  - `RTO = 41.0 + 4 × 5.75 = 41.0 + 23.0 =` **64.0 ms**. *(2 Marks)*
- **Round 2 (`RTT_m = 32 ms`)**:
  - Error: `|41.0 - 32| = 9.0 ms`.
  - `RTT_s = (0.875 × 41.0) + (0.125 × 32) = 35.875 + 4.0 =` **39.875 ms**.
  - `D = (0.75 × 5.75) + (0.25 × 9.0) = 4.3125 + 2.25 =` **6.5625 ms**.
  - `RTO = 39.875 + 4 × 6.5625 = 39.875 + 26.25 =` **66.125 ms**. *(2 Marks)*
- **Round 3 (`RTT_m = 60 ms`)**:
  - Error: `|39.875 - 60| = 20.125 ms`.
  - `RTT_s = (0.875 × 39.875) + (0.125 × 60) = 34.89 + 7.5 =` **42.39 ms**.
  - `D = (0.75 × 6.5625) + (0.25 × 20.125) = 4.92 + 5.03 =` **9.95 ms**.
  - `RTO = 42.39 + 4 × 9.95 = 42.39 + 39.80 =` **82.19 ms**. *(2 Marks)*

**Summary Table**:

| Round | Sample $RTT_m$ | Error $|RTT_s - RTT_m|$ | Smoothed $RTT_s$ | Deviation $D$ | Calculated $RTO$ |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **Initial** | - | - | 40.0 ms | 5.0 ms | 60.0 ms |
| **Round 1** | 48 ms | 8.0 ms | **41.0 ms** | **5.75 ms** | **64.0 ms** |
| **Round 2** | 32 ms | 9.0 ms | **39.875 ms** | **6.56 ms** | **66.13 ms** |
| **Round 3** | 60 ms | 20.125 ms | **42.39 ms** | **9.95 ms** | **82.19 ms** |

#### Part 2: Why AIMD Converges to Fairness and Efficiency (4 Marks)
- In Chiu & Jain's vector space analysis of two competing flows sharing a bottleneck link of capacity $C$:
  - The **Efficiency Line** is given by $x_1 + x_2 = C$.
  - The **Fairness Line** is given by $x_1 = x_2$ (a 45° line through the origin).
  - The optimal operating point is the intersection of the two lines $(C/2, C/2)$.
- **Additive Increase**: When link is under-utilized, both flows add 1 MSS per RTT. Vector moves along a 45° trajectory parallel to the fairness line ($x_1 + 1, x_2 + 1$).
- **Multiplicative Decrease**: When congestion occurs, both flows cut their rate by half ($x_1 / 2, x_2 / 2$). This projects the operating point directly along a ray toward the origin $(0, 0)$.
- **Convergence**: Every cycle of additive increase followed by multiplicative decrease moves the operating point closer to the **Fairness Line** while oscillating around the **Efficiency Line**.
- **Why AIAD or MIMD Fail**:
  - **AIAD (Additive Increase Additive Decrease)**: Decreases along a 45° line; oscillates in an endless parallel loop without ever moving closer to the fairness line!
  - **MIMD (Multiplicative Increase Multiplicative Decrease)**: Increases along rays from the origin and decreases along the same rays; the flow that started with a larger window maintains an unfair advantage permanently!

**Marking Scheme**:
- 6 Marks for step-by-step arithmetic of Round 1, Round 2, Round 3 RTO values.
- 4 Marks for AIMD mathematical convergence, Chiu & Jain vector justification, and failure of AIAD/MIMD = **Total 10 Marks**.
