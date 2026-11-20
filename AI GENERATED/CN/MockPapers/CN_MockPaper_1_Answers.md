# End-Semester Examination: Computer Networks (CS3213) — Model Answers & Marking Scheme

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Duration**: 2 Hours | **Maximum Marks**: 60  
**Scope**: Complete Solutions and Examiner Rubric for Mock Paper 1

---

## SECTION A: Short Answer Conceptual Questions (20 Marks)

### Q1. [4 Marks]
#### (a) Differentiate between Hub, Layer-2 Switch, and Router. [2M]
- **Hub (Layer 1 - Physical)**: Electrical repeater. Single collision domain, single broadcast domain across all ports. Cannot filter or forward frames selectively.
- **Layer-2 Switch (Layer 2 - Data Link)**: Hardware frame forwarder based on MAC address tables. **Each port is its own separate collision domain**; all ports belong to one single broadcast domain (without VLANs).
- **Router (Layer 3 - Network)**: Packet router based on IP routing tables. **Each interface is its own separate collision domain AND its own separate broadcast domain**.
*(1M for layers/collision domains, 1M for broadcast domains)*

#### (b) Line Encoding Waveforms for `1 0 1 1 0 0 1 0` in Manchester and Differential Manchester. [2M]
- **Bit String**: `1  0  1  1  0  0  1  0`
- **Manchester (IEEE 802.3)**:
  - Transition in the middle of every bit interval.
  - `0` = Low-to-High transition (`_|-`).
  - `1` = High-to-Low transition (`-| _`).
  - Trace: `1 (-|_)`, `0 (_|-)`, `1 (-|_)`, `1 (-|_)`, `0 (_|-)`, `0 (_|-)`, `1 (-|_)`, `0 (_|-)`. *(1M)*
- **Differential Manchester**:
  - Transition in the middle of every bit interval (for clocking).
  - `0` = Transition at the *start* of the bit interval.
  - `1` = *No* transition at the start of the bit interval.
  - Signal inverts only at start when bit is `0`. *(1M)*

---

### Q2. [4 Marks]
#### (a) Bit Stuffing on `011111101111110`. [2M]
- **Rule**: Whenever the sender encounters **five consecutive 1s (`11111`)** in the data payload, it automatically inserts a stuffing `0` immediately after the 5th 1 to prevent accidental collision with the flag pattern `01111110`.
- **Input Bits**: `0 1 1 1 1 1 1 0 1 1 1 1 1 1 0`
- **Stuffed Sequence**:
  - First five 1s: `0 1 1 1 1 1` ➔ insert `0` ➔ `0 1 1 1 1 1 [0] 1 0`
  - Next five 1s: `1 1 1 1 1` ➔ insert `0` ➔ `1 1 1 1 1 [0] 1 0`
  - Result: `0 1 1 1 1 1 0 1 0 1 1 1 1 1 0 1 0` *(2M)*

#### (b) Hamming Code Parity Bits for 7-bit ASCII Data. [2M]
- **Formula**: `2ʳ ≥ m + r + 1` where data bits `m = 7`.
  - If `r = 3`: `2³ = 8 < 7 + 3 + 1 = 11` (Insufficient).
  - If `r = 4`: `2⁴ = 16 ≥ 7 + 4 + 1 = 12` (Valid!).
  - Therefore, **r = 4 parity bits** are required. Total code word length = 7 + 4 = **11 bits**. *(1M)*
- **Bit Placement**: Parity bits are placed strictly at power-of-2 bit positions:
  - `P1` at Position 1 (2⁰)
  - `P2` at Position 2 (2¹)
  - `P4` at Position 4 (2²)
  - `P8` at Position 8 (2³)
  - Data bits occupy positions 3, 5, 6, 7, 9, 10, 11. *(1M)*

---

### Q3. [4 Marks]
#### (a) Pure ALOHA vs Slotted ALOHA. [2M]

| Parameter | Pure ALOHA | Slotted ALOHA |
| :--- | :--- | :--- |
| **Transmission Timing** | Transmit anytime immediately. | Transmit only at discrete slot boundaries. |
| **Vulnerable Period** | `2 × T_frame` | `1 × T_frame` (half of Pure ALOHA). |
| **Throughput Formula** | `S = G · e^(-2G)` | `S = G · e^(-G)` |
| **Maximum Efficiency** | **18.4%** (`1 / 2e`) at `G = 0.5`. | **36.8%** (`1 / e`) at `G = 1.0` (Twice as efficient!). |
*(1M for vulnerable period & equations, 1M for max efficiency)*

#### (b) Minimum Frame Length in CSMA/CD. [2M]
- **Governing Condition**: The frame transmission time `T_trans` must be at least twice the maximum one-way propagation delay:  
  `T_trans ≥ 2 × T_prop` ⟹ `L_min ≥ 2 × T_prop × Bandwidth`. *(1M)*
- **Physical Reason**: The sender must still be actively transmitting bits when the collision signal from the farthest node travels back to it. If a frame is too short, the sender finishes transmitting before the collision notification arrives, wrongly assuming successful delivery and failing to retransmit!
- For 10 Mbps Ethernet over a 2.5 km bus (`T_prop = 25.6 μs`), `2 × T_prop = 51.2 μs`.  
  `L_min = 51.2 μs × 10 Mbps = 512 bits = 64 Bytes`. *(1M)*

---

### Q4. [4 Marks]
#### (a) Subnet Analysis for `172.16.20.0/22`. [2M]
1. **Subnet Mask**: Prefix `/22` has 22 ones ➔ `11111111.11111111.11111100.00000000` = **`255.255.252.0`**. [0.5M]
2. **Network ID**: 3rd octet block size = `256 - 252 = 4`. Multiples of 4: `0, 4, 8, 12, 16, 20...` Since 20 is a multiple of 4, **Network ID = `172.16.20.0`**. [0.5M]
3. **Usable Hosts**: Host bits `h = 32 - 22 = 10`. Total = 2¹⁰ = 1024. **Usable Hosts = 1024 - 2 = 1022 hosts**. [0.5M]
4. **Directed Broadcast Address**: Block spans 20, 21, 22, 23. **Broadcast = `172.16.23.255`**. [0.5M]

#### (b) Count-to-Infinity & Solutions. [2M]
- **Cause**: In Distance Vector routing (Bellman-Ford), routers share tables only with neighbors. When a link fails, routers believe outdated neighbor rumors claiming an alternate path, looping packets and incrementing hop metric by 1 indefinitely until reaching infinity (16 in RIP). *(1M)*
- **Solutions**:
  - **Split Horizon**: Never advertise a route back out the same interface through which it was learned.
  - **Poison Reverse**: Advertise that route back out the same interface with metric = **infinity (16)** to explicitly break loops. *(1M)*

---

### Q5. [4 Marks]
#### (a) TCP 3-Way Handshake & 2-Way Failure. [2M]
- **Handshake**:
  1. `Client ➔ Server`: `SYN=1, Seq=x`
  2. `Server ➔ Client`: `SYN=1, ACK=1, Seq=y, Ack=x+1`
  3. `Client ➔ Server`: `ACK=1, Seq=x+1, Ack=y+1` *(1M)*
- **Why 2-way fails**: If an old duplicate `SYN` delayed in the network arrives at the server after the session closed, a 2-way handshake would cause the server to allocate memory and enter `ESTABLISHED`, creating a **half-open zombie connection**. In a 3-way handshake, the client rejects unexpected `SYN+ACK` with `RST`. *(1M)*

#### (b) Recursive vs Iterative DNS & IPv6 Record. [2M]
- **Recursive**: Client places entire resolution burden on Local DNS; server must return final IP address or error.
- **Iterative**: Local DNS queries Root/TLD servers; queried server returns a referral pointer ("ask server X"). *(1.5M)*
- **IPv6 Record**: **`AAAA`** ("Quad-A") record maps hostname to 128-bit IPv6 address. *(0.5M)*

---

## SECTION B: Analytical & Derivation Questions (Answer any TWO — 16 Marks)

### Q6. [8 Marks]
#### (a) OSI vs TCP/IP & Software-Defined Networking (SDN). [4M]
- **OSI vs TCP/IP**:
  - OSI (7 layers) was designed before implementation; overly complex with redundant functions (presentation/session layers). Failed commercially due to bad timing, bad technology, bad implementations, and bad politics.
  - TCP/IP (4 layers) was implemented and deployed first (ARPANET); robust, simple, and pragmatic. *(2M)*
- **SDN Architecture**:
  - Decouples the **Control Plane** (routing logic, path calculation) from the **Data Plane** (hardware packet forwarding).
  - Traditional routers run both planes locally. SDN centralizes the Control Plane into a software controller (e.g., OpenFlow protocol) communicating with simple hardware packet switches, enabling programmable dynamic traffic engineering. *(2M)*

#### (b) Sliding Window ARQ Numerical. [4M]
- **Given**: `Bandwidth B = 2 Mbps = 2 × 10⁶ bps`, `T_prop = 50 ms = 0.05 s`, `Frame size L = 1000 Bytes = 8000 bits`.
1. **Transmission Time**:  
   `T_trans = L / B = 8000 / (2 × 10⁶) = 0.004 s = 4 ms`.  
   Parameter `a = T_prop / T_trans = 50 ms / 4 ms =` **12.5**. *(1M)*
2. **Stop-and-Wait Utilization**:  
   `Efficiency η = 1 / (1 + 2a) = 1 / (1 + 2 × 12.5) = 1 / 26 ≈` **3.85%** (Throughput = `3.85% × 2 Mbps = 77 kbps`). *(1M)*
3. **Window Sizes for 100% Utilization**:  
   Optimal window `W_opt = 1 + 2a = 1 + 25 = 26 frames`.
   - **Go-Back-N**: Window size `N = 2ᵏ - 1 ≥ 26` ⟹ `2ᵏ ≥ 27` ⟹ **k = 5 bits** (`N = 31`). *(1M)*
   - **Selective Repeat**: Window size `N = 2^(k-1) ≥ 26` ⟹ `2^(k-1) ≥ 26` ⟹ `k - 1 = 5` ⟹ **k = 6 bits** (`N = 32`). *(1M)*

---

### Q7. [8 Marks]
#### (a) CSMA/CD and Binary Exponential Back-Off. [4M]
- **CSMA/CD Mechanism**: Listen before transmitting (carrier sense). If idle, transmit and continuously monitor line. If a collision is detected, immediately abort transmission, broadcast a 32-to-48 bit **Jam Signal**, and initiate back-off. *(1.5M)*
- **BEB Algorithm**:
  - After `n`-th collision, pick random integer `K` from range `[0, 2ᵏ - 1]` where `k = min(n, 10)`.
  - Wait `K × Slot_Time` (where `Slot_Time = 51.2 μs` on 10 Mbps Ethernet).
  - **For 3rd collision (`n = 3`)**: `k = 3`. Range = `[0, 2³ - 1] = [0, 7]`.  
    Stations choose randomly from `{0, 1, 2, 3, 4, 5, 6, 7}`, waiting `0` to `358.4 μs`. *(1.5M)*
  - **After 16 collisions (`n = 16`)**: Transmission is aborted entirely; an error is reported to upper layer (network failure). *(1M)*

#### (b) Subnet Design for `130.34.12.64/26` into 4 Subnets. [4M]
- Initial prefix `/26`. Subnets required = 4 ⟹ borrow `s = 2` bits (`2² = 4`).
- New prefix = `26 + 2 = /28`. Mask = **`255.255.255.240`**.
- Host bits `h = 32 - 28 = 4`. Total IPs = 2⁴ = **16**. Usable hosts = `16 - 2 =` **14**. *(2M)*
- **Subnet Allocation Table**: *(2M)*
  - **Subnet 0**: NetID = `130.34.12.64/28` | Usable Range: `130.34.12.65` to `.78` | Broadcast: `130.34.12.79`
  - **Subnet 1**: NetID = `130.34.12.80/28` | Usable Range: `130.34.12.81` to `.94` | Broadcast: `130.34.12.95`
  - **Subnet 2**: NetID = `130.34.12.96/28` | Usable Range: `130.34.12.97` to `.110` | Broadcast: `130.34.12.111`
  - **Subnet 3**: NetID = `130.34.12.112/28`| Usable Range: `130.34.12.113` to `.126`| Broadcast: `130.34.12.127`

---

### Q8. [8 Marks]
#### (a) TCP vs UDP Headers & Field Explanations. [4M]
- **TCP (20-60B)**: Complex, byte-stream oriented, reliable. Fields: Seq Num, Ack Num, Data Offset, Flags (SYN/FIN/RST/PSH/URG/ACK/ECE/CWR), Window Size, Checksum, Options.
- **UDP (8B fixed)**: Lightweight, message-oriented. 4 fields: Source Port, Dest Port, Length, Checksum. *(1.5M)*
- **Fields**:
  - `Seq Num`: Byte sequence number of first data byte in segment.
  - `Ack Num`: Sequence number of next expected byte (cumulative ACK).
  - `Window Size`: Advertised receive window (`rwnd`) for flow control.
  - `SYN`: Synchronizes sequence numbers during 3-way handshake.
  - `FIN`: Signals sender finished transmission for graceful close.
  - `RST`: Resets connection abruptly upon error or rejection. *(2.5M)*

#### (b) DHCP DORA Process & Lease Timers. [4M]
- **DORA Steps**:
  1. `DISCOVER`: Client broadcasts `0.0.0.0:68 ➔ 255.255.255.255:67` with MAC and xid.
  2. `OFFER`: Server offers IP, subnet mask, gateway, DNS, lease.
  3. `REQUEST`: Client broadcasts acceptance of chosen offer (notifying all servers).
  4. `ACK`: Server confirms lease binding. *(2M)*
- **Timers**:
  - `T1 (50% of lease)`: Client sends unicast `DHCPREQUEST` to original server to extend lease.
  - `T2 (87.5% of lease)`: If original server fails to answer, client broadcasts `DHCPREQUEST` to any available server.
  - `Expiration (100%)`: IP released; restart DORA. *(2M)*

---

## SECTION C: Long Numerical & Comprehensive Problems (Answer any TWO — 24 Marks)

### Q9. [12 Marks]
#### (a) Cyclic Redundancy Check (CRC). [6M]
- **Given**: Data `D = 1 1 0 1 0 1 1 0 1 1` (10 bits), Generator `G(x) = x⁴ + x + 1 ➔ 1 0 0 1 1` (5 bits, degree `r = 4`).
1. **Sender Modulo-2 Division**:
   - Append `r = 4` zeros to D: `1 1 0 1 0 1 1 0 1 1 0 0 0 0`
   - Modulo-2 division by `1 0 0 1 1`:
     ```
     11010110110000 / 10011
     XOR 10011 -> Remainder = 1110 (CRC = 1 1 1 0)
     ```
   - Checksum = `1 1 1 0`. *(2M)*
2. **Transmitted Frame**:
   - `T = D + Checksum = 1 1 0 1 0 1 1 0 1 1 1 1 1 0` (14 bits). *(1M)*
3. **Receiver Verification (No Error)**:
   - Divide `1 1 0 1 0 1 1 0 1 1 1 1 1 0` by `1 0 0 1 1`.
   - Modulo-2 remainder = `0 0 0 0`. **Zero remainder confirms error-free frame**. *(1.5M)*
4. **Error Detection (4th bit inverted: `1 1 0 0 0 1 1 0 1 1 1 1 1 0`)**:
   - Divide corrupted frame by `1 0 0 1 1`.
   - Remainder is non-zero (`≠ 0000`). Frame is rejected! *(1.5M)*

#### (b) Dijkstra Link State Routing Algorithm. [6M]
- **Given Topology**: `(A,B)=3`, `(A,C)=1`, `(B,C)=7`, `(B,D)=5`, `(B,E)=1`, `(C,D)=2`, `(D,E)=7`. Source = `A`.
- **Step-by-Step Execution Table**: *(3M)*

| Step | Node Added | Permanent Set `S` | `D(B), p(B)` | `D(C), p(C)` | `D(D), p(D)` | `D(E), p(E)` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Init** | - | `{ A }` | `3, A` | `1, A` | `∞, -` | `∞, -` |
| **1** | **C** (`D=1`) | `{ A, C }` | `3, A` | **1, A** | `1+2 = 3, C` | `∞, -` |
| **2** | **B** (`D=3`) | `{ A, C, B }` | **3, A** | `1, A` | `3, C` | `3+1 = 4, B` |
| **3** | **D** (`D=3`) | `{ A, C, B, D }`| `3, A` | `1, A` | **3, C** | `4, B` |
| **4** | **E** (`D=4`) | `{ A, C, B, D, E }`| `3, A` | `1, A` | `3, C` | **4, B** |

- **Shortest Path Tree**: `A ➔ C (1)`, `A ➔ B (3)`, `C ➔ D (2)`, `B ➔ E (1)`. *(1.5M)*
- **Final Routing Table for Router A**: *(1.5M)*

| Destination | Cost | Next Hop | Outgoing Interface |
| :---: | :---: | :---: | :---: |
| **B** | 3 | `B` | `eth0` |
| **C** | 1 | `C` | `eth1` |
| **D** | 3 | `C` | `eth1` |
| **E** | 4 | `B` | `eth0` |

---

### Q10. [12 Marks]
#### (a) IPv4 Datagram Fragmentation Numerical. [6M]
- **Given**: Total Length = 4000 Bytes (Header = 20 Bytes, Data = 3980 Bytes). Link MTU = 1500 Bytes.
- **Max Data Payload per Fragment**: `1500 - 20 = 1480 Bytes`.  
  Check 8-byte boundary: `1480 / 8 = 185` (valid integer).
- **Number of Fragments**:
  - Fragment 1: 1480 Bytes payload (leaving 2500 Bytes).
  - Fragment 2: 1480 Bytes payload (leaving 1020 Bytes).
  - Fragment 3: 1020 Bytes payload.
  Total = **3 Fragments**. *(2M)*
- **Fragment Parameter Table**: *(4M — 1M per fragment + offsets)*

| Parameter | Original Datagram | Fragment 1 | Fragment 2 | Fragment 3 |
| :--- | :---: | :---: | :---: | :---: |
| **Total Length** | 4000 Bytes | **1500 Bytes** | **1500 Bytes** | **1040 Bytes** |
| **Data Payload** | 3980 Bytes | 1480 Bytes (0-1479) | 1480 Bytes (1480-2959) | 1020 Bytes (2960-3979) |
| **MF Flag** | 0 | **1** | **1** | **0** |
| **DF Flag** | 0 | **0** | **0** | **0** |
| **Fragment Offset**| 0 | `0 / 8 =` **0** | `1480 / 8 =` **185** | `2960 / 8 =` **370** |

#### (b) TCP Congestion Control Trace (Tahoe vs Reno). [6M]
- **Initial State**: `ssthresh = 16 MSS`, `cwnd = 1 MSS`.
- **Trace Table (Rounds 1 to 14)**: *(4M)*

| Round | cwnd (MSS) | Operational Phase | Events & Calculations |
| :---: | :---: | :--- | :--- |
| **1** | 1 | Slow Start | Starts at 1; doubles each RTT |
| **2** | 2 | Slow Start | 2 < 16 |
| **3** | 4 | Slow Start | 4 < 16 |
| **4** | 8 | Slow Start | 8 < 16 |
| **5** | 16 | Reached ssthresh | `cwnd = 16 = ssthresh` ➔ switch to Congestion Avoidance |
| **6** | 17 | Congestion Avoidance | Additive Increase (+1 MSS) |
| **7** | 18 | Congestion Avoidance | Additive Increase (+1 MSS) |
| **8** | 19 | Congestion Avoidance | **Triple Duplicate ACK occurs at cwnd = 19!** |
| **9** | **9 (Reno)**<br>*(1 in Tahoe)* | **Fast Recovery (Reno)**<br>*(Slow Start in Tahoe)* | `ssthresh = 19 / 2 = 9 MSS`.<br>**Reno**: Sets `cwnd = ssthresh = 9`, stays in CA.<br>**Tahoe**: Drops `cwnd = 1`, enters Slow Start. |
| **10** | 10 (Reno) / 2 (Tahoe) | Congestion Avoidance / SS | Reno: Linear (`9+1=10`). Tahoe: Doubles (`1*2=2`). |
| **11** | 11 (Reno) / 4 (Tahoe) | Congestion Avoidance / SS | Reno: Linear (`10+1=11`). Tahoe: Doubles (`2*2=4`). |
| **12** | 12 (Reno) / 8 (Tahoe) | Congestion Avoidance | **Retransmission TIMEOUT occurs at cwnd = 12!** |
| **13** | **1** | **Slow Start (Both)** | `ssthresh = 12 / 2 = 6 MSS`. **Both drop cwnd = 1 MSS**! |
| **14** | 2 | Slow Start (Both) | Doubles (`1*2 = 2 < 6`). |

- **Tahoe vs Reno Explanation**: Upon 3 duplicate ACKs, Tahoe drops `cwnd = 1` requiring slow recovery. Reno enters **Fast Recovery**, setting `cwnd = ssthresh` and skipping slow start to preserve high throughput. *(2M)*

---

### Q11. [12 Marks]
#### (a) QUIC Protocol vs Traditional Web Stack. [6M]
- **Limitations of TCP+TLS+HTTP/2**:
  - *Kernel Ossification*: Upgrading TCP takes a decade due to OS kernel dependencies.
  - *Middlebox Rigidity*: Firewalls drop novel TCP options.
  - *High Latency*: TCP (1 RTT) + TLS 1.3 (1 RTT) = 2 RTTs before HTTP data.
  - *Head-of-Line Blocking*: Losing 1 TCP packet halts delivery of all multiplexed HTTP/2 streams! *(2M)*
- **Stack Comparison**: Traditional: `HTTP/2 ➔ TLS 1.3 ➔ TCP ➔ IP`. Modern: `HTTP/3 ➔ QUIC (built-in TLS 1.3) ➔ UDP ➔ IP`. *(1M)*
- **Three QUIC Innovations**:
  1. *0-RTT Handshake*: Merges transport and TLS into 1 RTT (or 0-RTT for returning clients).
  2. *No HoL Blocking*: Streams are independent at Layer 4; loss on Stream A never blocks Stream B.
  3. *Connection Migration*: 64-bit Connection ID (CID) survives Wi-Fi to 5G IP changes seamlessly. *(3M)*

#### (b) Email Journey & FTP Dual Connections. [6M]
1. **End-to-End Email Journey (`alice@vit.edu` to `bob@gmail.com`)**:
   - Alice MUA connects to `mail.vit.edu:587` via SMTP submission (`EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`, `<CRLF>.<CRLF>`).
   - `vit.edu` MTA queries DNS for `MX` record of `gmail.com` to find Google's mail exchanger IP.
   - `vit.edu` MTA opens TCP Port 25 to Google MTA and relays the message.
   - Google MTA stores email in Bob's mailbox. Bob retrieves email using IMAP4 (`Port 993 SSL`), which synchronizes mail and folders across all his devices. *(4M)*
2. **FTP Dual Connections & Firewall Issue**:
   - FTP uses **Control Port 21** (commands, persistent) and **Data Port 20** (file data, dynamic).
   - In Active Mode, the server initiates an incoming connection to the client's data port, which client NAT firewalls block.
   - In Passive Mode (`PASV`), the client initiates the connection to an ephemeral port opened by the server, traversing client firewalls seamlessly. *(2M)*
