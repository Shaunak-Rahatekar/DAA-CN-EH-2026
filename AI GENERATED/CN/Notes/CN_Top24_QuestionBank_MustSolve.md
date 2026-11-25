# Computer Networks (CS3052 / CS3213 / IT3231)
## The Official Google Drive Question Bank: Top 24 Master Hit-List
### Priority *** Very Important Questions (Repeated 2 to 6 Times in Semester Papers)

> **Source**: Official VIT Pune CN Question Bank (82 Solved Drive Questions).  
> **Selection Criteria**: Strictly the **24 Questions marked '*** Very important'**, having the highest repetition count across May 2018, Dec 2018, May 2019 TY/TE IT, Dec 2022, Nov 2023, and Nov 2024.  
> **Golden Strategy for Tonight**: Master these 24 questions first. In a 60-mark paper with internal choice, these 24 questions account for over **85% of the total examinable marks**.  
> **Formatting**: 100% clean universal ASCII text, exact teacher's numerical solutions, formulas, and examiner marking rubrics.

---

# UNIT 1: NETWORKING FUNDAMENTALS & PHYSICAL LAYER

---

### HIT 1 [Q1 in QB]: Manchester & Differential Manchester Line Encoding (5M)
**Repetition**: Repeated 2 times (Nov 2024, Dec 2022) | Topic: Line Coding
* **Question**: Design the Manchester and Differential Manchester encoding for the data sequence `010011` (also asked for `010010110110`).
* **Conventions (Write first in exam)**:
  - **Manchester (IEEE 802.3)**: `0` = High-to-Low transition at mid-bit (`+V -> -V`), `1` = Low-to-High transition at mid-bit (`-V -> +V`).
  - **Differential Manchester**: Always a transition in the middle of each bit interval. `0` = Extra transition at the start of the bit interval; `1` = No transition at the start. Assume line starts at `+V`.
* **Bit-by-Bit Transitions for `010011`**:
  - Bit 1 (`0`): Start flip (`+V -> -V`), mid flip (`-> +V`). Levels: `(-, +)`
  - Bit 2 (`1`): No start flip (`+V`), mid flip (`-> -V`). Levels: `(+, -)`
  - Bit 3 (`0`): Start flip (`-> +V`), mid flip (`-> -V`). Levels: `(+, -)`
  - Bit 4 (`0`): Start flip (`-> +V`), mid flip (`-> -V`). Levels: `(+, -)`
  - Bit 5 (`1`): No start flip (`-V`), mid flip (`-> +V`). Levels: `(-, +)`
  - Bit 6 (`1`): No start flip (`+V`), mid flip (`-> -V`). Levels: `(+, -)`
* **Why Used**: Mid-bit transition provides self-synchronization; equal time at `+V` and `-V` gives zero DC component. Disadvantage: double bandwidth of NRZ.

---

### HIT 2 [Q2 in QB]: LAN vs WAN Architectural Comparison & The Internet as a WAN (6M)
**Repetition**: Repeated 2 times (Nov 2024, Nov 2023 Set-I) | Topic: Network Types & Architectures
* **Comparative Summary Table**:

| Parameter | LAN (Campus Ethernet, Wi-Fi) | WAN (ISP Backbone, Internet) |
| :--- | :--- | :--- |
| **Line Encoding** | Manchester (10BASE-T), 4B/5B (100BASE-TX) | B8ZS/HDB3 on T1/E1, scrambled NRZ, 64B/66B on optics |
| **Modulation** | Baseband (no carrier) on copper; OFDM on Wi-Fi | Carrier modulation: QAM, QPSK, DWDM on optical fiber |
| **Topology** | Star around central switches | Partial mesh of routers joined by point-to-point links |
| **Physical Media** | UTP Cat 5e/6 (<= 100 m), multimode fiber | Single-mode fiber, leased lines, submarine cables |
| **Data Rates** | 100 Mbps to 10/40/100 Gbps, low delay, cheap/bit | Shared, costly per bit, high delay across long spans |

* **Layers that Make the Internet a WAN**: The **Network / Internet Layer (IP)** is key. IP provides a single unified global logical address space, allowing routers to interconnect thousands of heterogeneous local networks.
* **Why Our College Network is an Intranet**: Uses identical Internet technologies (TCP/IP, HTTP, DNS) but is private: uses private IP addresses (`10.x.x.x`) behind NAT and firewalls, with services (ERP, Moodle) restricted to students and staff.

---

### HIT 3 [Q3 in QB]: SONET Optical Carrier (OC-1 to OC-768) Speed Derivation (6M)
**Repetition**: Repeated 2 times (Nov 2023 Set-I, Teacher's Solved Sets) | Topic: Physical Media & Optical Carriers
* **Question**: Show with manual calculation that fiber-optic OC-1 provides 51.84 Mbps. How do OC-256 and OC-768 give 13.271 Gbps and 40 Gbps?
* **Derivation for OC-1 (STS-1)**:
  - SONET frame: 9 rows x 90 columns = **810 bytes** per frame.
  - Frame transmission rate = **8000 frames/sec** (sent every 125 us, matching standard voice sampling).
  - OC-1 Rate = 810 bytes x 8 bits/byte x 8000 frames/s = 6,480 bits x 8000 = **51,840,000 bps = 51.84 Mbps**.
* **Formula for OC-n**: Speed = n x 51.84 Mbps.
  - **OC-256**: 256 x 51.84 Mbps = 13,271.04 Mbps = **13.271 Gbps**.
  - **OC-768**: 768 x 51.84 Mbps = 39,813.12 Mbps = **39.813 Gbps (~40 Gbps)**.
* **Campus Application**: Campus departments connect to server rooms via Gigabit / 10G Ethernet (1000BASE-SX or 10GBASE-SR) over multimode fiber, while the telecom ISP uses OC-n links for the external Internet leased-line uplink.

---

# UNIT 2: DATA LINK LAYER, FRAMING & FLOW CONTROL

---

### HIT 4 [Q10 in QB]: Flow Control Pipe Capacity & Sequence Bits Calculation (4M)
**Repetition**: Repeated 3 times (Nov 2023 Set-I, Dec 2022, Teacher's Sets) | Topic: Flow Control
* **Given**: Frame size $L = 1000$ bits, propagation delay $T_p = 10$ ms, bandwidth $R = 20$ Mbps.
* **Step-by-step Math**:
  1. Transmission time: $T_t = L / R = 1000 / (20 \times 10^6) = 50$ us.
  2. Ratio $a$: $a = T_p / T_t = 10 \text{ ms} / 50 \text{ us} = 200$.
  3. Packets on pipe (one-way): $T_p / T_t = 200$ frames. In full round-trip: $2a = 400$ frames in flight.
  4. Window size for 100% efficiency: $W = 1 + 2a = 1 + 400 = \mathbf{401 \text{ frames}}$.
  5. Sequence Number Bits:
     - **Go-Back-N**: $2^m - 1 \ge 401 \implies 2^m \ge 402 \implies \mathbf{m = 9 \text{ bits}}$ ($2^9 = 512$).
     - **Selective Repeat**: $2^{m-1} \ge 401 \implies 2^m \ge 802 \implies \mathbf{m = 10 \text{ bits}}$ ($2^{10} = 1024$).

---

### HIT 5 [Q11 in QB]: Point-to-Point Protocol (PPP) Architecture vs HDLC (5M)
**Repetition**: Repeated 3 times (Nov 2023 Set-I, Dec 2018, May 2019 TY) | Topic: WAN Protocols
* **PPP Frame Format**:
```
+----------+----------+----------+--------------------+-------------+-----------+----------+
| Flag     | Address  | Control  | Protocol           | Payload     | FCS (CRC) | Flag     |
| 7E (1 B) | FF (1 B) | 03 (1 B) | 2 B (IP, LCP, PAP) | <= 1500 B   | 2 or 4 B  | 7E (1 B) |
+----------+----------+----------+--------------------+-------------+-----------+----------+
```
* **Why Byte Stuffing**: PPP was designed for byte-oriented modem serial links. If flag byte `7E` appears in data, it is escaped with `7D` followed by the original byte XORed with `0x20` (`7E -> 7D 5E`).
* **Why PPP is More Popular than HDLC**:
  1. Multi-protocol support (Protocol field supports IP, IPX, AppleTalk).
  2. **LCP (Link Control Protocol)** negotiates MTU and link quality dynamically.
  3. Authentication built-in (**PAP** and **CHAP**), essential for ISP billing.
  4. Automatic IP configuration via **NCP / IPCP**.

---

### HIT 6 [Q12 in QB]: CRC Modulo-2 Division & Destination Verification (9M)
**Repetition**: Repeated 3 times (Nov 2024, Dec 2018, Teacher's Sets) | Topic: Error Detection
* **Problem**: $M(x) = x^7 + x^6 + x^5 + x^2 + x$ (`11100110`), Generator $G(x) = x^4 + x^3 + 1$ (`11001`, degree $r=4$).
* **Step-by-step Math**:
  1. Append $r=4$ zeros: `111001100000`.
  2. Modulo-2 XOR division by `11001` yields **Remainder FCS = `0110`**.
  3. Transmitted Codeword: `11100110` + `0110` = **`111001100110`**.
  4. Receiver Check: Dividing `111001100110` by `11001` gives **Remainder = `0000`** $\implies$ Accepted without error!

---

### HIT 7 [Q13 in QB]: Delay-Bandwidth Product & Bit Length in Distance (4M)
**Repetition**: Repeated 2 times (Nov 2024, Teacher's Sets) | Topic: Network Performance
* **Problem**: Speed $v = 2.3 \times 10^8$ m/s, $R = 56$ kbps, distance $d = 10$ km.
* **Math**:
  - $T_p = 10,000 / (2.3 \times 10^8) = 43.48$ us $\implies \text{RTT} = 2 T_p = 86.96$ us.
  - **Delay \times Bandwidth**: $56,000 \times 86.96 \times 10^{-6} = \mathbf{4.87 \text{ bits}}$ (less than 1 byte in flight!).
  - **1 Bit in Time**: $t = 1 / R = 1 / 56,000 = \mathbf{17.86 \text{ us}}$.
  - **1 Bit in Length**: $\text{Length} = v \times t = (2.3 \times 10^8) \times (17.86 \times 10^{-6}) = \mathbf{4,107 \text{ meters} \approx 4.11 \text{ km}}$.

---

### HIT 8 [Q14 in QB]: Hamming Code Error Detection & Correction (6M)
**Repetition**: Repeated 2 times (Dec 2022, Teacher's Sets) | Topic: Error Correction
* **Problem**: Test received 12-bit word `011100101110` (even parity). Correct if wrong.
* **Positions (1 to 12 from left)**: Parity at 1, 2, 4, 8; Data at 3, 5, 6, 7, 9, 10, 11, 12.
* **Syndrome Bit Calculations**:
  - $c_1$ (1,3,5,7,9,11): bits are 0,1,0,1,1,1 $\implies$ four 1s (Even) $\implies c_1 = 0$.
  - $c_2$ (2,3,6,7,10,11): bits are 1,1,0,1,1,1 $\implies$ five 1s (Odd) $\implies c_2 = 1$.
  - $c_4$ (4,5,6,7,12): bits are 1,0,0,1,0 $\implies$ two 1s (Even) $\implies c_4 = 0$.
  - $c_8$ (8,9,10,11,12): bits are 0,1,1,1,0 $\implies$ three 1s (Odd) $\implies c_8 = 1$.
* **Syndrome**: $c_8 c_4 c_2 c_1 = 1010_2 = \mathbf{10}$.
* **Correction**: **Bit 10 is wrong!** Flip bit 10 from 1 to 0.
* **Corrected Codeword**: **`011100101010`**. Original data (positions 3,5,6,7,9,10,11,12) = **`10011010`**.

---

# UNIT 3: MEDIUM ACCESS CONTROL & WIRELESS STANDARDS

---

### HIT 9 [Q31 in QB]: CSMA/CA vs CSMA/CD, Hidden & Exposed Terminals (8M)
**Repetition**: Repeated 4 times (Nov 2024, Nov 2023 Set-I, May 2019 TY, May 2019 TE IT)
* **Why CSMA/CD Fails on Radio**: A wireless transceiver cannot listen while transmitting (outgoing signal is orders of magnitude stronger than any incoming signal), and signal attenuation hides collisions at the receiver.
* **CSMA/CA Mechanics (802.11 DCF)**: Sense idle $\to$ wait DIFS $\to$ random backoff $\to$ **RTS/CTS exchange** $\to$ data transmission $\to$ SIFS $\to$ ACK.
* **Hidden Terminal Problem**: Station A and C can both reach B, but cannot hear each other. Both transmit to B simultaneously $\implies$ collision at B.  
  *Fix*: B's **CTS** packet silences C (sets C's **Network Allocation Vector / NAV**).
* **Exposed Terminal Problem**: B transmits to A. C wants to send to D. C hears B and needlessly waits, even though C sending to D would not interfere with A.  
  *Fix*: RTS/CTS lets C hear B's RTS but not A's CTS, allowing C to transmit safely.

---

### HIT 10 [Q32 in QB]: Wi-Fi Evolution: Wi-Fi 4 vs Wi-Fi 5 vs Wi-Fi 6 (6M)
**Repetition**: Repeated 3 times (Nov 2024, Nov 2023 Set-I, Dec 2022)
* **Comparative Evolution**:

| Feature | Wi-Fi 4 (802.11n, 2009) | Wi-Fi 5 (802.11ac, 2013) | Wi-Fi 6 (802.11ax, 2019) |
| :--- | :--- | :--- | :--- |
| **Frequency Bands** | 2.4 GHz & 5 GHz | 5 GHz only (cleaner spectrum) | 2.4 GHz & 5 GHz (Wi-Fi 6E adds 6 GHz) |
| **Channel Width** | 20 / 40 MHz | 20 / 40 / 80 / 160 MHz | 20 to 160 MHz (split into OFDMA RUs) |
| **Modulation** | 64-QAM (6 bits/symbol) | 256-QAM (8 bits/symbol, +33%) | 1024-QAM (10 bits/symbol, +25%) |
| **MIMO Capabilities**| Up to 4x4 Single-User MIMO | Downlink MU-MIMO (4 clients) | Uplink & Downlink MU-MIMO (8 clients)|
| **Multiplexing** | OFDM (1 user per channel) | OFDM | **OFDMA** (multi-user subcarrier sharing)|
| **Max PHY Rate** | 600 Mbps | 6.9 Gbps | 9.6 Gbps |
| **High-Density Tech**| None | Standardized Beamforming | **BSS Coloring** + Target Wake Time (TWT)|

---

### HIT 11 [Q33 in QB]: Minimum Frame Size in Gigabit Ethernet & Full Duplex Justification (4M)
**Repetition**: Repeated 2 times (Nov 2024, Nov 2023 Set-I)
* **Problem**: 1 Gbps building network over 1 km cable ($v = 200,000$ km/s).
  - $T_p = 1 \text{ km} / (200,000 \text{ km/s}) = 5$ us.
  - $L_{\text{min}} = 2 \times T_p \times B = 2 \times (5 \times 10^{-6}) \times 10^9 = \mathbf{10,000 \text{ bits} = 1,250 \text{ bytes}}$.
  - If $L_{\text{min}} = 2500$ bytes ($20,000$ bits) $\implies T_t = 20$ us $\implies T_p \le 10$ us $\implies d_{\text{max}} = 10 \text{ us} \times (2 \times 10^8 \text{ m/s}) = \mathbf{2 \text{ km}}$.
* **Why 64-Byte Minimum Frame Still Exists in Full Duplex**:
  Full-duplex switched Ethernet has no collisions (CSMA/CD is disabled), but the 64-byte minimum is retained for:
  1. **Backward Compatibility**: Allows full-duplex ports to fall back to half-duplex hubs seamlessly.
  2. **Runt Detection**: Frames under 64 bytes are immediately discarded by switches as damaged fragments.
  3. **Hardware Standardization**: Switch buffers and ASIC parsing logic are permanently tuned to 64-byte minimum slot times.

---

# UNIT 4: NETWORK LAYER & IP ADDRESSING

---

### HIT 12 [Q40 in QB]: QoS & Congestion in Datagram vs Virtual-Circuit Subnets & Jitter (6M)
**Repetition**: Repeated 4 times (Nov 2024, Nov 2023 Set-I, Dec 2018, May 2018)
* **Datagram (IP) vs Virtual-Circuit (MPLS/ATM)**:
  - *Datagram Subnet*: No connection setup; packets routed independently; routers store no flow state; cannot perform admission control $\implies$ congestion control is purely reactive (dropping packets).
  - *Virtual-Circuit Subnet*: Explicit setup phase; bandwidth and buffers reserved in advance; admission control blocks new flows if congested $\implies$ congestion is prevented proactively.
* **Jitter**: Variation in packet arrival delays (e.g. packet 1 arrives in 20 ms, packet 2 in 60 ms). Stutters real-time audio/video.
* **Mitigation**: Receiver **Playout (Jitter) Buffer** that delays rendering until packets arrive steadily, sequenced using RTP timestamps.

---

### HIT 13 [Q41 in QB]: CIDR Subnetting Derivations: 167.199.170.82/27 & /25 (8M)
**Repetition**: Repeated 5 times (Dec 2018, May 2018, May 2019 TY, May 2019 TE IT, Teacher's Sets)
* **For `167.199.170.82/27`**:
  - Host bits: $32 - 27 = 5$ bits $\implies$ Block size = $2^5 = 32$.
  - Subnet mask: 27 ones $\implies$ `255.255.255.224`.
  - Block index: $82 / 32 = 2$ remainder 18 $\implies$ Starts at $2 \times 32 = 64$.
  - **First Address (Network ID)**: `167.199.170.64`.
  - **Last Address (Broadcast)**: $64 + 31 = \mathbf{167.199.170.95}$.
  - **Usable Host Range**: `167.199.170.65` to `167.199.170.94` (30 usable hosts).
* **For `167.199.130.83/25`**:
  - Block size = $2^7 = 128$. Subnet mask = `255.255.255.128`.
  - Range: `167.199.130.0` to `167.199.130.127` (Usable: `.1` to `.126`).
* **Prefixes for Small Blocks**: 2 hosts $\to$ `/31`, 4 hosts $\to$ `/30`, 8 hosts $\to$ `/29`, 16 hosts $\to$ `/28`.

---

### HIT 14 [Q42 in QB]: VIT Campus Address Allocation on 10.21.0.0/16 (8M)
**Repetition**: Repeated 3 times (Nov 2023 Set-I, Dec 2018, Teacher's Sets)
* **Requirements**: 3 departments needing 8192 addresses each:
  - Computer: 32 labs x 256 addresses = 8192 addresses (allocate `/24` blocks).
  - Chemical: 64 labs x 128 addresses = 8192 addresses (allocate `/25` blocks).
  - Mechanical: 128 labs x 64 addresses = 8192 addresses (allocate `/26` blocks).
* **Allocation Plan**:
  - *Computer*: `10.21.0.0/24` to `10.21.31.255/24` (Subnet range `10.21.0.0 - 10.21.31.255`).
  - *Chemical*: `10.21.32.0/25` to `10.21.63.255/25` (Subnet range `10.21.32.0 - 10.21.63.255`).
  - *Mechanical*: `10.21.64.0/26` to `10.21.95.255/26` (Subnet range `10.21.64.0 - 10.21.95.255`).
* **Total Used**: $3 \times 8192 = 24,576$ addresses.
* **Available Free Space**: $65,536 - 24,576 = \mathbf{40,960 \text{ addresses}}$ (`10.21.96.0` to `10.21.255.255`).

---

### HIT 15 [Q43 in QB]: VLSM 5-Department Allocation on 217.20.12.0/24 (8M)
**Repetition**: Repeated 3 times (Dec 2022, May 2018, Teacher's Sets)
* **Requirements**: 1 large department (110 hosts) and 4 small departments (< 25 hosts each).
* **VLSM Sizing (Largest First)**:
  - Large (110 hosts): $110 + 2 = 112 \le 128 \implies \mathbf{/25}$ block (`255.255.255.128`).
  - 4 Small (< 25 hosts): $25 + 2 = 27 \le 32 \implies \mathbf{/27}$ block (`255.255.255.224`).
* **Subnet Assignment**:
  - Large Dept: `217.20.12.0/25` (Range: `.0` to `.127`, Usable: `.1` to `.126`).
  - Small Dept 1: `217.20.12.128/27` (Range: `.128` to `.159`).
  - Small Dept 2: `217.20.12.160/27` (Range: `.160` to `.191`).
  - Small Dept 3: `217.20.12.192/27` (Range: `.192` to `.223`).
  - Small Dept 4: `217.20.12.224/27` (Range: `.224` to `.255`).
* **Proof CIDR Reduces Wastage**: Fits all 5 departments into a single `/24` (256 addresses). Classful addressing would require 5 separate Class C licenses (1280 addresses), wasting over 1000 IPs!

---

### HIT 16 [Q44 in QB]: 7-Department Address Chart on 202.202.0.0/16 for 504 Nodes (4M)
**Repetition**: Repeated 2 times (Nov 2024, Teacher's Sets)
* **Teacher's Solved Method**: 7 departments $\implies 2^3 = 8 \ge 7 \implies$ borrow 3 bits from third octet $\implies \mathbf{/19}$ (`255.255.224.0`).
  - Each department gets an entire `/19` block (step size of 32 in 3rd octet):
    Dept 1: `202.202.0.0/19`, Dept 2: `202.202.32.0/19`, Dept 3: `202.202.64.0/19`, Dept 4: `202.202.96.0/19`, Dept 5: `202.202.128.0/19`, Dept 6: `202.202.160.0/19`, Dept 7: `202.202.192.0/19`, Spare: `202.202.224.0/19`.
* **Tighter Extra-Credit Alternative**: Each dept needs $504 / 7 = 72$ nodes $\implies /25$ (126 usable hosts). Uses only 1024 addresses total instead of entire `/16`.

---

# UNIT 5: TRANSPORT LAYER PROTOCOLS & TIMERS

---

### HIT 17 [Q61 in QB]: UDP Operation Without Flow/Error Control & Buffer Management (4M)
**Repetition**: Repeated 6 times (Nov 2024, Nov 2023, Dec 2022, Dec 2018, May 2018, May 2019 TY - HIGHEST RECORD!)
* **How UDP Works Without Flow/Error Control**: Fixed 8-byte header (Src Port, Dst Port, Length, Checksum). No handshake, no ACKs, no sequence numbers, no retransmissions. Damaged datagrams failing the checksum are **silently discarded**.
* **Buffer Management (TCP vs UDP)**:
  - *TCP*: Keeps send buffer (data held until ACKed) and receive buffer (holds out-of-order bytes); free buffer space is advertised as `rwnd`.
  - *UDP*: Single FIFO receive queue per socket. Datagrams are placed in queue; if queue fills up, incoming datagrams are **silently dropped**. No send buffer maintained.

---

### HIT 18 [Q62 in QB]: The Four Critical TCP Timers (8M)
**Repetition**: Repeated 3 times (Nov 2024, Dec 2022, May 2018)
* **Comprehensive Timers Breakdown**:
  1. **Retransmission Timer (RTO)**: Started when segment is sent. Value: $\text{RTO} = \text{SRTT} + 4 \times \text{RTTVAR}$ (Jacobson's algorithm). Doubled upon each timeout (Karn's rule: no RTT samples from retransmissions).
  2. **Persistence Timer**: Started when receiver announces window size = 0. Prevents deadlock if window-update packet is lost. Sender periodically sends a **1-byte Window Probe** to query current buffer status.
  3. **Keepalive Timer**: Typically set to 2 hours of inactivity. Sends 10 probe segments spaced 75 seconds apart to detect crashed or unreachable clients and reclaim half-open server resources.
  4. **Time-Wait Timer (2MSL)**: Runs for twice the Maximum Segment Lifetime (e.g. 60 to 120 seconds) on the side that initiated active close. Ensures final ACK reaches receiver and allows delayed duplicate packets to expire before port reuse.

---

# UNIT 6: APPLICATION LAYER SERVICES & PROTOCOLS

---

### HIT 19 [Q72 in QB]: Persistent vs Non-Persistent HTTP & RTT Calculations (8M)
**Repetition**: Repeated 5 times (Nov 2024, Dec 2022, Dec 2018, May 2018, Teacher's Sets)
* **Counting Rule (Base HTML + $n$ Embedded Objects)**:
  - Non-Persistent (HTTP/1.0): $\mathbf{2(n + 1) \text{ RTT}}$. (1 RTT TCP handshake + 1 RTT request/response per object).
  - Persistent Non-Pipelined (HTTP/1.1): $\mathbf{n + 2 \text{ RTT}}$. (1 RTT TCP handshake + 1 RTT base + $n$ sequential RTTs).
  - Persistent Pipelined (HTTP/1.1): $\mathbf{3 \text{ RTT}}$. (1 RTT TCP handshake + 1 RTT base + 1 RTT parallel request for all $n$ objects).
* **Solved Examples Table**:

| Objects ($n$) | Non-Persistent (HTTP/1.0) | Persistent Non-Pipelined | Persistent Pipelined |
| :---: | :---: | :---: | :---: |
| **5 Objects** | $2(5 + 1) = \mathbf{12 \text{ RTT}}$ | $5 + 2 = \mathbf{7 \text{ RTT}}$ | **3 RTT** |
| **12 Objects** | $2(12 + 1) = \mathbf{26 \text{ RTT}}$ | $12 + 2 = \mathbf{14 \text{ RTT}}$ | **3 RTT** |
| **50 Objects** (RTT = 10 ms) | $102 \text{ RTT} = \mathbf{1.02 \text{ s}}$ | $52 \text{ RTT} = \mathbf{520 \text{ ms}}$ | $3 \text{ RTT} = \mathbf{30 \text{ ms}}$ |
| **100 Objects** | $202 \text{ RTT}$ | $102 \text{ RTT}$ | **3 RTT** |

---

### HIT 20 [Q73 in QB]: DNS Hierarchical Resolution of `www.vit.edu` (6M)
**Repetition**: Repeated 5 times (Dec 2022, Dec 2018, May 2018, May 2019 TY, May 2019 TE IT)
* **Architecture**: Distributed tree database. Port 53 UDP.
* **Resolution Steps (Recursive to Local Resolver, Iterative Upstream)**:
```
  [Laptop] <--(1) Recursive Request-- [Local DNS Resolver]
     |     <--(8) IP 115.111.x.x ----       |
     |                                      +--(2) Iterative Query--> [Root Server .]
     |                                      <--(3) Referral to .edu--
     |                                      |
     |                                      +--(4) Iterative Query--> [.edu TLD Server]
     |                                      <--(5) Referral to vit.edu-
     |                                      |
     |                                      +--(6) Iterative Query--> [vit.edu Authoritative]
     |                                      <--(7) A Record (IP) ----
```

---

### HIT 21 [Q74 in QB]: DHCP DORA Process & Mobile Laptop IP Allocation (8M)
**Repetition**: Repeated 5 times (Dec 2022, Dec 2018, May 2018, May 2019 TY, May 2019 TE IT)
* **The 4-Way DORA Handshake (UDP 67 Server, UDP 68 Client)**:
  1. **Discover**: Client broadcasts `DHCPDISCOVER` (`Src: 0.0.0.0, Dst: 255.255.255.255`).
  2. **Offer**: Server broadcasts `DHCPOFFER` offering unallocated IP, subnet mask, gateway, lease time.
  3. **Request**: Client broadcasts `DHCPREQUEST` confirming selection of the offered IP.
  4. **Acknowledge**: Server broadcasts `DHCPACK` committing the binding to its lease database.
* **Mobile Laptops**: Roaming laptops run DORA on every new subnet to acquire a temporary lease; renew lease at 50% timer ($T_1$) and rebind at 87.5% ($T_2$).

---

### HIT 22 [Q75 in QB]: Electronic Mail: POP3 vs IMAP & Webmail (6M)
**Repetition**: Repeated 3 times (Nov 2024, Nov 2023 Set-I, May 2018)
* **Comparative Summary**:
  - *POP3 (Port 110)*: **Download-and-delete** model. Mail downloaded to single client device and purged from server. Unsuitable for users with phones and laptops.
  - *IMAP (Port 143)*: **Server-side synchronization** model. All emails, folders, and read/unread flags remain centrally on server. Allows multi-device sync and partial header downloads.
* **ISP Advice**: Offer **IMAP over TLS** by default to support multi-device customers; offer POP3 only as an archival option.
* **Webmail Protocol**: Browser communicates with webmail server over **HTTP/HTTPS**; webmail server retrieves mail from mailbox via IMAP and transmits outbound mail via SMTP.

---

### HIT 23 [Q76 in QB]: FTP Dual Connection Architecture (Port 20 vs Port 21) (4M)
**Repetition**: Repeated 3 times (Nov 2023 Set-I, May 2018, May 2019 TE IT)
* **Out-of-Band Design**:
  - **Control Connection (Port 21)**: Stays open for whole session; carries user commands (`USER`, `PASS`, `QUIT`) and status replies.
  - **Data Connection (Port 20)**: Ephemeral TCP connection opened on-demand for each file transfer (`RETR`, `STOR`) or directory listing (`LIST`), and closed immediately upon transfer completion.
* **Does it Make FTP Faster?**: No (opening new TCP sockets adds RTT overhead), but it prevents command traffic from being blocked behind huge file payloads.

---

### HIT 24 [Q77 in QB]: DNS Over UDP Packet Loss Recovery (6M)
**Repetition**: Repeated 2 times (Nov 2024, Nov 2023 Set-I)
* **The Problem**: DNS uses UDP port 53 (connectionless, no transport retransmissions).
* **How Problem is Solved at Application Layer**:
  1. **Resolver Timeout & Retransmit**: Local DNS resolver sets a timer; if no reply arrives within timeout, it resends query with exponential backoff.
  2. **Alternate Server Failover**: If primary server is unresponsive, resolver queries secondary authoritative server.
  3. **16-Bit Transaction ID**: Matches arriving replies to outstanding queries, rejecting delayed duplicates.
  4. **TCP Fallback**: If response exceeds 512 bytes, server sets **TC (Truncation) bit**; resolver repeats query over **TCP port 53**.
