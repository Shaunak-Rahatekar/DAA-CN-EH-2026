# End-Semester Examination: Computer Networks (CS3213)

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Duration**: 2 Hours (11:00 AM – 1:00 PM) | **Maximum Marks**: 60  
*Instructions*:
1. Answer **ALL** questions in Section A (Compulsory).
2. Answer any **TWO** questions from Section B.
3. Answer any **TWO** questions from Section C.
4. Draw neat, labeled diagrams and show step-by-step calculations where required.
5. Universal Unicode notation is used throughout.

---

## SECTION A: Short Answer Conceptual Questions (Compulsory)
**Answer all 5 questions. Each question carries 4 marks.** (5 × 4 = 20 Marks)

### Q1. [4 Marks] (Unit I)
(a) Differentiate between **Hub**, **Layer-2 Switch**, and **Router** with respect to OSI layer of operation, collision domains, and broadcast domains. [2M]  
(b) Draw the line encoding waveforms for the bit sequence `1 0 1 1 0 0 1 0` in **Manchester** and **Differential Manchester** encoding. [2M]

### Q2. [4 Marks] (Unit II)
(a) Why is **bit stuffing** used in Data Link layer framing? Stuff the following bit sequence assuming standard flag pattern `01111110`: `011111101111110`. [2M]  
(b) Calculate the minimum number of parity bits required to construct an error-correcting **Hamming Code** for a 7-bit ASCII data character. State where the parity bits are placed. [2M]

### Q3. [4 Marks] (Unit III)
(a) Compare **Pure ALOHA** and **Slotted ALOHA** in terms of vulnerable period, throughput equations, and maximum channel efficiency. [2M]  
(b) In IEEE 802.3 Ethernet, explain why the minimum frame length is restricted to **64 Bytes (512 bits)**. State the governing equation relating transmission time and propagation delay. [2M]

### Q4. [4 Marks] (Unit IV)
(a) For the IP address block `172.16.20.0/22`, determine:
    1. Subnet mask in dotted decimal notation
    2. Network ID
    3. Total number of usable host IP addresses
    4. Directed broadcast address [2M]  
(b) What causes the **Count-to-Infinity** problem in Distance Vector routing? Explain how **Split Horizon** and **Poison Reverse** resolve this issue. [2M]

### Q5. [4 Marks] (Unit V & VI)
(a) Draw the **TCP 3-way handshake** timing sequence. Why is a 2-way handshake inadequate in packet-switched networks? [2M]  
(b) Differentiate between **Recursive** and **Iterative** DNS queries. State the exact DNS Record type used for IPv6 address resolution. [2M]

---

## SECTION B: Medium Analytical & Derivation Questions
**Answer any TWO questions from Q6, Q7, and Q8. Each carries 8 marks.** (2 × 8 = 16 Marks)

### Q6. [8 Marks] (Units I & II)
(a) Compare the **OSI 7-Layer Reference Model** with the **TCP/IP Protocol Suite**. Explain why the OSI model failed commercially while TCP/IP succeeded, and discuss the architectural concept of **Software-Defined Networking (SDN)**. [4M]  
(b) Consider a channel with bandwidth `B = 2 Mbps` and one-way propagation delay `T_prop = 50 ms`. The frame size is `L = 1000 Bytes (8000 bits)`.
    1. Calculate the transmission time `T_trans` and the parameter `a = T_prop / T_trans`.
    2. Calculate the channel utilization/efficiency for **Stop-and-Wait ARQ**.
    3. Determine the minimum sequence number bit length `k` required for **Go-Back-N** and **Selective Repeat** protocols to achieve 100% channel utilization. [4M]

### Q7. [8 Marks] (Units III & IV)
(a) Describe the **CSMA/CD** protocol and the **Binary Exponential Back-off (BEB)** algorithm. If two stations collide for the 3rd time on a 10 Mbps Ethernet network, what is the range of back-off slot times from which they choose? What happens after 16 failed collisions? [4M]  
(b) An organization is granted the address block `130.34.12.64/26`. The organization needs to design **4 subnets** with an equal number of hosts in each subnet.  
    Determine the subnet mask, number of addresses per subnet, number of usable hosts per subnet, and construct a complete table listing the Network ID, Usable IP Range, and Directed Broadcast Address for all 4 subnets. [4M]

### Q8. [8 Marks] (Units V & VI)
(a) Contrast the **TCP Segment Header** (20-60 Bytes) with the **UDP Datagram Header** (8 Bytes). Explain the significance of the `Sequence Number`, `Acknowledgment Number`, `Window Size`, `SYN`, `FIN`, and `RST` fields. [4M]  
(b) Explain the **DHCP DORA Process** (Discover, Offer, Request, Acknowledge) using a message sequence chart with ports and IP addresses. Explain why the `DHCPREQUEST` is broadcast, and define the **T1 (50%)** and **T2 (87.5%)** lease renewal timers. [4M]

---

## SECTION C: Long Numerical & Comprehensive Problems
**Answer any TWO questions from Q9, Q10, and Q11. Each carries 12 marks.** (2 × 12 = 24 Marks)

### Q9. [12 Marks] (Error Detection & Network Layer Routing)
(a) **Cyclic Redundancy Check (CRC)**:
    A sender wants to transmit the data bit sequence `D = 1 1 0 1 0 1 1 0 1 1` using the generator polynomial `G(x) = x⁴ + x + 1` (binary `1 0 0 1 1`).
    1. Show the step-by-step modulo-2 binary division at the sender to compute the CRC checksum bits.
    2. State the transmitted frame `T`.
    3. Show how the receiver verifies the frame if no errors occur.
    4. Suppose the 4th bit from the left is inverted during transmission (`1` becomes `0`). Show the receiver's modulo-2 division and explain how the error is detected. [6M]  
(b) **Dijkstra's Link State Routing**:
    Consider a network of 5 routers `(A, B, C, D, E)` with bidirectional link costs:
    `(A, B) = 3`, `(A, C) = 1`, `(B, C) = 7`, `(B, D) = 5`, `(B, E) = 1`, `(C, D) = 2`, `(D, E) = 7`.
    Using **Dijkstra's Shortest Path First algorithm** with **Node A as the source**, construct the step-by-step iteration table showing permanent set `S`, tentative distances `D(v)`, and predecessors `p(v)`. Draw the resulting Shortest Path Tree and write the final routing table for Router A. [6M]

### Q10. [12 Marks] (Addressing, Fragmentation & Congestion Control)
(a) **IPv4 Datagram Fragmentation**:
    An IPv4 datagram of total size 4000 Bytes (including a standard 20-byte IP header and 3980 bytes of data payload) arrives at a router whose outgoing link has an MTU of 1500 Bytes.
    Determine the number of fragments generated, and for each fragment, compute:
    1. Total Length (in bytes)
    2. Data payload carried (in bytes)
    3. More Fragments (MF) flag
    4. Don't Fragment (DF) flag
    5. Fragment Offset field value. Present your results in a clear table. [6M]  
(b) **TCP Congestion Control (Tahoe vs Reno)**:
    A TCP connection has an initial slow start threshold `ssthresh = 16 MSS` and begins transmission at Round 1 with `cwnd = 1 MSS`.
    - At transmission Round 8, the sender receives a **Triple Duplicate ACK**.
    - At transmission Round 12, a **Retransmission Timeout (RTO)** occurs.
    Construct a complete round-by-round trace table showing `cwnd` values from Round 1 to Round 14 for both **TCP Tahoe** and **TCP Reno**. Clearly indicate the operational phase (Slow Start, Congestion Avoidance, Fast Recovery) and explain the difference between Tahoe and Reno during packet loss recovery. [6M]

### Q11. [12 Marks] (Application Layer & Modern Transport Protocols)
(a) **The QUIC Protocol vs Traditional Web Stack**:
    Explain why traditional `HTTP/2 over TLS over TCP` suffers from OS kernel ossification, middlebox rigidity, high handshake latency, and transport-layer Head-of-Line (HoL) blocking.
    Draw the architectural comparison diagram between the traditional stack and the modern `HTTP/3 over QUIC` stack. Detail the three core innovations of QUIC: 0-RTT connection handshake, elimination of Head-of-Line blocking, and Connection Migration using the 64-bit Connection ID (CID). [6M]  
(b) **Electronic Mail & File Transfer Architectures**:
    1. Draw the complete email architecture showing MUA, MTA, MAA, and mailbox storage. Trace the end-to-end journey of an email from `alice@vit.edu` to `bob@gmail.com`, showing the SMTP submission, DNS MX query, SMTP relay, and IMAP4 retrieval. [4M]
    2. Explain the dual-connection architecture of FTP (Control Port 21 vs Data Port 20). Contrast Active FTP and Passive FTP (`PASV`), explaining why Active FTP fails when the client is located behind a home router with NAT and firewall. [2M]
