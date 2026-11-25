# Vishwakarma Institute of Technology, Pune
## Third Year B.Tech - Department of Computer Engineering
### Course: Computer Networks (Subject Code: CS3052)
### End Semester Examination (ESE) - Last Year Paper (Nov 2025) - Fully Solved
> **Exam Date:** Tuesday, 25 November 2025 | Time: 04:00 PM to 06:00 PM (2 Hours)  
> **Max. Marks:** 60 | Total Questions: 6 (10 Marks each)  
> **Target Department:** Computer Engineering (Official Department Paper)  
> **Formatting:** 100% universal clean ASCII text, exact mathematical derivations, and marking rubrics.

---

# QUESTION 1 [CO1: Fundamentals & Physical Layer] (Compulsory, 10 Marks)

---

### Q.1.A: Line Coding: Manchester & Differential Manchester for `010011` [5 Marks]
**Question Statement:**
> Design the encoding of Manchester and Differential Manchester for the sequence of data 010011.

**Model Answer:**

**1. Encoding Rules:**
* **Manchester Encoding (IEEE 802.3 Standard):**
  - Bit `0`: High-to-Low transition (`+V -> -V`) in the middle of the bit interval.
  - Bit `1`: Low-to-High transition (`-V -> +V`) in the middle of the bit interval.
* **Differential Manchester Encoding:**
  - There is **always a transition in the middle** of every bit interval (provides clock synchronization).
  - Bit `0`: Extra transition at the **beginning** of the bit interval.
  - Bit `1`: **No transition** at the beginning of the bit interval.
  - (Assume initial reference voltage starts at `+V`).

**2. Bit-by-Bit Transition Table for `010011`:**

| Bit Index | Bit Value | Manchester Transition (Mid-bit) | Differential Manchester Transition | Half-Bit Levels |
| :---: | :---: | :---: | :---: | :---: |
| 1 | **0** | High-to-Low (`+V -> -V`) | Start flip (`+V -> -V`), Mid flip (`-V -> +V`) | `(-, +)` |
| 2 | **1** | Low-to-High (`-V -> +V`) | No start flip (`+V`), Mid flip (`+V -> -V`) | `(+, -)` |
| 3 | **0** | High-to-Low (`+V -> -V`) | Start flip (`-V -> +V`), Mid flip (`+V -> -V`) | `(+, -)` |
| 4 | **0** | High-to-Low (`+V -> -V`) | Start flip (`-V -> +V`), Mid flip (`+V -> -V`) | `(+, -)` |
| 5 | **1** | Low-to-High (`-V -> +V`) | No start flip (`-V`), Mid flip (`-V -> +V`) | `(-, +)` |
| 6 | **1** | Low-to-High (`-V -> +V`) | No start flip (`+V`), Mid flip (`+V -> -V`) | `(+, -)` |

**3. ASCII Signal Waveforms:**
```
Bit Pattern:    |   0   |   1   |   0   |   0   |   1   |   1   |

Manchester:
  +V            |---+   |   +---|---+   |---+   |   +---|   +---|
   0            |   |   |   |   |   |   |   |   |   |   |   |   |
  -V            |   +---|---+   |   +---|   +---|---+   |---+   |

Diff Manch:
  +V            |   +---|---+   |---+   |---+   |   +---|---+   |
   0            |   |   |   |   |   |   |   |   |   |   |   |   |
  -V            |---+   |   +---|   +---|   +---|---+   |   +---|
```

**Examiner Marking Rubric:**
* Stating correct encoding conventions: 1.0 Mark.
* Accurate Manchester mid-bit waveform: 2.0 Marks.
* Accurate Differential Manchester edge transitions: 2.0 Marks.

---

### Q.1.B: OSI 7-Layer Model: PDUs, Transmission/Receiving Logic & Data Sizes [5 Marks]
**Question Statement:**
> Construct the OSI reference model and specify the data type and protocols at each layer with brief information about each layer. Also apply the logic for the transmission and receiving of data at each layer and comment on the size of data at each layer.

**Model Answer:**

**1. Layer Functions, PDUs, and Protocols:**

| Layer Number & Name | Core Functionality | PDU (Data Unit) | Standard Protocols |
| :--- | :--- | :--- | :--- |
| **7. Application Layer** | Direct network services to end-user programs | **Message / Data** | HTTP, SMTP, DNS, FTP, DHCP |
| **6. Presentation Layer**| Data translation, encryption/decryption, compression | **Message** | TLS/SSL, JPEG, MPEG, ASCII |
| **5. Session Layer** | Dialog management, checkpoints, token management | **Message** | RPC, NetBIOS, PPTP |
| **4. Transport Layer** | End-to-end process-to-process delivery, flow/error control | **Segment / Datagram**| TCP, UDP, SCTP |
| **3. Network Layer** | Host-to-host routing, logical IP addressing | **Packet** | IPv4, IPv6, ICMP, ARP, OSPF |
| **2. Data Link Layer** | Hop-to-hop node transfer, MAC addressing, framing, CRC | **Frame** | IEEE 802.3 Ethernet, Wi-Fi, PPP |
| **1. Physical Layer** | Raw bit stream transmission over physical media | **Bits / Signals** | RS-232, 1000BASE-T, V.35 |

**2. Transmission & Reception Logic (Encapsulation vs De-encapsulation):**
* **Transmitting (Down the Stack):** Application data passes down. Each layer prepends its own control header (Transport adds port numbers; Network adds IP addresses; Data Link adds MAC addresses and appends a CRC trailer). Physical layer converts bits to electromagnetic signals.
* **Receiving (Up the Stack):** Physical layer reconstructs bits into frames. Data link verifies CRC and strips MAC header; Network verifies IP destination and strips IP header; Transport demultiplexes via port number and reassembles stream; Application displays message.

**3. Data Size Growth Across Layers (Example for 1000 Bytes Payload):**
```
Layer               Header Added                    Resulting Size
Application         None                            1000 Bytes
Transport (TCP)     +20 Bytes TCP Header            1020 Bytes (Segment)
Network (IPv4)      +20 Bytes IPv4 Header           1040 Bytes (Packet)
Data Link (Ethernet)+14 B MAC Header + 4 B CRC      1058 Bytes (Frame)
Physical            +8 Bytes Preamble + SFD         1066 Bytes = 8528 Bits on wire
```
* **Comment on Size:** Data grows going down (protocol overhead) and shrinks going up. If a packet exceeds the link MTU (1500 B on Ethernet), it is fragmented into multiple packets.

**Examiner Marking Rubric:**
* Complete 7-layer architecture with PDUs and protocols: 2.0 Marks.
* Transmission (down) and receiving (up) logic: 1.5 Marks.
* Layer-by-layer size calculation table and MTU comment: 1.5 Marks.

---

# QUESTION 2 [CO2: Logical Link Control & Framing] (Compulsory, 10 Marks)

---

### Q.2.A: CRC Error Detection for Data `10100001` with Generator `1001` [5 Marks]
**Question Statement:**
> Examine the given data to transfer and find out the data to be sent, prove that you have sent correct data using CRC method. Data given is = 10100001 if the generator polynomial in binary is = 1001.

**Model Answer:**

**1. Sender Side (FCS Generation):**
* Data $D = 10100001$ ($k = 8$ bits).
* Generator Divisor $G = 1001$ (degree $r = 4 - 1 = 3$ bits).
* Append $r = 3$ zeros to $D$: Dividend = `10100001000` (11 bits).
* Perform Modulo-2 Division (XOR, no borrows):
```
Quotient : 10111111
 10100001000  <- Data + 3 zeros
 1001         <- XOR with divisor
 ----
 00110001000
   1001
   ----
   010101000
    1001
    ----
    00111000
      1001
      ----
      011100
       1001
       ----
       01110
        1001
        ----
        0111  <- Remainder (3 bits)
```
* **Remainder FCS** = **`111`**.
* **Transmitted Frame $T$** = Data + FCS = **`10100001111`**.

**2. Receiver Side (Correctness Verification):**
* Divide received frame `10100001111` by generator `1001`:
```
Quotient : 10111111
 10100001111  <- Received Codeword
 1001
 ----
 00110001111
   1001
   ----
   010101111
    1001
    ----
    00111111
      1001
      ----
      011011
       1001
       ----
       01001
        1001
        ----
        0000  <- Remainder is strictly ZERO!
```
* **Conclusion:** Since **Remainder = `000`**, the frame is accepted as 100% error-free. Proved!

**Examiner Marking Rubric:**
* Appending 3 zeros and showing division steps: 2.0 Marks.
* Correct remainder `111` and transmitted frame `10100001111`: 1.5 Marks.
* Receiver check showing zero remainder proof: 1.5 Marks.

---

### Q.2.B: Point-to-Point Protocol (PPP) Evaluation & Byte Stuffing [5 Marks]
**Question Statement:**
> Evaluate PPP protocol with neat diagram of frame format and bringing line up and down. Also give at least one reason why PPP uses byte stuffing instead of bit stuffing.

**Model Answer:**

**1. PPP Frame Format:**
```
+----------+----------+----------+--------------------+-------------+-----------+----------+
| Flag     | Address  | Control  | Protocol           | Payload     | FCS (CRC) | Flag     |
| 7E (1 B) | FF (1 B) | 03 (1 B) | 2 B (IP, LCP, PAP) | <= 1500 B   | 2 or 4 B  | 7E (1 B) |
+----------+----------+----------+--------------------+-------------+-----------+----------+
```

**2. Bringing the Line Up and Down (Phase Transition Lifecycle):**
```
  [DEAD] --(Carrier Detected)--> [ESTABLISH] (LCP Option Negotiation)
                                      |
                                      v
                                [AUTHENTICATE] (PAP / CHAP Credentials)
                                      |
                                      v
                                  [NETWORK] (NCP / IPCP Assigns IP)
                                      |
                                      v
                                   [OPEN] (Payload Data Transfer)
                                      |
                                      v
                                 [TERMINATE] (LCP Closes Connection)
                                      |
                                      v
                                   [DEAD]
```

**3. Why PPP Uses Byte Stuffing Instead of Bit Stuffing:**
* PPP was primarily architected for **character-oriented asynchronous communication links** (RS-232 serial ports, modems, UARTs) which naturally read and write data in **entire 8-bit bytes**, not individual bits.
* Byte stuffing (inserting escape byte `0x7D` followed by the original byte XORed with `0x20`) operates with negligible CPU overhead on byte-stream hardware, whereas bit stuffing requires complex bit-shifting and hardware clock synchronization.

**Examiner Marking Rubric:**
* Clean PPP frame format diagram: 1.5 Marks.
* 5-phase line lifecycle (Dead -> Establish -> Authenticate -> Network -> Open -> Terminate): 2.0 Marks.
* Technical justification for byte stuffing over bit stuffing: 1.5 Marks.

---

# QUESTION 3 [CO3: Flow Control, Pipelining & MAC] (Compulsory, 10 Marks)

---

### Q.3.A: File Transfer Latency Calculation for 1.5 MB File [5 Marks]
**Question Statement:**
> Calculate the total time required to transfer a 1.5 MB file in following cases assuming a RTT of 80 ms, a packet size of 1 KB data, and an initial 2*RTT of handshaking before data is sent:
> A) the bandwidth is 10 Mbps, and data packets can be sent continuously
> B) the bandwidth is 10 Mbps, but after we finish sending each data packet we must wait one RTT before sending the next.
> C) link allows infinitely fast transmit, but limit the bandwidth such that only 20 packets can be sent per RTT.

**Model Answer:**

**1. Baseline Conversion:**
* File size = $1.5 \text{ MB} = 1.5 \times 1024 \times 1024 \text{ bytes} = 1,572,864 \text{ bytes} = 12,582,912 \text{ bits}$.
* Packet size = $1 \text{ KB} = 1024 \text{ bytes} = 8192 \text{ bits}$.
* Total Packets = $1,572,864 / 1024 = \mathbf{1536 \text{ packets}}$.
* Initial Handshake Time = $2 \times \text{RTT} = 2 \times 80 \text{ ms} = \mathbf{160 \text{ ms} = 0.160 \text{ s}}$.
* One-way propagation delay $T_p = \text{RTT} / 2 = 40 \text{ ms} = 0.040 \text{ s}$.

**2. Case A: Continuous Transmission at 10 Mbps:**
* Total transmission time: $T_{\text{trans}} = \text{Total Bits} / \text{Bandwidth} = 12,582,912 / 10,000,000 = 1.258 \text{ s}$.
* Since packets are sent continuously without waiting, the total time is:
  $\text{Total Time} = \text{Handshake} + T_{\text{trans}} + T_p = 0.160 + 1.258 + 0.040 = \mathbf{1.458 \text{ seconds}}$.
  *(Or if teacher's set counts $1.5 \text{ MB} = 1.5 \times 10^6 \text{ B} = 12 \text{ Mb} \implies 1.20 \text{ s} \implies 1.40 \text{ s}$)*.

**3. Case B: Wait One RTT After Each Packet (Stop-and-Wait Style):**
* After each of the 1536 packets, the sender waits 1 RTT ($80$ ms) for an ACK, except after the last packet which only takes $T_p$ to arrive.
* Number of intermediate RTT waits = $1536 - 1 = 1535$ waits.
* Total wait time = $1535 \times 0.080 \text{ s} = 122.80 \text{ s}$.
* $\text{Total Time} = 1.458 \text{ s} + 122.80 \text{ s} = \mathbf{124.26 \text{ seconds}}$ (~85 times slower!).

**4. Case C: Infinitely Fast Transmit, Paced at 20 Packets per RTT:**
* Transmission time $T_{\text{trans}} \approx 0$.
* Number of RTT rounds required = $1536 / 20 = 76.8 \implies 77 \text{ rounds}$.
* After 76 full rounds, 1520 packets have been transferred. The 77th round sends the remaining 16 packets and takes 1 RTT (or $0.5 \text{ RTT}$ for one-way delivery).
* Total RTTs = $2 \text{ (handshake)} + 76 + 0.5 = 78.5 \text{ RTTs}$.
* $\text{Total Time} = 78.5 \times 0.080 \text{ s} = \mathbf{6.28 \text{ seconds}}$.

**Examiner Marking Rubric:**
* Unit conversions and packet count (1536 packets): 1.0 Mark.
* Case A correct derivation ($1.458$ s): 1.5 Marks.
* Case B correct derivation ($124.26$ s): 1.5 Marks.
* Case C correct derivation ($6.28$ s): 1.0 Mark.

---

### Q.3.B: Collision-Free Protocols OR Minimum Frame Size in Fast Ethernet [5 Marks]
*(Option 1: Collision-Free Protocols)*
**Question Statement:**
> Construct a collision free protocol for 1. A Bit-Map Protocol, 2. Binary Countdown.

**Model Answer:**
1. **Bit-Map Protocol (Reservation Protocol):**
   - The channel operates in repeating cycles. Each cycle begins with a **Contention Period** of $N$ bit-slots for $N$ stations (numbered 0 to $N-1$).
   - If Station $j$ wants to transmit a frame, it transmits a `1` bit during slot $j$. All stations listen and record the reservation bit-map.
   - After slot $N-1$, stations that asserted a `1` transmit their full frames sequentially in ascending order. Since reservations are pre-declared, **zero collisions occur!**
2. **Binary Countdown Protocol:**
   - Stations wanting to transmit broadcast their binary station addresses bit-by-bit from Most Significant Bit (MSB) to Least Significant Bit (LSB).
   - The channel performs a **Wired-OR** of all broadcast bits.
   - If a station has a `0` in a bit position but senses a `1` on the channel, it recognizes that a higher-numbered station is competing and immediately drops out.
   - The station with the highest binary address wins and transmits its frame with zero collisions.

*(Option 2: Minimum Frame Size in Fast Ethernet)*
**Question Statement:**
> Ethernet Frame must be at least 64 bytes long to ensure that the transmitter is still going in the event of collision at the far end of the cable. Fast ethernet has the same 64 byte minimum frame size but can get the bits out ten times faster. How is it possible?

**Model Answer:**
* **The CSMA/CD Rule:** $T_{\text{trans}} \ge 2 T_p \implies L_{\text{min}} / B \ge 2 d / v \implies d_{\text{max}} = \frac{L_{\text{min}} \cdot v}{2 B}$.
* In Standard Ethernet (10 Mbps), $L_{\text{min}} = 64$ bytes (512 bits) gives a slot time of $51.2$ us, allowing a maximum network diameter of **2500 meters** (with 4 repeaters).
* In Fast Ethernet (100 Mbps), the bandwidth $B$ increased by a factor of 10. To maintain the exact same 64-byte minimum frame size for backward compatibility, the transmission time shrinks to $5.12$ us ($10\times$ shorter).
* **How it is Possible:** The maximum allowable cable length (network diameter) was **reduced by a factor of 10**! Fast Ethernet cuts the collision domain diameter from 2500 meters down to **~200 meters** (100 meters per UTP link to a central switch). By shrinking the distance, $T_p$ shrinks by $10\times$, preserving $T_{\text{trans}} \ge 2 T_p$ without changing the frame format!

**Examiner Marking Rubric:**
* For Option 1: Bit-Map diagram/explanation (2.5M) + Binary countdown logic (2.5M).
* For Option 2: Formula $L_{\text{min}} = 2 T_p B$ (2.0M) + Explanation of shrinking network diameter by $10\times$ (3.0M).

---

# QUESTION 4 [CO4: Network Layer & Subnetting] (Compulsory, 10 Marks)

---

### Q.4.A: ISP Customer Subblock Address Allocation on 190.100.0.0/16 [5 Marks]
**Question Statement:**
> An ISP is granted a block of addresses starting with 190.100.0.0/16. The ISP needs to distribute these addresses to three groups of customers as follows:
> a. The first group has 64 customers: each needs 256 address
> b. The second group has 128 customers: each needs 128 address
> c. The third group has 128 customers: each needs 64 addresses
> Design the subblocks and find out how many addresses are still available after these allocations.

**Model Answer:**

**1. Sizing and Prefix Determination (Largest Blocks First):**
* ISP Total Block: `190.100.0.0/16` $\implies 2^{16} = \mathbf{65,536 \text{ addresses}}$.
* **Group 1 (64 Customers $\times$ 256 addresses):**
  - Addresses per customer = $256 = 2^8 \implies \mathbf{/24}$ prefix (`255.255.255.0`).
  - Total addresses used = $64 \times 256 = \mathbf{16,384 \text{ addresses}}$ (equivalent to a `/18` block).
  - Subnet Range: `190.100.0.0/24` to `190.100.63.255/24` (Third octet: 0 to 63).
* **Group 2 (128 Customers $\times$ 128 addresses):**
  - Addresses per customer = $128 = 2^7 \implies \mathbf{/25}$ prefix (`255.255.255.128`).
  - Total addresses used = $128 \times 128 = \mathbf{16,384 \text{ addresses}}$ (equivalent to a `/18` block).
  - Subnet Range: `190.100.64.0/25` to `190.100.127.255/25` (Third octet: 64 to 127).
* **Group 3 (128 Customers $\times$ 64 addresses):**
  - Addresses per customer = $64 = 2^6 \implies \mathbf{/26}$ prefix (`255.255.255.192`).
  - Total addresses used = $128 \times 64 = \mathbf{8,192 \text{ addresses}}$ (equivalent to a `/19` block).
  - Subnet Range: `190.100.128.0/26` to `190.100.159.255/26` (Third octet: 128 to 159).

**2. Address Availability Calculation:**
* **Total Addresses Allocated**:
  $\text{Total Allocated} = 16,384 + 16,384 + 8,192 = \mathbf{40,960 \text{ addresses}}$.
* **Addresses Still Available**:
  $\text{Available} = 65,536 - 40,960 = \mathbf{24,576 \text{ addresses}}$ (equivalent to a `/19` + `/20` free block).
* **Available Unallocated Range**: `190.100.160.0` to `190.100.255.255`.

**Examiner Marking Rubric:**
* Allocation plan for Group 1 (/24 blocks, range .0.0 to .63.255): 1.5 Marks.
* Allocation plan for Group 2 (/25 blocks, range .64.0 to .127.255): 1.0 Mark.
* Allocation plan for Group 3 (/26 blocks, range .128.0 to .159.255): 1.0 Mark.
* Correct available address count (24,576) and free range: 1.5 Marks.

---

### Q.4.B: Distance Vector Routing Table for Node C [5 Marks]
**Question Statement:**
> Consider that a network graph has A-B-C-D-E and F nodes. Connected by non-directional edges with metrics: A-B=4, B-C=2, C-D=3, A-E=5, E-F=8, F-D=7, B-F=6, C-E=1. Use Distance Vector Routing and measure new metrics for node C. If C received vectors from B: (5,0,8,12,6,2), from D: (16,12,6,0,9,10), from E: (7,6,3,9,0,4). Measured delays to B, D, E are 6, 3, 5 respectively. What is C's new routing table?

**Model Answer:**

**1. Given Link Costs from Router C to its Neighbors:**
* $c(C, B) = 6$
* $c(C, D) = 3$
* $c(C, E) = 5$

**2. Bellman-Ford Metric Calculation ($D_C(X) = \min [c(C,B)+D_B(X),\; c(C,D)+D_D(X),\; c(C,E)+D_E(X)]$):**

| Dest | Delay via B ($6 + D_B$) | Delay via D ($3 + D_D$) | Delay via E ($5 + D_E$) | Minimum Cost | Next Hop |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **A** | $6 + 5 = \mathbf{11}$ | $3 + 16 = 19$ | $5 + 7 = 12$ | **11** | **B** |
| **B** | $6 + 0 = \mathbf{6}$ | $3 + 12 = 15$ | $5 + 6 = 11$ | **6** | **B** |
| **C** | Direct = 0 | Direct = 0 | Direct = 0 | **0** | **-** |
| **D** | $6 + 12 = 18$ | $3 + 0 = \mathbf{3}$ | $5 + 9 = 14$ | **3** | **D** |
| **E** | $6 + 6 = 12$ | $3 + 9 = 12$ | $5 + 0 = \mathbf{5}$ | **5** | **E** |
| **F** | $6 + 2 = \mathbf{8}$ | $3 + 10 = 13$ | $5 + 4 = 9$ | **8** | **B** |

**3. Router C's New Routing Table:**
```
Destination:   A   B   C   D   E   F
Delay:        11   6   0   3   5   8
Next Hop:      B   B   -   D   E   B
```

*(Note: C completely ignores its own previous table when recomputing from incoming neighbor vectors).*

**Examiner Marking Rubric:**
* Complete step-by-step vector arithmetic for all 6 destinations: 3.0 Marks.
* Clean final routing table with next hops: 2.0 Marks.

---

# QUESTION 5 [CO5: Transport Layer Protocols] (Compulsory, 10 Marks)

---

### Q.5.A: TCP Connection Establishment: 3-Way Handshake [5 Marks]
**Question Statement:**
> Construct the TCP protocol Connection establishment using three way of handshaking.

**Model Answer:**

**1. Three-Way Handshake Protocol Exchange:**
```
       CLIENT (Host A)                                         SERVER (Host B)
              |                                                       |
   [CLOSED]   |                                                       |  [LISTEN]
              |                                                       |
   (SYN_SENT) | ---------- (1) SYN, Seq = x ------------------------> |
              |                (MSS, Window Scale options)            |  (SYN_RCVD)
              |                                                       |
              | <--------- (2) SYN + ACK, Seq = y, Ack = x + 1 ------ |
 [ESTABLISHED]|                                                       |
              |                                                       |
              | ---------- (3) ACK, Seq = x + 1, Ack = y + 1 -------->| [ESTABLISHED]
              |                                                       |
              | =========== Bi-directional Data Transfer ===========> |
```

**2. Detailed Step Descriptions:**
1. **Segment 1 (SYN):** Client chooses random Initial Sequence Number (ISN $x$), sets SYN flag, and advertises its receive window. (State: `SYN_SENT`).
2. **Segment 2 (SYN + ACK):** Server acknowledges client's sequence number ($x + 1$), picks its own random ISN $y$, sets SYN and ACK flags, and allocates connection buffers. (State: `SYN_RCVD`).
3. **Segment 3 (ACK):** Client acknowledges server's sequence number ($y + 1$). Both enter `ESTABLISHED` state. Data payload may now be carried.
* **Why 3-Way (Not 2-Way):** Prevents obsolete delayed duplicate SYN segments from past zombie sessions from accidentally establishing phantom connections.

**Examiner Marking Rubric:**
* Clean timeline exchange diagram with SYN, ACK, and ISNs: 2.5 Marks.
* Step-by-step description of states (`SYN_SENT`, `SYN_RCVD`, `ESTABLISHED`): 1.5 Marks.
* Justification of ISN synchronization and duplicate rejection: 1.0 Mark.

---

### Q.5.B: TCP Transmission Policy & Silly Window Syndrome Solutions [5 Marks]
**Question Statement:**
> Justify that the TCP transmission policy is useful for the Window management and give solutions to the silly window syndrome.

**Model Answer:**

**1. TCP Window Management & Transmission Policy:**
TCP utilizes a sliding window mechanism where the receiver advertises its available buffer space via the **16-bit Window (`rwnd`) field**.
* **Usable Window Formula:** $\text{Usable Window} = \text{rwnd} - (\text{LastByteSent} - \text{LastByteAcked})$.
* The sender is strictly throttled by the receiver's processing capacity, preventing buffer overflow and packet drops.

**2. The Silly Window Syndrome (SWS):**
SWS occurs when data is exchanged in **ridiculously tiny micro-segments** (e.g., 1 byte of user data wrapped in 40 bytes of TCP/IP header, resulting in **< 3% efficiency**).
* *Causes*:
  - **Receiver-induced SWS**: Slow application reads 1 byte at a time from socket buffer; receiver immediately advertises `rwnd = 1`, prompting sender to transmit a 1-byte segment.
  - **Sender-induced SWS**: Application writes small character bytes (e.g. telnet keystrokes) into socket buffer, and sender transmits each byte immediately.

**3. Solutions to Silly Window Syndrome:**
1. **Clark's Solution (Receiver-Side):**
   - The receiver is forbidden from advertising tiny window increments.
   - The receiver must advertise `rwnd = 0` until it can accommodate **at least one Maximum Segment Size (MSS)** or **half of its total receive buffer space** ($	ext{Buffer} / 2$).
2. **Nagle's Algorithm (Sender-Side - RFC 896):**
   - The sender transmits the first small chunk immediately.
   - While waiting for the ACK of that segment, the sender **buffers all subsequent outgoing data**.
   - The sender only transmits the buffered data when:
     - It accumulates **at least one full MSS** of data, OR
     - An **ACK for the previously transmitted segment arrives**.

**Examiner Marking Rubric:**
* Explanation of TCP sliding window transmission policy: 1.5 Marks.
* Description of Silly Window Syndrome problem: 1.5 Marks.
* Clark's solution (Receiver-side) & Nagle's algorithm (Sender-side): 2.0 Marks.

---

# QUESTION 6 [CO6: Application Layer Protocols] (Compulsory, 10 Marks)

---

### Q.6.A: Domain Name System (DNS) Architecture & Resolution [5 Marks]
**Question Statement:**
> Discuss in detail about DNS.

**Model Answer:**

**1. Overview & Architecture:**
* DNS (Domain Name System - RFC 1034/1035) is a globally distributed, hierarchical database that resolves human-readable hostnames (e.g., `www.vit.edu`) into 32-bit/128-bit IP addresses. Runs over **UDP Port 53** (falls back to TCP for responses > 512 bytes).
* **Hierarchical Tree Structure**:
```
                        Root Servers (.)
                               |
         +---------------------+---------------------+
         | generic TLDs                              | country-code TLDs
       .com, .edu, .org, .gov                      .in, .uk, .us
         |
      vit.edu (Second-Level Domain)
         |
      www.vit.edu (Host)
```

**2. Query Resolution Types:**
* **Recursive Query:** Client asks Local DNS Resolver to find the complete answer. The resolver assumes full responsibility.
* **Iterative Query:** Resolver contacts root, TLD, and authoritative servers in sequence. Each server returns a referral pointer to the next lower-level server.

**3. Core Resource Record (RR) Types:**
* **A Record:** Maps hostname to IPv4 address (`vit.edu -> 115.111.x.x`).
* **AAAA Record:** Maps hostname to 128-bit IPv6 address.
* **CNAME (Canonical Name):** Aliases one name to another (`www -> webserver1`).
* **MX Record:** Specifies mail exchange servers for email delivery.
* **PTR Record:** Pointer record used for Reverse DNS (IP to hostname).

**Examiner Marking Rubric:**
* DNS hierarchical namespace tree diagram: 2.0 Marks.
* Recursive vs Iterative resolution explanation: 1.5 Marks.
* Key Resource Records (A, AAAA, CNAME, MX, PTR): 1.5 Marks.

---

### Q.6.B: Multipurpose Internet Mail Extensions (MIME) Protocol [5 Marks]
**Question Statement:**
> Identify the protocol used to support non-ASCII data in email applications and brief about the same.

**Model Answer:**

**1. Protocol Identification: MIME (RFC 2045 - 2049)**
* Traditional SMTP (RFC 821) is strictly restricted to **7-bit NVT ASCII characters**.
* If a sender transmits binary attachments (images, PDFs, audio/video) or non-English characters, legacy SMTP servers strip the 8th bit, irreversibly corrupting the file.
* **MIME (Multipurpose Internet Mail Extensions)** extends SMTP to carry arbitrary binary content seamlessly.

**2. How MIME Operates:**
1. **At Sender (MUA):** The user agent encodes binary bytes into safe 7-bit ASCII text using **Base64 Encoding** (which groups 3 binary bytes [24 bits] into 4 printable ASCII characters [6 bits each], adding a ~33% overhead).
2. **In Transit:** Standard SMTP servers relay the 7-bit ASCII stream without corruption.
3. **At Receiver (MUA):** The recipient's email client decodes Base64 text back into the original binary file.

**3. Essential MIME Headers:**
* `MIME-Version: 1.0`
* `Content-Type: image/jpeg` (or `multipart/mixed` for multiple attachments).
* `Content-Transfer-Encoding: base64` (indicates encoding format).
* `Content-Disposition: attachment; filename="network_diagram.png"`.

**Examiner Marking Rubric:**
* Identification of MIME: 1.0 Mark.
* Explanation of SMTP 7-bit ASCII limitation: 1.5 Marks.
* Base64 encoding mechanism: 1.5 Marks.
* Core MIME header fields: 1.0 Mark.
