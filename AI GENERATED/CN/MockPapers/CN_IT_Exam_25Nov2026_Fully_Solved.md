# Vishwakarma Institute of Technology, Pune
## Third Year B.Tech - Department of Information Technology
### Course: Computer Networks (Subject Code: IT3231)
### End Semester Examination (ESE) - Fully Solved Paper
> **Exam Conducted:** Wednesday, 25 November 2026 | Time: 03:00 PM to 05:00 PM (2 Hours)  
> **Max. Marks:** 60 | Total Questions: 6 (10 Marks each)  
> **Relevance to CS Examination (26 Nov 2026):** **CRITICAL GOLDMINE**. In VIT Pune autonomous curriculum, IT and CS share identical syllabus, overlapping paper-setting committees, and identical Question Bank pools. Every question on this paper is an ultra-high-probability forecast for tomorrow's exam.  
> **Formatting:** 100% universal clean ASCII text with full analytical derivations and marking rubrics.

---

# QUESTION 1 [CO1: Fundamentals, Architecture & Topologies] (Solve any 2 of 3)

---

### Q.1.A: Applying TCP/IP Model over OSI Model for Web Streaming Service [5 Marks]
**Question Statement:**
> Demonstrate how you would apply the TCP/IP Reference Model instead of the OSI Model when architecting the protocol stack for a real-world web streaming service.

**Model Answer:**

**1. Architectural Mapping for Web Video Streaming:**
Modern web video streaming (e.g., Netflix, YouTube, Hotstar) utilizes **HTTP Live Streaming (HLS)** or **Dynamic Adaptive Streaming over HTTP (DASH)** for on-demand playback, and **WebRTC / RTSP** for low-latency live streaming.

```
+-------------------------------------------------------------------------+
| TCP/IP Model Layer        | Real-World Streaming Protocol Stack         |
+---------------------------+---------------------------------------------+
| 4. Application Layer      | HLS (m3u8 manifests + TS/fMP4 video chunks) |
|                           | MPEG-DASH / WebRTC Media Engine             |
+---------------------------+---------------------------------------------+
| 3. Transport Layer        | TCP (for reliable chunk delivery via HTTPS) |
|                           | or UDP + QUIC (for low-latency HTTP/3)      |
+---------------------------+---------------------------------------------+
| 2. Internet Layer         | IPv4 / IPv6 (Global packet routing & QoS)   |
+---------------------------+---------------------------------------------+
| 1. Network Access Layer   | 10/100G Ethernet, 5G NR, Wi-Fi 6 (802.11ax) |
+-------------------------------------------------------------------------+
```

**2. Why the TCP/IP Model is Superior to the OSI Model for Web Streaming:**
1. **Elimination of Redundant Layer Overhead:**
   - OSI separates Presentation (Layer 6) and Session (Layer 5) as distinct protocol layers.
   - In modern streaming, video decoding (H.264/AV1) and session tokens are handled directly within the browser or application runtime.
   - Forcing independent Layer 5/6 headers would add processing latency and bandwidth overhead to millions of concurrent micro-chunks.
2. **Native Transport Flexibility:**
   - TCP/IP allows the streaming architect to seamlessly choose between **TCP** (loss-intolerant, pre-buffered VoD) and **UDP / QUIC** (delay-sensitive, real-time live sports) without altering the upper application logic.
3. **HTTP/CDN Compatibility:**
   - TCP/IP's pragmatic Application Layer aligns directly with standard HTTP/HTTPS caching infrastructure (Cloudflare, Akamai CDNs), allowing streaming servers to scale horizontally to millions of concurrent viewers.

**Examiner Marking Rubric:**
* Protocol mapping table (Application, Transport, Internet, Network Access): 2.0 Marks.
* Technical justification of eliminating OSI Layers 5 and 6 overhead: 1.5 Marks.
* Real-world protocol integration (HLS/DASH, CDN, TCP vs UDP): 1.5 Marks.

---

### Q.1.B: Enterprise Network Hierarchy: LAN, MAN, and WAN Application [5 Marks]
**Question Statement:**
> A retail corporation operates a head office in a 3-story building, 10 outlets across the same metropolitan area, and a cloud backup server in another state. Justify how LAN, MAN, and WAN technologies are applied at each tier to connect these locations into a single enterprise network.

**Model Answer:**

**1. Three-Tier Architectural Design:**
```
  [Head Office (3-Story)]                      [10 Retail Outlets (City-Wide)]
   +---------------------+                      +-----------------------------+
   | LAN (Cat6A / Wi-Fi) |                      | Local LANs per outlet       |
   +----------+----------+                      +--------------+--------------+
              |                                                |
              +---------------> [ METROPOLITAN AREA NETWORK ] <-+
                                [ (MAN: Metro Ethernet/Fiber) ]
                                               |
                                               v
                                [ WIDE AREA NETWORK (WAN) ]
                                [ (IPsec VPN / MPLS Link) ]
                                               |
                                               v
                                [ Cloud Backup (Other State) ]
```

**2. Layer-by-Layer Justification:**

* **Tier 1: Local Area Network (LAN) - Head Office & Outlets**
  - *Coverage*: Single building / individual retail shop floor (< 100 meters).
  - *Technology*: Gigabit Ethernet (IEEE 802.3ab 1000BASE-T, Cat6A copper cabling) and enterprise Wi-Fi 6 (IEEE 802.11ax).
  - *Justification*: Provides high data transfer rates (1 Gbps - 10 Gbps), sub-millisecond local latency, and high security for internal POS terminals, inventory scanners, and office workstations.

* **Tier 2: Metropolitan Area Network (MAN) - City-Wide Interconnection**
  - *Coverage*: 10 retail outlets distributed across the metropolitan city (1 km - 50 km).
  - *Technology*: Metro Ethernet (Carrier Ethernet) over leased dark fiber rings (SONET/SDH or DWDM).
  - *Justification*: Enables private, high-bandwidth (100 Mbps - 1 Gbps), low-latency connection between branch POS systems and the central inventory database at the Head Office without exposing internal POS traffic to the public Internet.

* **Tier 3: Wide Area Network (WAN) - Interstate Cloud Backup**
  - *Coverage*: Regional and interstate connection spanning hundreds of kilometers.
  - *Technology*: Multiprotocol Label Switching (MPLS) or Secure SD-WAN over encrypted IPsec tunnels on redundant commercial fiber backbones.
  - *Justification*: High geographical reach, SLA guarantees for backup replication, and robust end-to-end encryption across untrusted public carrier links.

**Examiner Marking Rubric:**
* Architecture diagram connecting HQ, outlets, and cloud: 1.5 Marks.
* Clear definition and technology selection for LAN, MAN, and WAN: 2.5 Marks.
* Business justification (latency, bandwidth, cost, security): 1.0 Mark.

---

### Q.1.C: Network Topology Expansion: Star to Tree / Hybrid Architecture [5 Marks]
**Question Statement:**
> A small design firm operates 8 workstations connected via a basic Star topology. The firm is expanding to 60 employees spread across 3 distinct departments on different floors. Investigate how you would apply a Tree or Hybrid topology to expand their current network into a scalable, multi-floor layout while keeping maintenance simple and preserving central management.

**Model Answer:**

**1. Current vs Proposed Topology:**
* *Current (8 Nodes)*: Single unmanaged 8-port switch in a single room (flat Star).
* *Expansion Requirement*: 60 workstations across 3 floors (Floor 1: Dept A [20], Floor 2: Dept B [20], Floor 3: Dept C [20]) with centralized server access.

**2. Proposed Tree (Hierarchical Extended Star) Topology:**
```
                          [ SERVER ROOM / IT RACK ]
                          +-----------------------+
                          |      CORE SWITCH      |
                          +-----------+-----------+
                                      |
         +----------------------------+----------------------------+
         | (Gigabit Trunk)            | (Gigabit Trunk)            | (Gigabit Trunk)
         v                            v                            v
+-----------------+          +-----------------+          +-----------------+
| FLOOR 1 SWITCH  |          | FLOOR 2 SWITCH  |          | FLOOR 3 SWITCH  |
| (Access Layer)  |          | (Access Layer)  |          | (Access Layer)  |
+--------+--------+          +--------+--------+          +--------+--------+
         |                            |                            |
  [20 Workstations]            [20 Workstations]            [20 Workstations]
```

**3. Engineering Advantages for Multi-Floor Enterprise:**
1. **Preserved Central Management:**
   - The central Core Switch in the ground-floor server room houses the organization router, firewall, and shared NAS storage.
   - Network administrators manage security policies, VLANs, and DHCP reservations centrally.
2. **Simplified Cabling & Maintenance:**
   - Instead of running 60 individual long-distance cables across floors, only **one high-speed fiber or Cat6A backbone trunk** runs from the Core Switch to each floor's Distribution Switch.
   - Local workstation drops remain short (< 30 meters), eliminating cable clutter and signal attenuation.
3. **Fault Isolation & High Availability:**
   - If a floor switch or workstation cable fails, **only that single floor or station is impacted**. The remaining floors and central servers continue running unaffected.

**Examiner Marking Rubric:**
* Hierarchical Tree / Extended Star diagram with Core and Floor switches: 2.0 Marks.
* Detailed multi-floor physical allocation (60 nodes / 3 floors): 1.5 Marks.
* Justification of maintenance simplicity, fault isolation, and central management: 1.5 Marks.

---

# QUESTION 2 [CO2: Physical Layer & Signal Encodings] (Compulsory)

---

### Q.2.A: Manchester and Differential Manchester Encoding [5 Marks]
**Question Statement:**
> Apply Manchester and Differential Manchester Encoding method on `1011101100110` bit pattern.

**Model Answer:**

**1. Encoding Rules:**
* **Manchester Encoding (IEEE 802.3 Standard):**
  - Bit `0`: Low-to-High transition (`L -> H` or `0 -> 1`) in the middle of bit interval.
  - Bit `1`: High-to-Low transition (`H -> L` or `1 -> 0`) in the middle of bit interval.
* **Differential Manchester Encoding:**
  - Always has a transition in the middle of the interval (for clock synchronization).
  - Bit `0`: Transition at the **beginning** of the bit interval.
  - Bit `1`: **No transition** at the beginning of the bit interval.
  - (Assume initial signal reference starts High).

**2. Step-by-Step Transition Table for `1011101100110`:**

| Bit Index | Bit Value | Manchester Transition (IEEE 802.3) | Differential Manchester Transition |
| :---: | :---: | :---: | :---: |
| 1 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`H -> L`) |
| 2 | **0** | Low-to-High (`0 -> 1`) | Start transition (`L -> H`); Mid-bit toggle (`H -> L`) |
| 3 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`L -> H`) |
| 4 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`H -> L`) |
| 5 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`L -> H`) |
| 6 | **0** | Low-to-High (`0 -> 1`) | Start transition (`H -> L`); Mid-bit toggle (`L -> H`) |
| 7 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`H -> L`) |
| 8 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`L -> H`) |
| 9 | **0** | Low-to-High (`0 -> 1`) | Start transition (`H -> L`); Mid-bit toggle (`L -> H`) |
| 10 | **0** | Low-to-High (`0 -> 1`) | Start transition (`H -> L`); Mid-bit toggle (`L -> H`) |
| 11 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`H -> L`) |
| 12 | **1** | High-to-Low (`1 -> 0`) | No start transition; Mid-bit toggle (`L -> H`) |
| 13 | **0** | Low-to-High (`0 -> 1`) | Start transition (`H -> L`); Mid-bit toggle (`L -> H`) |

**3. ASCII Signal Waveforms:**
```
Bit Pattern:    |  1  |  0  |  1  |  1  |  1  |  0  |  1  |  1  |  0  |  0  |  1  |  1  |  0  |

Manchester:
  +V            |--+  |   +-|--+  |--+  |--+  |   +-|--+  |--+  |   +-|   +-|--+  |--+  |   +-|
   0            |  |  |   | |  |  |  |  |  |  |   | |  |  |  |  |   | |   | |  |  |  |  |   | |
  -V            |  +--|---+ |  +--|  +--|  +--|---+ |  +--|  +--|---+ |---+ |  +--|  +--|---+ |

Diff Manch:
  +V            |--+  |   +-|  +--|--+  |  +--|--+  |  +--|--+  |   +-|   +-|  +--|--+  |   +-|
   0            |  |  |   | |  |  |  |  |  |  |  |  |  |  |  |  |   | |   | |  |  |  |  |   | |
  -V            |  +--|---+ |--+  |  +--|--+  |  +--|--+  |  +--|---+ |---+ |--+  |  +--|---+ |
```

**Examiner Marking Rubric:**
* Stating correct encoding rules for both schemes: 1.0 Mark.
* Accurate mid-bit transitions for Manchester across all 13 bits: 2.0 Marks.
* Accurate start-of-bit edge transitions for Differential Manchester: 2.0 Marks.

---

### Q.2.B: Spread Spectrum Techniques: FHSS vs DSSS [5 Marks]
**Question Statement:**
> Illustrate the working of FHSS and DSSS spread spectrum methods, also comment on uses.

**Model Answer:**

**1. Frequency Hopping Spread Spectrum (FHSS):**
* *Principle*: The available channel bandwidth is partitioned into multiple narrow frequency channels. The carrier frequency rapidly **hops** among these channels in a pseudo-random order determined by a Pseudo-Noise (PN) sequence.
* *Mechanism*: Both transmitter and receiver share the same PN code generator and frequency synthesizer. Even if an adversary jams frequency $f_1$, only a single hop's worth of data is corrupted; the next hop at $f_4$ succeeds.

```
Frequency ^
   f4     |         [Hop 2]
   f3     |                           [Hop 4]
   f2     |                   [Hop 3]
   f1     | [Hop 1]
          +------------------------------------> Time
```

**2. Direct Sequence Spread Spectrum (DSSS):**
* *Principle*: Each original data bit is expanded by multiplying (XORing) it with a high-rate bit sequence called a **Chip Sequence** (e.g., 11-bit Barker code).
* *Mechanism*: If data bit is `1`, transmit Barker code `10110111000`. If data bit is `0`, transmit inverted Barker code. This spreads the narrow signal across a wide RF spectrum, causing it to appear below the noise floor to unintended listeners.

```
Data Bit (Tb)       : [       1       ]  (Duration Tb)
Barker Chips (Tc)   : [1 0 1 1 0 1 1 1 0 0 0]  (Chip duration Tc << Tb)
Transmitted Signal  : Wideband spread spectrum signal
```

**3. Comparative Summary & Real-World Uses:**

| Feature | FHSS (Frequency Hopping) | DSSS (Direct Sequence) |
| :--- | :--- | :--- |
| **Spreading Method** | Dynamic carrier frequency switching | Bit-level XOR multiplication with PN chips |
| **Complexity** | Simple radio frequency synthesizers | Requires high-speed correlators & DSPs |
| **Interference Resilience** | Narrowband interference avoided by hopping | Noise rejection via processing gain |
| **Primary Practical Uses** | **Bluetooth (IEEE 802.15.1)**, Military tactical combat radios | **Wi-Fi (IEEE 802.11b)**, GPS satellite signals, 3G CDMA |

**Examiner Marking Rubric:**
* Diagram and operating principle of FHSS: 2.0 Marks.
* Diagram and chip-sequence principle of DSSS: 2.0 Marks.
* Practical applications and use-cases: 1.0 Mark.

---

# QUESTION 3 [CO3: Data Link Layer & MAC Protocols] (Solve any 2 of 3)

---

### Q.3.A: Hamming Code Error-Detection & Correction Computation [5 Marks]
**Question Statement:**
> Compute Hamming Code for the given data bits D = `1011001`. Show the complete process and write down the values of redundant bits.

**Model Answer:**

**1. Determine Number of Redundant (Parity) Bits ($r$):**
* Given data bits $m = 7$ ($D = 1011001$).
* Formula: $2^r \ge m + r + 1$.
  - For $r = 3$: $2^3 = 8 < 7 + 3 + 1 = 11$ (Insufficient).
  - For $r = 4$: $2^4 = 16 \ge 7 + 4 + 1 = 12$ (Satisfied!).
* Total Codeword Length $n = m + r = 7 + 4 = 11$ bits.

**2. Bit Positioning:**
Parity bits occupy powers-of-2 positions ($1, 2, 4, 8$). Data bits fill remaining slots ($3, 5, 6, 7, 9, 10, 11$).
Let $D = d_7 d_6 d_5 d_4 d_3 d_2 d_1 = 1 0 1 1 0 0 1$:
- Pos 1: $P_1$
- Pos 2: $P_2$
- Pos 3: $d_1 = 1$
- Pos 4: $P_4$
- Pos 5: $d_2 = 0$
- Pos 6: $d_3 = 0$
- Pos 7: $d_4 = 1$
- Pos 8: $P_8$
- Pos 9: $d_5 = 1$
- Pos 10: $d_6 = 0$
- Pos 11: $d_7 = 1$

**3. Parity Bit Calculation (Even Parity System):**

* **$P_1$ (Checks bit positions whose binary index has 1 in position 0: 1, 3, 5, 7, 9, 11):**
  $P_1 \oplus 	ext{bit}(3) \oplus 	ext{bit}(5) \oplus 	ext{bit}(7) \oplus 	ext{bit}(9) \oplus 	ext{bit}(11) = 0$  
  $P_1 \oplus 1 \oplus 0 \oplus 1 \oplus 1 \oplus 1 = 0$  
  $P_1 \oplus (4 	ext{ ones}) = 0 \implies P_1 \oplus 0 = 0 \implies \mathbf{P_1 = 0}$.

* **$P_2$ (Checks bit positions whose binary index has 1 in position 1: 2, 3, 6, 7, 10, 11):**
  $P_2 \oplus 	ext{bit}(3) \oplus 	ext{bit}(6) \oplus 	ext{bit}(7) \oplus 	ext{bit}(10) \oplus 	ext{bit}(11) = 0$  
  $P_2 \oplus 1 \oplus 0 \oplus 1 \oplus 0 \oplus 1 = 0$  
  $P_2 \oplus (3 	ext{ ones}) = 0 \implies P_2 \oplus 1 = 0 \implies \mathbf{P_2 = 1}$.

* **$P_4$ (Checks bit positions whose binary index has 1 in position 2: 4, 5, 6, 7):**
  $P_4 \oplus 	ext{bit}(5) \oplus 	ext{bit}(6) \oplus 	ext{bit}(7) = 0$  
  $P_4 \oplus 0 \oplus 0 \oplus 1 = 0$  
  $P_4 \oplus 1 = 0 \implies \mathbf{P_4 = 1}$.

* **$P_8$ (Checks bit positions whose binary index has 1 in position 3: 8, 9, 10, 11):**
  $P_8 \oplus 	ext{bit}(9) \oplus 	ext{bit}(10) \oplus 	ext{bit}(11) = 0$  
  $P_8 \oplus 1 \oplus 0 \oplus 1 = 0$  
  $P_8 \oplus (2 	ext{ ones}) = 0 \implies \mathbf{P_8 = 0}$.

**4. Final Codeword:**
* Redundant bit values: **$P_1 = 0,\; P_2 = 1,\; P_4 = 1,\; P_8 = 0$**.
* 11-Bit Codeword:
```
Position:  1   2   3   4   5   6   7   8   9  10  11
Bit:      P1  P2  d1  P4  d2  d3  d4  P8  d5  d6  d7
Value:     0   1   1   1   0   0   1   0   1   0   1
```
* Codeword = **`01110010101`**.

*(Note: If mapped with $D$ from left to right as $d_1 d_2 d_3 d_4 d_5 d_6 d_7 = 1011001$, positions 3,5,6,7,9,10,11 become 1,0,1,1,0,0,1, giving $P_1=1, P_2=1, P_4=0, P_8=1$ with codeword `11100111001`. Both indexing conventions are recognized by examiners).*

**Examiner Marking Rubric:**
* Calculation of $r=4$ using $2^r \ge m+r+1$: 1.0 Mark.
* Positioning table of data and parity bits: 1.0 Mark.
* Derivation and values of $P_1, P_2, P_4, P_8$: 2.0 Marks.
* Final 11-bit codeword: 1.0 Mark.

---

### Q.3.B: Go-Back-N ARQ vs Selective Repeat ARQ Performance Comparison [5 Marks]
**Question Statement:**
> Distinguish between Go back N ARQ & Selective Repeat ARQ. Comment on the performance of each.

**Model Answer:**

**1. Architectural & Operational Comparison:**

| Parameter | Go-Back-N (GBN) ARQ | Selective Repeat (SR) ARQ |
| :--- | :--- | :--- |
| **Sender Window Size ($W_S$)** | Up to $2^m - 1$ | $2^{m-1}$ |
| **Receiver Window Size ($W_R$)** | Strictly **1** | Equal to sender window $W_S = 2^{m-1}$ |
| **Receiver Buffering** | No buffer; out-of-order frames discarded | Out-of-order error-free frames buffered |
| **Acknowledgment Type** | **Cumulative ACK** (ACK $n$ confirms all $< n$) | **Individual / Selective ACK** (ACKs specific frame) |
| **Retransmission on Loss** | Retransmits **all $N$ unACKed frames** | Retransmits **only the specific lost frame** |
| **Implementation Logic** | Very simple; minimal receiver memory | Complex; sorting buffers & logic required |

**2. Performance Commentary:**
* **Channel Efficiency under High Error Rates ($p > 0$):**
  - In GBN, losing a single frame $F_i$ forces retransmission of the entire window $F_i, F_{i+1}, \dots, F_{i+N-1}$. Over high bandwidth-delay product links (e.g. satellite), this wastes enormous channel bandwidth, dropping efficiency drastically.
  - In SR, only $F_i$ is retransmitted. Channel throughput remains close to optimal ($\eta pprox 1 - p$), making SR far superior on noisy, high-latency links.
* **Hardware Cost vs Throughput Trade-off:**
  - GBN is favored in resource-constrained IoT/embedded systems with tiny RAM buffers.
  - SR is used in high-performance protocols like TCP (via SACK option).

**Examiner Marking Rubric:**
* Comparative table (Window sizes, buffering, ACK mechanisms): 3.0 Marks.
* Performance analysis under error conditions and satellite channels: 2.0 Marks.

---

### Q.3.C: Persistence Algorithms in CSMA: 1-Persistent, Non-Persistent, and p-Persistent [5 Marks]
**Question Statement:**
> Analyze 1-persistent, non-persistent and p-persistent CSMA algorithm, also comment on their performances.

**Model Answer:**

**1. Operational Algorithms:**
When a station has a frame to transmit, it first senses the transmission channel:
* **1-Persistent CSMA:**
  - If channel is IDLE: Transmit immediately with probability $p = 1$.
  - If channel is BUSY: Continuously listen until channel becomes idle, then transmit immediately.
  - *Problem*: If two or more stations were waiting, they both transmit simultaneously the instant the channel clears $\implies$ **Guaranteed Collision!**
* **Non-Persistent CSMA:**
  - If channel is IDLE: Transmit immediately.
  - If channel is BUSY: **Do not continuously listen**. Wait for a random backoff time, then sense again.
  - *Advantage*: Reduces collision probability substantially.
  - *Disadvantage*: Channel stays idle unnecessarily while stations are in backoff, reducing utilization under light loads.
* **p-Persistent CSMA (Used in slotted channels):**
  - If channel is IDLE: Transmit with probability $p$; with probability $q = 1 - p$, defer to next time slot.
  - If channel is BUSY: Wait until idle, then apply probability $p$ test.

**2. Performance & Throughput Comparison:**

```
Throughput (S) ^
               |       p-Persistent (p=0.1)
           0.8 |          /---------           0.6 |         /           \      Non-Persistent
           0.4 |        /  /-------\  \     /-----------           0.2 |  1-Pers  /         \  \   /
             0 +---------+-----------+--\-+--------------> Offered Load (G)
```

* Under **Light Traffic Load**: 1-Persistent achieves lowest delay because stations transmit instantly without waiting.
* Under **Heavy Traffic Load**: 1-Persistent collapses due to endless collisions. Non-Persistent and low-$p$ Persistent maintain stable throughput because deferral mitigates burst collisions.

**Examiner Marking Rubric:**
* Algorithmic definition of all 3 modes: 2.5 Marks.
* Throughput vs Load curve / performance comparison: 2.5 Marks.

---

# QUESTION 4 [CO4: Network Layer, Subnetting & Routing] (Compulsory)

---

### Q.4.A: Critical Subnetting Evaluation on 192.168.1.0/24 [5 Marks]
**Question Statement:**
> An organization requires at least 6 subnets with a minimum of 25 hosts each on the 192.168.1.0/24 network. Critically evaluate whether a /27 subnet mask is the optimal configuration for this design. Assess the trade-offs regarding future subnet growth versus host expansion per subnet, and justify your final mask recommendation.

**Model Answer:**

**1. Mathematical Verification of /27 Subnet Mask:**
* Network: `192.168.1.0/24` (Class C, default 24 network bits, 8 host bits).
* For `/27` Mask (`255.255.255.224`):
  - Borrowed subnet bits $s = 27 - 24 = 3$ bits.
  - Remaining host bits $h = 32 - 27 = 5$ bits.
  - Number of Subnets = $2^s = 2^3 = \mathbf{8 	ext{ subnets}}$.
  - Since $8 \ge 6$, the subnet requirement is **fully satisfied** ($8 - 6 = 2$ spare subnets).
  - Number of Usable Hosts per Subnet = $2^h - 2 = 2^5 - 2 = 32 - 2 = \mathbf{30 	ext{ usable hosts}}$.
  - Since $30 \ge 25$, the host requirement is **fully satisfied** ($30 - 25 = 5$ spare host slots per subnet).

**2. Critical Evaluation of Alternative Subnet Masks:**
* **Evaluation of `/26` Mask (2 borrowed bits):**
  - Subnets = $2^2 = 4$. Fails the requirement of at least 6 subnets ($4 < 6$). **INVALID**.
* **Evaluation of `/28` Mask (4 borrowed bits):**
  - Usable hosts = $2^4 - 2 = 14$ hosts. Fails the requirement of at least 25 hosts ($14 < 25$). **INVALID**.

**3. Assessment of Expansion Trade-offs & Recommendation:**
* **The Mathematical Uniqueness:**
  Within a single flat `/24` block, the `/27` mask is the **ONLY mathematically viable standard configuration** that simultaneously yields $\ge 6$ subnets and $\ge 25$ usable hosts.
* **Trade-Off Analysis:**
  - *Subnet Growth*: Yields **2 extra subnets** (33% expansion capability). If the organization opens 2 new departments, no re-addressing is required.
  - *Host Growth*: Yields **5 extra host addresses** per subnet (20% headroom). If a department exceeds 30 workstations, that department will suffer IP starvation.
* **Final Engineering Recommendation:**
  Adopt `/27` for immediate deployment. If future departmental growth anticipates $>30$ hosts, migrate the organization to a private Class B block (`172.16.0.0/22`) or employ Variable-Length Subnet Masking (VLSM).

**Examiner Marking Rubric:**
* Calculation of subnets ($2^3 = 8$) and usable hosts ($2^5 - 2 = 30$): 2.0 Marks.
* Proof of why `/26` and `/28` fail requirements: 1.5 Marks.
* Trade-off assessment between subnet growth vs host growth: 1.5 Marks.

---

### Q.4.B: Distance Vector Routing Table Calculation for Router J [5 Marks]
**Question Statement:**
> Given the network topology in Figure-a and the incoming vectors from neighboring routers in Figure-b, apply the Distance Vector Routing algorithm to calculate Router J's new routing table. Evaluate whether Router J should update any of its preferred next-hop paths, and assess if any reported vector indicates a potential routing loop or infinity-convergence issue.

**Model Answer:**

**1. Given Link Costs from Router J to its 4 Neighbors:**
* $c(J, A) = 8$
* $c(J, I) = 10$
* $c(J, H) = 12$
* $c(J, K) = 6$

**2. Bellman-Ford Calculation for all 12 Destinations ($D_J(X) = \min_N [c(J,N) + D_N(X)]$):**

| Dest | Delay via A ($8 + D_A$) | Delay via I ($10 + D_I$) | Delay via H ($12 + D_H$) | Delay via K ($6 + D_K$) | Minimum Delay | Best Next Hop |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **A** | $8 + 0 = \mathbf{8}$ | $10 + 24 = 34$ | $12 + 20 = 32$ | $6 + 21 = 27$ | **8** | **A** |
| **B** | $8 + 12 = \mathbf{20}$ | $10 + 36 = 46$ | $12 + 31 = 43$ | $6 + 28 = 34$ | **20** | **A** |
| **C** | $8 + 25 = 33$ | $10 + 18 = \mathbf{28}$ | $12 + 19 = 31$ | $6 + 36 = 42$ | **28** | **I** |
| **D** | $8 + 40 = 48$ | $10 + 27 = 37$ | $12 + 8 = \mathbf{20}$ | $6 + 24 = 30$ | **20** | **H** |
| **E** | $8 + 14 = 22$ | $10 + 7 = \mathbf{17}$ | $12 + 30 = 42$ | $6 + 22 = 28$ | **17** | **I** |
| **F** | $8 + 23 = 31$ | $10 + 20 = \mathbf{30}$ | $12 + 19 = 31$ | $6 + 40 = 46$ | **30** | **I** |
| **G** | $8 + 18 = 26$ | $10 + 31 = 41$ | $12 + 6 = \mathbf{18}$ | $6 + 31 = 37$ | **18** | **H** |
| **H** | $8 + 17 = 25$ | $10 + 20 = 30$ | $12 + 0 = \mathbf{12}$ | $6 + 19 = 25$ | **12** | **H** |
| **I** | $8 + 21 = 29$ | $10 + 0 = \mathbf{10}$ | $12 + 14 = 26$ | $6 + 22 = 28$ | **10** | **I** |
| **J** | Direct = 0 | Direct = 0 | Direct = 0 | Direct = 0 | **0** | **-** |
| **K** | $8 + 24 = 32$ | $10 + 22 = 32$ | $12 + 22 = 34$ | $6 + 0 = \mathbf{6}$ | **6** | **K** |
| **L** | $8 + 29 = 37$ | $10 + 33 = 43$ | $12 + 9 = 21$ | $6 + 9 = \mathbf{15}$ | **15** | **K** |

**3. Router J's New Routing Table Output:**
```
Destination:   A   B   C   D   E   F   G   H   I   J   K   L
New Delay:     8  20  28  20  17  30  18  12  10   0   6  15
Next Hop:      A   A   I   H   I   I   H   H   I   -   K   K
```

**4. Evaluation of Next Hops, Routing Loops & Convergence:**
* **Path Updates**: Router J updates its preferred next hop dynamically: Destinations A, B route via A; C, E, F, I route via I; D, G, H route via H; K, L route via K.
* **Potential Loop / Infinity-Convergence Warning**:
  - Neighbor H reports a delay of $7$ to reach J ($D_H(J) = 7$). However, the direct link cost between J and H is $12$ ($c(J,H) = 12$).
  - If router H calculated this path of cost 7 by routing through an intermediate node that loops back to J (or before H learned about a link failure), a classic **two-node or multi-node routing loop** exists.
  - In basic Distance Vector routing, if link $J-H$ or a link behind H fails, routers J and H will mutually increment their reported vectors in steps of link cost, causing the infamous **Count-to-Infinity problem** until infinity (e.g., 16 hops) is reached.
  - *Mitigation*: Split Horizon with Poison Reverse must be enforced to suppress advertising routes back to the neighbor through which they were learned.

**Examiner Marking Rubric:**
* Complete numerical calculation for all 12 destinations: 2.5 Marks.
* Clean new routing table (Delays & Next Hops): 1.5 Marks.
* Critical loop/convergence evaluation ($D_H(J)=7$ anomaly & Count-to-Infinity): 1.0 Mark.

---

# QUESTION 5 [CO5: Transport Layer & Reliability] (Solve any 2 of 3)

---

### Q.5.A: TCP Congestion Window Slow-Start Graph [5 Marks]
**Question Statement:**
> Draw the graph of showing the growth of the TCP congestion window by applying slow start phase for a given initial congestion window and slow-start threshold.

**Model Answer:**

**1. Mechanism of Slow Start:**
* Starts with Initial Congestion Window: $	ext{cwnd} = 1 	ext{ MSS}$.
* For every valid ACK received, $	ext{cwnd}$ increments by $1 	ext{ MSS}$.
* Over one Round-Trip Time (RTT), $	ext{cwnd}$ **doubles exponentially**: $1 	o 2 	o 4 	o 8 	o 16 	ext{ MSS}$.
* When $	ext{cwnd} = 	ext{ssthresh}$ (Slow-Start Threshold, e.g., 16 MSS), TCP exits Slow Start and enters **Congestion Avoidance** (linear growth: $+1 	ext{ MSS}$ per RTT).

**2. Graph of TCP Congestion Window Growth:**
```
Congestion Window (cwnd in MSS) ^
                                |
                   32           |
                                |
                   24           |                                  / (Additive Increase)
                                |                                 /
     ssthresh ---> 16           |                     /----------/
                                |                    / (Linear: Congestion Avoidance)
                    8           |              /----/
                                |             /
                    4           |         /--/
                    2           |     /--/  (Exponential: Slow Start)
                    1           |----+
                                +----+----+----+----+----+----+----+----+---->
                                0    1    2    3    4    5    6    7    8    Transmission Round (RTT)
```

**3. Phase Descriptions for Full Marks:**
1. **Slow Start ($0 \le 	ext{cwnd} < 	ext{ssthresh}$):** Window grows exponentially ($2^{	ext{RTT}}$). Quickly probes available channel capacity.
2. **Transition Threshold ($	ext{cwnd} = 	ext{ssthresh}$):** State transition prevents violent network collapse.
3. **Congestion Avoidance ($	ext{cwnd} \ge 	ext{ssthresh}$):** Additive increase ($	ext{cwnd} = 	ext{cwnd} + 1/	ext{cwnd}$ per ACK) probing limit cautiously.

**Examiner Marking Rubric:**
* Clean labelled graph showing exponential Slow Start up to ssthresh: 2.5 Marks.
* Transition to linear Congestion Avoidance beyond threshold: 1.5 Marks.
* Explanation of cwnd doubling per RTT: 1.0 Mark.

---

### Q.5.B: TCP/IP Encapsulation and De-encapsulation & Protocol Data Units (PDUs) [5 Marks]
**Question Statement:**
> Demonstrate the process of encapsulation and de-encapsulation in TCP/IP layered reference model. Comment on various PDU's.

**Model Answer:**

**1. The Encapsulation / De-encapsulation Flow:**
```
  SENDER (Host A)                                                 RECEIVER (Host B)
  +------------------+                                            +------------------+
  | Application Data |                                            | Application Data |
  +--------+---------+                                            +--------^---------+
           | [Encapsulation: Add TCP Header]                               | [De-encapsulation: Strip TCP Header]
           v                                                               |
  +--------+---------+--------------------+                       +--------+---------+--------------------+
  | TCP Hdr| Data    | (PDU: Segment)     |                       | TCP Hdr| Data    | (PDU: Segment)     |
  +--------+---------+--------------------+                       +--------+---------+--------------------+
           | [Add IP Header]                                               | [Strip IP Header]
           v                                                               |
  +--------+---------+---------+----------+                       +--------+---------+---------+----------+
  | IP Hdr | TCP Hdr | Data    | (Packet) |                       | IP Hdr | TCP Hdr | Data    | (Packet) |
  +--------+---------+---------+----------+                       +--------+---------+---------+----------+
           | [Add MAC Header + FCS Trailer]                                | [Strip MAC Header + FCS Trailer]
           v                                                               |
  +--------+--------+--------+-----+-----+                         +--------+--------+--------+-----+-----+
  | MAC Hdr| IP Hdr | TCP Hdr| Data| FCS | (Frame)                 | MAC Hdr| IP Hdr | TCP Hdr| Data| FCS |
  +--------+--------+--------+-----+-----+                         +--------+--------+--------+-----+-----+
           | [Physical Conversion to Raw Bits]                             | [Convert Signals to Bits]
           v                                                               |
     1 0 1 0 1 1 0 0 1 1 0 1 0 0 1 0 ========= Transmission Wire =======> 1 0 1 0 1 1 0 0 1 1 0 1 0 0 1 0
```

**2. Detailed Commentary on Protocol Data Units (PDUs):**
1. **Application Layer PDU - Message / Data:** Contains raw application content (HTTP GET request, HTML payload, DNS query).
2. **Transport Layer PDU - Segment (TCP) / Datagram (UDP):** Prepends source and destination port numbers (e.g., 80, 443), sequence numbers, and checksum for process-to-process communication.
3. **Network / Internet Layer PDU - Packet / Datagram:** Prepends Source and Destination IP addresses, TTL, and fragmentation headers for host-to-host routing across intermediate routers.
4. **Data Link Layer PDU - Frame:** Prepends hardware MAC addresses and appends a **Frame Check Sequence (FCS / CRC-32)** trailer for hop-to-hop node transfer and physical bit-error detection.
5. **Physical Layer PDU - Bits / Symbols:** Encodes frames into electrical voltages, optical pulses, or radio waves.

**Examiner Marking Rubric:**
* Complete encapsulation & de-encapsulation diagram: 2.5 Marks.
* Explanation of all 4 major PDUs (Message, Segment, Packet, Frame): 2.5 Marks.

---

### Q.5.C: TCP Sockets vs UDP Sockets Comparison [5 Marks]
**Question Statement:**
> Compare TCP sockets and UDP-sockets.

**Model Answer:**

**1. Comprehensive Technical Comparison:**

| Feature | TCP Sockets (`SOCK_STREAM`) | UDP Sockets (`SOCK_DGRAM`) |
| :--- | :--- | :--- |
| **Connection Type** | Connection-oriented (requires 3-way handshake) | Connectionless (no handshake required) |
| **Reliability** | 100% Guaranteed delivery, ordered byte stream | Best-effort; messages may be lost, duplicated, or reordered |
| **Flow & Congestion Control**| Sliding window flow control + AIMD congestion control | Zero flow control, zero congestion control |
| **Data Framing** | Continuous Byte-Stream (no message boundaries) | Preserves discrete **Datagram Message Boundaries** |
| **Header Overhead** | 20 to 60 Bytes | Fixed 8 Bytes |
| **Server Socket APIs** | `socket() -> bind() -> listen() -> accept() -> recv()/send()` | `socket() -> bind() -> recvfrom()/sendto()` |
| **Client Socket APIs** | `socket() -> connect() -> send()/recv()` | `socket() -> sendto()/recvfrom()` |
| **Typical Applications** | HTTP/HTTPS, SSH, FTP, SMTP, MySQL database queries | DNS lookup, DHCP, VoIP, live video streaming, multiplayer games |

**Examiner Marking Rubric:**
* Comparative table across key attributes: 3.0 Marks.
* Comparison of socket API primitives (`listen`/`accept` vs `recvfrom`): 2.0 Marks.

---

# QUESTION 6 [CO6: Application Layer & Security] (Compulsory)

---

### Q.6.A: Web Cookies, Security Mitigations & Role of Proxy Servers [5 Marks]
**Question Statement:**
> Discuss how cookies are handled in a web communication scenario and apply suitable security measures to mitigate cookie-related threats. Explain the role of a proxy server in this process.

**Model Answer:**

**1. How Cookies are Handled in Web Communication:**
HTTP is inherently a **stateless protocol**. To maintain user sessions (e.g., shopping carts, login authentication), cookies bridge this state:
1. **Creation**: When a user logs in, the Server sends an HTTP response containing:
   `Set-Cookie: session_id=XYZ789; Path=/; Domain=example.com`
2. **Storage**: The client browser stores this key-value pair in its local cookie jar.
3. **Subsequent Requests**: For every subsequent HTTP request to that domain, the browser automatically attaches:
   `Cookie: session_id=XYZ789`
   allowing the server to identify the authenticated user immediately.

```
  CLIENT (Browser)                                                SERVER (Web App)
         |                                                               |
         | ----- (1) POST /login (Credentials) ------------------------> |
         | <---- (2) HTTP 200 OK [Set-Cookie: session=XYZ; HttpOnly] --- |
         |                                                               |
         | ----- (3) GET /dashboard [Cookie: session=XYZ] -------------> |
         | <---- (4) HTTP 200 OK (Personalized Page Content) ----------- |
```

**2. Cookie Threats and Security Measures:**
* **Threat 1: Cross-Site Scripting (XSS) Cookie Theft**
  - *Attack*: Malicious JavaScript injected into a page reads `document.cookie` and exfiltrates session tokens.
  - *Mitigation*: Set the **`HttpOnly` flag**. This forbids client-side scripts from reading the cookie.
* **Threat 2: Man-in-the-Middle (MitM) Eavesdropping**
  - *Attack*: Cookie intercepted over plain HTTP Wi-Fi.
  - *Mitigation*: Set the **`Secure` flag**. The browser will only transmit the cookie over encrypted TLS (`https://`).
* **Threat 3: Cross-Site Request Forgery (CSRF)**
  - *Attack*: An attacker's site tricks the browser into issuing authenticated requests to the bank.
  - *Mitigation*: Set **`SameSite=Strict`** or `SameSite=Lax` flag, preventing cross-origin transmission.

**3. Role of Proxy Servers in Web Communication:**
* **Forward Proxy (Client-Side)**: Caches web pages to reduce WAN bandwidth, enforces organizational access control, and hides client internal IP addresses.
* **Reverse Proxy (Server-Side - e.g. NGINX)**: Terminates SSL/TLS certificates, load-balances requests across backend application clusters, and mitigates DDoS attacks.
* **Proxy Cookie Handling Constraint**: Proxies **must never cache** responses containing `Set-Cookie` headers or private authentication data (`Cache-Control: no-cache, private`), otherwise a user's session token could be served to other clients, causing massive account compromise!

**Examiner Marking Rubric:**
* Step-by-step cookie lifecycle (`Set-Cookie` and `Cookie` headers): 2.0 Marks.
* Three security flags (`HttpOnly`, `Secure`, `SameSite`) with mitigated threats: 2.0 Marks.
* Role of Forward/Reverse proxy and cache isolation rules: 1.0 Mark.

---

### Q.6.B: FTP Working Architecture: Control vs Data Connection [5 Marks]
**Question Statement:**
> Explain the working of FTP with a neat diagram. Differentiate between its control connection and data connection.

**Model Answer:**

**1. FTP Working Architecture:**
The File Transfer Protocol (FTP - RFC 959) uses an **out-of-band architecture** employing two separate TCP connections to transfer files reliably:
```
  FTP CLIENT                                                      FTP SERVER
  +--------------------+                                          +--------------------+
  | Client User I/F    |                                          | Server Control     |
  | Client Control     | <==== [ Port 21: Control Connection ] ===>| Process (PI)       |
  | Process (PI)       |       (Commands: USER, PASS, RETR, QUIT) | (Listens on 21)    |
  +--------------------+                                          +--------------------+
  | Client Data        |                                          | Server Data        |
  | Transfer (DTP)     | <==== [ Port 20: Data Connection ] =====>| Transfer (DTP)     |
  +--------------------+       (Actual File Payload Stream)       +--------------------+
```

**2. Detailed Differentiation:**

| Characteristic | Control Connection (Port 21) | Data Connection (Port 20 or Dynamic) |
| :--- | :--- | :--- |
| **Standard Port** | **TCP Port 21** | **TCP Port 20** (Active) or Dynamic High Port (Passive) |
| **Duration** | Remains open for the **entire duration** of user session | Ephemeral: Opens on-demand for a file/directory and closes immediately |
| **Payload Transmitted**| Control commands (`USER`, `PASS`, `LIST`, `RETR`, `STOR`, `QUIT`) and 3-digit status codes | Raw file bytes, directory listings, binary/ASCII file content |
| **State Nature** | Two-way interactive command dialog | Pure unidirectional or bidirectional bulk data pipe |
| **Active vs Passive Mode**| Client connects directly to Server Port 21 | In Active Mode: Server initiates connection from Port 20 to Client port. In Passive Mode (`PASV`): Client connects to Server high port |

**Examiner Marking Rubric:**
* Clean dual-connection architecture diagram: 2.0 Marks.
* Port 21 (Control) vs Port 20 (Data) comparison across duration, payload, and ports: 2.0 Marks.
* Active vs Passive mode firewall implication: 1.0 Mark.
