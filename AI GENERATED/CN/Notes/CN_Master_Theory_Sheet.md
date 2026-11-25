# Computer Networks (CS3213 / IT2266 / CS3025)
# Master Theory, Architecture & Analytical Question Sheet

> **Scope**: All 6 Units of the Computer Networks Syllabus (VIT Pune / SPPU pattern).  
> **Structure**: Unit-wise and Topic-wise Master Theory Guide covering the 30 core exam topics.  
> **For Every Topic**:  
> - **Exact Exam Question Statement** (verbatim from past VIT papers including Nov 2024 and May 2019)  
> - **Core Concept & Working Principle**  
> - **Architecture / Protocol Stack / Packet Format / State Diagrams**  
> - **In-Depth Analytical Comparison & Technical Justification**  
> - **Examiner Marking Rubric & Key Points Evaluators Look For**  
> **Formatting**: 100% clean, human-readable universal ASCII formatting with zero unreadable symbols.

---

# UNIT 1: FUNDAMENTALS & PHYSICAL LAYER

---

### TOPIC 1: OSI 7-Layer Reference Model vs TCP/IP 4-Layer Architecture
**Exam Question Statement:**
> Explain the OSI Model with a neat diagram. State the functionality, representative protocols, and standards of each layer. Compare it with the TCP/IP Model and explain why the OSI model failed commercially. [Nov 2024 IT2266 Q1b, 4M / May 2019 CS3025 Q6a, 4M]

**1. Architectural Stacks Diagram:**
```
       OSI 7-LAYER MODEL                      TCP/IP 4-LAYER MODEL
  +--------------------------+             +--------------------------+
  | 7. Application Layer     |             |                          |
  +--------------------------+             |  Application Layer       |
  | 6. Presentation Layer    | ----------> |  (HTTP, DNS, SMTP, DHCP) |
  +--------------------------+             |                          |
  | 5. Session Layer         |             +--------------------------+
  +--------------------------+             |  Transport Layer         |
  | 4. Transport Layer       | ----------> |  (TCP, UDP, SCTP, QUIC)  |
  +--------------------------+             +--------------------------+
  | 3. Network Layer         | ----------> |  Internet / Network Layer|
  |                          |             |  (IPv4, IPv6, ICMP, ARP) |
  +--------------------------+             +--------------------------+
  | 2. Data Link Layer       | ----------> |  Network Access /        |
  +--------------------------+             |  Link Layer              |
  | 1. Physical Layer        | ----------> |  (Ethernet, Wi-Fi, PPP)  |
  +--------------------------+             +--------------------------+
```

**2. Detailed Layer Breakdown:**
* **Application Layer (Layer 7)**: Directly interfaces with application software. Provides user-level network services. Protocols: HTTP (web), DNS (name resolution), SMTP (mail), DHCP (auto IP), FTP (files).
* **Presentation Layer (Layer 6)**: Responsible for syntax, data representation, character translation (ASCII/EBCDIC), compression, and encryption/decryption (TLS/SSL).
* **Session Layer (Layer 5)**: Manages dialog control, session checkpointing, synchronization tokens, and connection establishment/teardown between communicating processes. Protocols: NetBIOS, RPC.
* **Transport Layer (Layer 4)**: True end-to-end process-to-process communication. Manages port addressing, segmentation, reassembly, reliable delivery, and flow/congestion control. Protocols: TCP (connection-oriented, reliable), UDP (connectionless, best-effort).
* **Network Layer (Layer 3)**: Host-to-host routing and logical addressing across heterogeneous networks. Formulates routing tables, handles packet forwarding, fragmentation, and IP addressing. Protocols: IPv4, IPv6, ICMP, OSPF, BGP.
* **Data Link Layer (Layer 2)**: Node-to-node hop-by-hop framing, physical MAC addressing, error detection (CRC), flow control, and medium access arbitration. Protocols: Ethernet (IEEE 802.3), Wi-Fi (802.11), PPP, HDLC.
* **Physical Layer (Layer 1)**: Transmits raw unstructured bitstreams over copper, fiber, or air. Specifies mechanical, electrical, and functional interface standards (voltage levels, pinouts, timing, bit duration). Standards: RS-232, 1000Base-T, DSL.

**3. Why the OSI Model Failed Commercially ("The 4 Bads"):**
1. **Bad Timing**: When OSI standards were being drafted, TCP/IP was already implemented, deployed on ARPANET, and widely adopted by universities and UNIX vendors.
2. **Bad Technology**: The 7 layers are unbalanced. Session and Presentation layers are very thin and rarely need dedicated layers, while Data Link and Network are overburdened. Redundant features appear in multiple layers (flow/error control in Layers 2 and 4).
3. **Bad Implementations**: Early OSI protocol stacks were bloated, slow, and resource-heavy, while TCP/IP code in BSD UNIX was lightweight, robust, and free.
4. **Bad Politics**: OSI was viewed as a bureaucratic top-down government/telecom committee standard, whereas TCP/IP was an agile, grassroots, running-code-first engineering triumph.

**Examiner Marking Rubric:**
* Neat diagram showing all 7 layers mapped to 4 layers: 1.5 Marks.
* Functionality and representative protocols per layer: 1.5 Marks.
* Analytical reason for OSI commercial failure: 1.0 Mark.

---

### TOPIC 2: Connecting Devices Across Layers
**Exam Question Statement:**
> Differentiate between Hub, Layer-2 Switch, and Router. Explain how they segment collision domains and broadcast domains in enterprise networks. [May 2019 CS3025 Q1a, 2M / Practice Pattern]

**1. Connecting Devices Comparison Matrix:**

| Feature | Hub | Layer-2 Switch | Router |
| :--- | :--- | :--- | :--- |
| **Operating Layer** | Layer 1 (Physical) | Layer 2 (Data Link) | Layer 3 (Network) |
| **Addressing Used** | None (Raw electrical bits) | 48-bit Physical MAC Addresses | 32-bit Logical IP Addresses |
| **Forwarding Method** | Electrical repeater / broadcast to all ports | Hardware MAC table lookup (CAM table) | Software/FIB IP routing table lookup |
| **Collision Domains** | **1 Single Collision Domain** across all ports | **Each port is its own separate Collision Domain** | **Each interface is its own separate Collision Domain** |
| **Broadcast Domains** | **1 Single Broadcast Domain** | **1 Single Broadcast Domain** (all ports share broadcast) | **Each interface is its own separate Broadcast Domain** |
| **Transmission Mode** | Half-Duplex strictly (collisions can occur) | Full-Duplex dedicated link per port | Full-Duplex point-to-point / routed |
| **Traffic Filtering** | Cannot filter frames | Filters selectively based on destination MAC | Filters based on IP, subnet, ACLs, and QoS |

**2. Physical Intuition:**
* **Collision Domain**: A network segment where packets can physically collide with one another if transmitted simultaneously. A Switch breaks collision domains because it buffers incoming frames in internal ASIC memory and forwards them directly to the destination port, eliminating shared wire contention.
* **Broadcast Domain**: A network segment where a broadcast frame (e.g. `FF:FF:FF:FF:FF:FF` in ARP) is received by all hosts. A Switch forwards broadcasts out all active ports. Only a **Router** terminates broadcast domains, preventing broadcast storms from flooding enterprise wide-area links.

**Examiner Marking Rubric:**
* Layers and addressing identification: 0.5 Mark.
* Clear distinction of collision domain behavior: 0.75 Mark.
* Clear distinction of broadcast domain behavior: 0.75 Mark.

---

### TOPIC 3: Line Encoding Techniques & Waveform Analysis
**Exam Question Statement:**
> Differentiate between NRZ, Manchester, Differential Manchester, and AMI line encodings. Draw the waveforms for the bit sequence `1 0 1 1 0 0 1 0`. Explain why Manchester encoding has self-clocking capability. [May 2019 CS3025 Q1b, 2M / Practice Pattern]

**1. Technical Definitions & Comparison:**
* **NRZ-L (Non-Return-to-Zero Level)**: `0 = High Voltage`, `1 = Low Voltage`. Suffers from DC baseline wander and loss of clock synchronization during long runs of consecutive 0s or 1s.
* **Manchester (IEEE 802.3 Standard)**:
  - Transition occurs in the **exact middle** of every single bit interval.
  - Bit `0` = Low-to-High transition (`_|-`).
  - Bit `1` = High-to-Low transition (`-|_`).
  - *Advantage*: Guarantees a voltage transition in every bit, allowing receiver clock circuits to lock onto the phase (100% self-clocking).
  - *Disadvantage*: Requires double the bandwidth (baud rate = 2x data rate, 50% modulation efficiency).
* **Differential Manchester (IEEE 802.5 Token Ring)**:
  - Transition occurs in the middle of every bit interval (used purely for clocking).
  - Bit `0` = Presence of a transition at the *start* of the bit interval.
  - Bit `1` = Absence of a transition at the *start* of the bit interval.
  - Inversion at boundary indicates `0`; continuity indicates `1`.
* **AMI (Alternate Mark Inversion)**:
  - Bit `0` = Zero voltage (neutral).
  - Bit `1` = Alternating positive (`+V`) and negative (`-V`) voltages.
  - Eliminates DC baseline wander because positive and negative pulses cancel out over time.

**2. Waveform Trace for `1 0 1 1 0 0 1 0`:**
```
Bit Stream:       1       0       1       1       0       0       1       0
Clock:         |---|___|---|___|---|___|---|___|---|___|---|___|---|___|---|___|

Manchester:    |---|_  |  _|---|---|_  |---|_  |  _|---|  _|---|---|_  |  _|---|
Transition:     H-to-L  L-to-H  H-to-L  H-to-L  L-to-H  L-to-H  H-to-L  L-to-H

Diff. Man:     |---|_  |---|___|  _|---|---|_  |---|___|  _|---|  _|---|---|___|
Start trans?     No     Yes(0)   No(1)   No(1)  Yes(0)  Yes(0)   No(1)  Yes(0)

AMI:           |+++|   |       |---|   |+++|   |       |       |---|   |       |
Voltage:        +V        0      -V      +V       0       0      -V       0
```

**Examiner Marking Rubric:**
* Rule statement for Manchester & Differential Manchester: 1.0 Mark.
* Correct waveform drawing with mid-bit boundary dashed lines: 1.0 Mark.

---

### TOPIC 4: Software-Defined Networking (SDN) Architecture
**Exam Question Statement:**
> Explain Software-Defined Networking (SDN). How does SDN decouple the Control Plane from the Data Plane, and what advantages does it offer over traditional routing? [May 2019 CS3025 Q6a, 2M / Practice Pattern]

**1. Architectural Decoupling Diagram:**
```
      TRADITIONAL ROUTER                     SOFTWARE-DEFINED NETWORKING (SDN)
  +------------------------+             +-----------------------------------------+
  |     CONTROL PLANE      |             |         SDN APPLICATION TIER            |
  |  (Routing Algorithm,   |             |  (Load Balancing, Firewall, Monitoring) |
  |   OSPF/BGP Protocol)   |             +-----------------------------------------+
  +-----------+------------+                                  | (Northbound API / REST)
              |                          +-----------------------------------------+
  +-----------v------------+             |      SDN CONTROLLER (Control Plane)     |
  |       DATA PLANE       |             |  (Centralized Network OS, Routing Graph)|
  |   (Fast Hardware ASIC  |             +-----------------------------------------+
  |   Forwarding Engine)   |                                  | (Southbound API / OpenFlow)
  +------------------------+             +--------------------+--------------------+
   (Both planes tightly bound            |                    |                    |
     inside proprietary box)        +----v----+          +----v----+          +----v----+
                                    | Switch1 |          | Switch2 |          | Switch3 |
                                    +---------+          +---------+          +---------+
                                    (Pure Fast Data Forwarding Engines - Flow Tables)
```

**2. Key Technical Concepts:**
* **Control Plane**: The intelligence of the network. Responsible for computing routing tables, evaluating topology graphs (Dijkstra/Bellman-Ford), and applying routing policies.
* **Data Plane**: The packet-forwarding engine. Examines incoming frame headers, performs rapid ternary CAM table matches, and directs packets to outgoing switch interfaces.
* **Traditional Networking Limitations**: Every router contains both planes locally in a single proprietary closed appliance (Cisco, Juniper). Upgrading protocols requires manual box-by-box CLI reconfiguration, making dynamic traffic re-routing sluggish and error-prone.
* **The SDN Breakthrough**:
  1. Centralizes network state into a programmable software controller (e.g. OpenDaylight, ONOS).
  2. Uses open standardized protocols (**OpenFlow**) to inject flow rules directly into simple, cost-effective hardware switches.
  3. Enables dynamic traffic engineering, global network visibility, automated security policies, and rapid protocol experimentation without purchasing new hardware.

**Examiner Marking Rubric:**
* Diagram showing Application, Controller, and Data planes: 1.0 Mark.
* Explanation of Control vs Data plane separation: 1.0 Mark.

---

# UNIT 2: DATA LINK LAYER & FRAMING / ERROR CONTROL

---

### TOPIC 5: Framing Techniques: Bit Stuffing vs Byte Stuffing
**Exam Question Statement:**
> Why is framing needed in the Data Link Layer? Compare Bit Stuffing and Byte Stuffing with examples. How does bit stuffing prevent frame flag collision? [Nov 2024 IT2266 Q2a / Practice Pattern]

**1. Need for Framing:**
The physical layer delivers a continuous, raw stream of unstructured bits without natural delimiters. The Data Link Layer must break this raw bitstream into discrete, manageable units called **Frames** so the receiver can detect the start and end of packets, compute checksums, and detect lost data.

**2. Framing Methods:**
* **Byte Stuffing (Character-Oriented Framing)**:
  - Frames are bounded by a special delimiter flag byte, typically `FLAG = 0x7E`.
  - If the flag byte pattern appears naturally inside user data, the sender inserts an Escape byte (`ESC = 0x1B`) immediately before it.
  - If an `ESC` byte appears in data, it is prefixed with another `ESC` byte (`ESC ESC`).
  - The receiver strips the first `ESC` and treats the following byte as pure payload.
* **Bit Stuffing (Bit-Oriented Framing - HDLC / PPP)**:
  - Frames begin and end with the unique 8-bit flag pattern: **`01111110`** (`0x7E`, zero followed by six consecutive 1s and a zero).
  - **The Golden Rule**: Whenever the sender's transmission hardware detects **five consecutive 1s (`11111`)** in the payload bitstream, it automatically stuffs a single `0` immediately after the 5th `1`, regardless of what the next bit is!
  - **Receiver Action**: Whenever the receiver detects five consecutive 1s followed by a `0`, it strips the `0` bit automatically. If the five 1s are followed by `10`, it recognizes the end-of-frame flag `01111110`.

**3. Bit Stuffing Worked Example:**
* Input Payload: `0 1 1 1 1 1 1 0 1 1 1 1 1 1 0`
* Stuffed Output:
  - First five 1s: `0 1 1 1 1 1` --> insert `[0]` --> `0 1 1 1 1 1 [0] 1 0`
  - Next five 1s: `1 1 1 1 1` --> insert `[0]` --> `1 1 1 1 1 [0] 1 0`
  - Transmitted: `0 1 1 1 1 1 [0] 1 0 1 1 1 1 1 [0] 1 0`

**Examiner Marking Rubric:**
* Reason for framing: 0.5 Mark.
* Explanation of Bit stuffing 5-ones rule: 1.0 Mark.
* Correctly worked bit stuffing trace: 0.5 Mark.

---

### TOPIC 6: Sliding Window Protocols: Stop-and-Wait vs GBN vs Selective Repeat
**Exam Question Statement:**
> Critically compare Stop-and-Wait, Go-Back-N, and Selective Repeat ARQ protocols in terms of sender/receiver window sizes, buffer requirements, out-of-order frame handling, retransmission strategy, and bandwidth utilization. [May 2019 CS3025 Q1a, 9M]

**Comprehensive Comparison Matrix:**

| Feature | Stop-and-Wait ARQ | Go-Back-N (GBN) ARQ | Selective Repeat (SR) ARQ |
| :--- | :--- | :--- | :--- |
| **Sender Window (W_s)** | Strictly **1** | **`2^k - 1`** (for k-bit sequence numbers) | **`2^(k - 1)`** |
| **Receiver Window (W_r)**| Strictly **1** | Strictly **1** | **`2^(k - 1)`** (Equals sender window!) |
| **Out-of-Order Handling**| Impossible (only 1 frame in flight) | **Discards** out-of-order frames silently! | **Buffers** out-of-order frames in memory |
| **Retransmission Trigger**| Timeout of single frame | Timeout of lost frame retransmits **N frames** (entire window!) | Timeout retransmits **ONLY the lost frame** |
| **Acknowledgment Type** | Individual ACK | **Cumulative ACK** (ACK n confirms all frames <= n) | **Independent / Selective ACK** (SACK confirms specific frame) |
| **Receiver Buffer Size** | 1 frame buffer | 1 frame buffer | `W_r = 2^(k - 1)` frames buffer |
| **Channel Efficiency (eta)**| `1 / (1 + 2a)` | `min(1.0, W_s / (1 + 2a))` | `min(1.0, W_s / (1 + 2a))` |
| **Link Utilization on Noisy Channel** | Poor | Extremely poor if error rate is high (retransmits redundant valid frames) | **Optimal** (zero redundant retransmissions) |
| **Implementation Complexity** | Trivial | Moderate | High (requires sorting buffers and individual timers per frame) |

**Physical Justification of Window Sizes:**
* In **GBN**, `W_s = 2^k - 1`. If `W_s = 2^k`, and an ACK is lost, the receiver cannot distinguish between a retransmitted old frame and a brand new frame with the same wrapped sequence number.
* In **SR**, `W_s + W_r <= 2^k`. Since `W_s = W_r`, we have `2 * W_s <= 2^k ==> W_s = 2^(k - 1)`. If `W_s > 2^(k - 1)`, the new receiver window overlaps with the old sender window, causing duplicate packet corruption.

**Examiner Marking Rubric:**
* Complete comparative table covering window sizes, buffers, and ACKs: 4.0 Marks.
* Mathematical derivation of why `W_s <= 2^k - 1` in GBN and `W_s <= 2^(k-1)` in SR: 3.0 Marks.
* Channel utilization equations: 2.0 Marks.

---

### TOPIC 7: Data Link Protocols: HDLC vs PPP
**Exam Question Statement:**
> Why is PPP (Point-to-Point Protocol) much more popular and widely deployed than HDLC (High-Level Data Link Control)? [May 2019 CS3025 Q2d, 3M]

**Core Analytical Reasons:**
1. **Multi-Protocol Encapsulation**: HDLC was designed primarily to support a single network-layer protocol (IBM SNA or raw IP). PPP incorporates the **Network Control Protocol (NCP)** family, allowing concurrent multiplexing of multiple network-layer protocols (IPv4, IPv6, AppleTalk, IPX) over the exact same physical link.
2. **Built-in Security & Authentication**: HDLC has zero native authentication capabilities. PPP includes native support for **PAP (Password Authentication Protocol)** and the cryptographically secure challenge-handshake protocol **CHAP (Challenge Handshake Authentication Protocol)**.
3. **Dynamic IP Address Assignment**: PPP includes **IPCP (Internet Protocol Control Protocol - RFC 1332)**, which automatically negotiates and assigns IP addresses, subnet masks, and DNS servers to dial-up or broadband clients without static manual configuration.
4. **Byte-Oriented vs Bit-Oriented**: PPP uses byte-oriented byte stuffing, which runs cleanly and efficiently on standard computer CPU memory architectures, whereas HDLC's bit stuffing requires dedicated hardware shift registers.
5. **Open Standard vs Vendor Lock-In**: HDLC implementations by vendors (e.g. Cisco HDLC) added proprietary header fields, making Cisco HDLC completely incompatible with non-Cisco routers. PPP is an open, standardized IETF RFC (RFC 1661) guaranteed to interoperate across all manufacturers.

**Examiner Marking Rubric:**
* Stating multi-protocol NCP support: 1.0 Mark.
* Authentication (PAP/CHAP) and IPCP configuration: 1.0 Mark.
* Interoperability vs proprietary HDLC lock-in: 1.0 Mark.

---

# UNIT 3: MEDIUM ACCESS CONTROL (MAC) SUBLAYER

---

### TOPIC 8: Random Access Protocols: ALOHA vs CSMA Evolution
**Exam Question Statement:**
> Trace the evolution of random access protocols from Pure ALOHA to Slotted ALOHA and CSMA methods (1-Persistent, Non-Persistent, and p-Persistent). Show why CSMA achieves superior throughput. [May 2019 CS3025 Q1d, 4M]

**1. Protocol Evolution Timeline:**
* **Pure ALOHA (1970)**:
  - Philosophy: "Transmit whenever you have data."
  - Vulnerable Period: `2 * T_frame`. A frame collides if another station transmits within one frame time before or after its start.
  - Maximum efficiency: `S_max = 1 / (2e) approx. 18.4%`.
* **Slotted ALOHA (1972)**:
  - Philosophy: "Divide time into discrete slots of duration `T_frame`. Transmit only at slot boundaries."
  - Vulnerable Period: `1 * T_frame` (halved!).
  - Maximum efficiency: `S_max = 1 / e approx. 36.8%` (Double Pure ALOHA!).
* **CSMA ("Carrier Sense Multiple Access - Listen Before Talk")**:
  - The Fatal Flaw of ALOHA: Stations inject frames blindly without checking whether another station is already transmitting.
  - The CSMA Breakthrough: Stations sample the physical medium voltage/energy before transmitting. If a carrier is detected, transmission is deferred.
* **CSMA Persistence Modes**:
  1. **1-Persistent CSMA**: Senses channel. If busy, listens continuously; transmits immediately with probability 1 as soon as channel becomes idle. Efficiency `approx. 53%`. (Suffers collisions when two waiting stations both fire immediately upon idle).
  2. **Non-Persistent CSMA**: Senses channel. If busy, does NOT listen continuously; waits a random back-off time before sensing again. Eliminates synchronized bursts. Efficiency `approx. 85%`!
  3. **p-Persistent CSMA (Used in slotted channels & Wi-Fi)**: Senses channel. When idle, transmits with probability `p`, and defers to next slot with probability `(1 - p)`. Optimizes trade-off between idle channel slots and collisions.

**Examiner Marking Rubric:**
* Vulnerable period derivation for Pure vs Slotted ALOHA: 1.5 Marks.
* Explanation of Carrier Sensing mechanism: 1.0 Mark.
* Contrast between 1-persistent, non-persistent, and p-persistent: 1.5 Marks.

---

### TOPIC 9: CSMA/CD: Operation & Minimum Frame Size Justification
**Exam Question Statement:**
> Explain the operation of CSMA/CD. Why is the minimum frame size of a 10Base5 Ethernet Network strictly 64 bytes? What happens if a station transmits a frame smaller than 64 bytes? [May 2019 CS3025 Q1c, 5M / Nov 2024 IT2266 Q3b, 6M]

**1. Step-by-Step CSMA/CD Operation:**
1. **Carrier Sense**: Station listens to channel before transmission.
2. **Transmit while Listening**: If idle, station begins transmitting data bits while *continuously monitoring* channel voltage levels.
3. **Collision Detection**: If another station transmits simultaneously, the two signals interfere, doubling the voltage amplitude on the bus. The transceiver detects this threshold surge.
4. **Jam Signal**: The station immediately aborts transmission and broadcasts a **32-to-48 bit Jamming Signal** to ensure all other stations recognize the collision.
5. **Back-Off Execution**: Station executes the **Binary Exponential Back-Off (BEB)** algorithm, waits for `K * 51.2 us`, and re-senses the channel.

**2. Physical Derivation of 64-Byte Minimum Frame:**
* **Governing Physical Law**: The sender must still be actively transmitting bits when the collision signal from the farthest node on the cable travels all the way back to the sender:
  `T_trans >= 2 * T_prop ==> L_min / Bandwidth >= 2 * (Distance / Velocity)`
  `L_min >= 2 * T_prop * Bandwidth`
* **Numerical Proof for 10Base5 Ethernet**:
  - Max network span with 4 repeaters = `2500 meters`.
  - Velocity of signal in copper cable `v approx. 2 x 10^8 m/s`.
  - One-way propagation delay: `T_prop = 2500 / (2 x 10^8) = 12.5 us`.
  - Accounting for repeater delays, IEEE 802.3 defines maximum round-trip time: `2 * T_prop = 51.2 us`.
  - Transmission Bandwidth: `B = 10 Mbps = 10^7 bps`.
  - Minimum Frame Size:
    `L_min = 51.2 us * 10 Mbps = 51.2 x 10^(-6) * 10^7 = 512 bits = 64 Bytes`.

**3. What Happens if Frame Size < 64 Bytes?**
If a station sends a tiny frame (e.g. 20 Bytes), `T_trans` finishes before the collision signal arrives from the distant node. The sender assumes the frame was delivered successfully, purges it from its transmit buffer, and fails to invoke back-off or retransmission! The frame is completely lost without upper layers being notified.

**Examiner Marking Rubric:**
* CSMA/CD operational steps and Jam signal: 2.0 Marks.
* Mathematical derivation of `T_trans >= 2 * T_prop`: 2.0 Marks.
* Consequence of frames smaller than 64 bytes: 1.0 Mark.

---

### TOPIC 10: CSMA/CA & Wireless Hidden Terminal Solutions
**Exam Question Statement:**
> How is the CSMA/CA protocol used to avoid collisions in wireless transmission media? Explain the Hidden Terminal and Exposed Terminal problems and how RTS/CTS exchange resolves them. [May 2019 CS3025 Q2a, 8M]

**1. Why CSMA/CD Fails in Wireless (Air):**
1. **Signal Attenuation**: In RF wireless channels, signal strength drops exponentially with distance (`1/d^2` to `1/d^4`). A transmitting antenna produces a local signal thousands of times stronger than any incoming distant signal. A station's own transmitter completely drowns out collision noise, making collision detection during transmission physically impossible.
2. **Hidden Terminal Problem**: Station A and Station C can both communicate with central Access Point B, but A and C are out of radio range of each other. When A transmits to B, C cannot sense A's carrier, assumes the air is idle, and transmits to B simultaneously, causing a severe collision at B!

**2. The Hidden Terminal Scenario Diagram:**
```
     [Station A] -------------> [AP Node B] <------------- [Station C]
    (Can reach B)              (In range of             (Can reach B)
                                 both A & C)
          \                                                 /
           \============= A and C cannot hear each other ==/
                                (HIDDEN TERMINALS)
```

**3. CSMA/CA Resolution via RTS/CTS Handshake:**
1. **Carrier Sensing & DIFS**: Station A senses medium. When idle for `DIFS` duration, it sends a tiny **RTS (Request to Send)** frame containing the requested duration of data transmission.
2. **CTS Broadcast**: AP Node B replies with a **CTS (Clear to Send)** frame echoing the duration.
3. **Virtual Carrier Sensing via NAV**:
   - Because Station C is in range of Node B, C overhears Node B's CTS frame!
   - Station C reads the duration field and sets its internal timer called the **NAV (Network Allocation Vector)**.
   - Station C suspends all transmission attempts until the NAV timer counts down to zero, even though its local physical carrier sensing suggests the air is silent!
4. **Data & SIFS ACK**: Station A transmits its full data frame unhindered, and Node B confirms reception with an ACK frame after a tiny `SIFS` (Short Interframe Space).

**Examiner Marking Rubric:**
* Reasons why CSMA/CD fails in wireless: 2.0 Marks.
* Hidden terminal diagram and explanation: 2.5 Marks.
* RTS/CTS frame sequence and NAV virtual carrier sensing: 3.5 Marks.

---

# UNIT 4: NETWORK LAYER & ROUTING PROTOCOLS

---

### TOPIC 11: IPv4 Header Architecture & Field Breakdown
**Exam Question Statement:**
> Draw the IPv4 Header format. Explain the purpose of each field and explain how IPv4 supports heterogeneous networks through MTU fragmentation. [May 2019 CS3025 Q4b, 8M]

**1. IPv4 Header Diagram (32-Bit Word Format):**
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|     Fragment Offset     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |        Header Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (0 to 40 Bytes)                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**2. Key Fields Functionality:**
* **Version (4 bits)**: Specifies IP protocol version (binary `0100` = IPv4).
* **IHL (Internet Header Length, 4 bits)**: Specifies header length in **4-byte words**. Minimum value = 5 (`5 * 4 = 20 Bytes`); maximum value = 15 (`15 * 4 = 60 Bytes`).
* **Type of Service / DSCP & ECN (8 bits)**: Quality of service flags, priority, delay/throughput preferences, and Explicit Congestion Notification.
* **Total Length (16 bits)**: Total size of datagram (header + data) in bytes. Maximum possible = `65,535 Bytes`.
* **Identification (16 bits)**: Unique sequence number assigned by sender to identify all fragments belonging to the same original datagram.
* **Flags (3 bits)**:
  - Bit 0: Reserved (must be 0).
  - Bit 1: **DF (Don't Fragment)**. If 1, routers cannot fragment packet; if packet exceeds MTU, router drops packet and returns ICMP Type 3 Code 4.
  - Bit 2: **MF (More Fragments)**. Set to 1 for all fragments except the last fragment (MF = 0).
* **Fragment Offset (13 bits)**: Specifies position of fragment's data relative to start of original unfragmented payload, measured strictly in **8-byte units**.
* **TTL (Time to Live, 8 bits)**: Hop count limit. Decremented by 1 at every router hop. Prevents packets from circulating indefinitely during routing loops. When TTL drops to 0, router drops packet and sends ICMP Type 11 (Time Exceeded) back to sender (basis for `traceroute`).
* **Protocol (8 bits)**: Demultiplexes payload to Transport Layer: `1 = ICMP`, `6 = TCP`, `17 = UDP`, `89 = OSPF`.
* **Header Checksum (16 bits)**: 16-bit one's complement checksum protecting the header only (recomputed at every hop because TTL decrements).
* **Source & Destination IP Addresses (32 bits each)**: End-to-end logical source and destination addresses.

**3. How Header Accommodates Heterogeneous Networks:**
Different physical link-layer technologies enforce different Maximum Transmission Units (MTUs) (e.g. Ethernet = 1500B, PPPoE = 1492B, FDDI = 4352B). Through the **Identification, Flags, and Fragment Offset** fields, intermediate IPv4 routers can transparently slice large datagrams into smaller fragments tailored to downstream MTUs, allowing arbitrary heterogeneous networks to interoperate seamlessly.

**Examiner Marking Rubric:**
* Neat 32-bit layout diagram: 2.0 Marks.
* Explanation of fields (especially IHL, TTL, Protocol, Offset): 4.0 Marks.
* Explanation of heterogeneous MTU fragmentation: 2.0 Marks.

---

### TOPIC 12: IPv4 vs IPv6 Architectural Comparison
**Exam Question Statement:**
> Compare IPv4 and IPv6 protocol architectures. Why does IPv6 eliminate header checksums and router-level fragmentation? [Practice Pattern]

**Detailed Architectural Comparison Table:**

| Feature | IPv4 | IPv6 |
| :--- | :--- | :--- |
| **Address Length** | 32 bits (4.29 billion addresses, exhausted) | **128 bits** ($3.4 \times 10^{38}$ addresses, infinite) |
| **Address Notation** | Dotted-decimal: `192.168.1.1` | Hexadecimal colon: `2001:0db8:85a3::8a2e:0370:7334` |
| **Base Header Size** | Variable: **20 to 60 Bytes** (depends on IHL/Options) | **Fixed 40 Bytes** (8 streamlined fields) |
| **Header Checksum** | **Present** (recalculated at every hop) | **ELIMINATED** (relies on L2 CRC and L4 TCP/UDP checksums) |
| **Router Fragmentation** | **Supported** (intermediate routers fragment) | **PROHIBITED** (Source-only fragmentation via PMTUD) |
| **Options Handling** | Embedded inside variable base header | Daisy-chained **Extension Headers** via `Next Header` field |
| **Address Resolution** | Uses **ARP** (broadcast-heavy) | Replaced by **ICMPv6 Neighbor Discovery (NDP)** (multicast) |
| **Host Auto-Configuration**| Manual static or stateful **DHCP** | Supports stateless auto-configuration (**SLAAC**) |
| **Broadcast Support** | Native Broadcast (`255.255.255.255`) | **ELIMINATED** (replaced by targeted Multicast & Anycast) |

**Engineering Justifications for IPv6 Innovations:**
1. **Elimination of Header Checksum**: In modern fiber/copper links, Layer 2 (Ethernet CRC-32) and Layer 4 (TCP/UDP checksum) already provide robust integrity checks. Recomputing checksums at every intermediate router was a severe CPU bottleneck that prevented routers from achieving line-speed forwarding. Removing it enables blazing-fast hardware routing.
2. **Elimination of Router Fragmentation**: Slicing packets in software at intermediate routers consumed massive router memory and caused packet buffer bloat. IPv6 forces hosts to use **Path MTU Discovery (PMTUD)**: sender transmits packets of increasing size and listens for ICMPv6 "Packet Too Big" messages to determine the optimal MTU upfront.

**Examiner Marking Rubric:**
* Comparison table with at least 5 key parameters: 2.5 Marks.
* Justification for removing header checksum: 1.5 Marks.
* Justification for eliminating router fragmentation: 1.0 Mark.

---

### TOPIC 13: Address Resolution Protocol (ARP) Operation
**Exam Question Statement:**
> Give the packet format of ARP protocol. How does ARP support address resolution within an Intranet? What is Gratuitous ARP and ARP Spoofing? [May 2019 CS3025 Q3c, 5M]

**1. ARP Packet Structure:**
```
+--------------------------------+--------------------------------+
|  Hardware Type (Ethernet = 1)  |   Protocol Type (IPv4 = 0800)  |
+--------------------------------+--------------------------------+
| HLEN (MAC = 6) | PLEN (IP = 4) |    Operation (1=Req, 2=Reply)  |
+--------------------------------+--------------------------------+
|                 Sender Hardware Address (MAC, Bytes 0-3)        |
+--------------------------------+--------------------------------+
| Sender MAC (Bytes 4-5)         | Sender Protocol Address (IP 0-1|
+--------------------------------+--------------------------------+
| Sender IP (Bytes 2-3)          | Target Hardware Address (MAC)  |
+--------------------------------+--------------------------------+
| Target MAC (Bytes 2-5)                                          |
+--------------------------------+--------------------------------+
|                 Target Protocol Address (IP, Bytes 0-3)         |
+--------------------------------+--------------------------------+
```

**2. Address Resolution Mechanism:**
* When Host A (`192.168.1.10`) wants to deliver a packet to Host B (`192.168.1.20`) on the same subnet, it checks its local in-memory **ARP Cache**.
* On cache miss, Host A broadcasts an **ARP Request**:
  - Ethernet Frame Dest: `FF:FF:FF:FF:FF:FF` (Broadcast).
  - Message: *"Who has IP 192.168.1.20? Tell 192.168.1.10."*
* Every station receives the broadcast. All stations inspect the target IP and drop the packet, except Host B.
* Host B sends back a unicast **ARP Reply** directly to Host A's MAC address:
  - Message: *"I have 192.168.1.20; my MAC is 00:1A:2B:3C:4D:5E."*
* Host A caches the IP-to-MAC mapping and encapsulates the IP datagram into an Ethernet frame.

**3. Gratuitous ARP & ARP Spoofing:**
* **Gratuitous ARP**: A host broadcasts an ARP request for its *own* IP address upon bootup. Purposes: (1) Detects duplicate IP address conflicts on the subnet; (2) Updates neighbor switches' CAM tables after NIC replacement.
* **ARP Spoofing / Poisoning**: ARP is stateless and has zero authentication. An attacker broadcasts forged ARP replies linking the Default Gateway's IP to the attacker's MAC address. All subnet hosts redirect their external Internet traffic through the attacker's machine, enabling Man-in-the-Middle (MITM) eavesdropping. Mitigation: **Dynamic ARP Inspection (DAI)** on switches.

**Examiner Marking Rubric:**
* ARP packet fields diagram: 1.5 Marks.
* Step-by-step resolution flow (Broadcast Request, Unicast Reply): 2.0 Marks.
* Gratuitous ARP and ARP spoofing explanation: 1.5 Marks.

---

### TOPIC 14: Distance Vector Routing & Count-to-Infinity Problem
**Exam Question Statement:**
> Explain the working of Distance Vector Routing. Describe the Count-to-Infinity problem with a diagram and explain how Split Horizon and Poison Reverse resolve it. Why is RIP limited to 15 hops? [May 2019 CS3025 Q6b, 5M / Nov 2024 IT2266 Q4a]

**1. Working Mechanism:**
Distance Vector (Bellman-Ford) routing operates on the distributed principle: *"Tell your neighbors about the entire world."* Each router maintains a routing table containing destination, cost, and next-hop. Periodically (every 30s in RIP), every router broadcasts its entire routing table to its immediate physical neighbors only.

**2. The Count-to-Infinity Problem:**
```
     [Node A] <----(1)----> [Node B] <----(1)----> [Node C]
```
1. Initially, link costs are: `A-B = 1`, `B-C = 1`. Router B reaches A with cost 1; Router C reaches A via B with cost 2.
2. Suppose the physical link `A-B` breaks.
3. Router B immediately knows A is unreachable (`cost = infinity`).
4. However, before B can propagate this, Router C advertises its vector claiming: *"I can reach A with cost 2."*
5. Router B receives this outdated rumor, mistakenly assumes an alternate path to A exists through C, and updates: `Dist(A) = 2 + 1 = 3 via C`.
6. On the next cycle, C hears B has cost 3, so C updates: `Dist(A) = 3 + 1 = 4 via B`.
7. The two routers bounce packets back and forth, slowly incrementing the metric (`5, 6, 7...`) in a routing loop until it counts to infinity!

**3. Solutions:**
* **Split Horizon**: A router must **never advertise a route back out the same physical interface** through which it learned that route! (C learned about A from B, so C will never advertise A to B).
* **Poison Reverse**: An aggressive enhancement to Split Horizon. Instead of omitting the route, C advertises the route back to B with an explicit metric of **Infinity (`16`)**: *"I can reach A via you, so for you, my cost to A is infinity."* This immediately breaks the two-node loop.
* **Why RIP is Restricted to 15 Hops**: To ensure that if a loop does occur, the Count-to-Infinity process terminates in reasonable time. A metric of **16 denotes infinity (unreachable)**, limiting RIP strictly to small autonomous local networks.

**Examiner Marking Rubric:**
* Distance Vector basic update rule: 1.0 Mark.
* Clear step-by-step diagram of Count-to-Infinity: 2.0 Marks.
* Split Horizon & Poison Reverse mechanisms: 1.5 Marks.
* Reason for 15-hop limit in RIP: 0.5 Mark.

---

### TOPIC 15: Link State Routing (Dijkstra SPF) & OSPF Architecture
**Exam Question Statement:**
> Explain Link State Routing. How does OSPF establish routing tables using Dijkstra's Shortest Path First algorithm? Differentiate between Distance Vector and Link State protocols. [May 2019 CS3025 Q5b, 8M]

**1. Working Principle of Link State Routing:**
Link State routing operates on the complementary principle: *"Tell the entire world about your immediate neighbors."*
1. **Neighbor Discovery**: Routers exchange `HELLO` packets to detect active neighbors.
2. **Link State Packet (LSP) Generation**: Each router constructs a compact LSP containing its ID, directly attached neighbors, and current link costs.
3. **Reliable Flooding**: Routers flood LSPs to all routers in the autonomous system using reliable flooding.
4. **LSDB Synchronization**: Every router compiles an identical graph of the entire network topology called the **Link State Database (LSDB)**.
5. **Dijkstra SPF Execution**: Each router independently executes Dijkstra's Shortest Path First algorithm using itself as the root to compute a loop-free Shortest Path Tree and formulate its IP forwarding table.

**2. OSPF Hierarchical Architecture:**
To scale across massive enterprise networks, OSPF divides the autonomous system into **Areas**:
* **Backbone Area (Area 0)**: The core transit area connecting all other areas.
* **Internal Routers**: All interfaces belong to a single non-backbone area.
* **Area Border Routers (ABRs)**: Routers attached to both Area 0 and a non-backbone area; aggregate and summarize topology information.
* **Autonomous System Boundary Routers (ASBRs)**: Routers that exchange routing information with external networks (e.g. BGP).

**3. Distance Vector vs Link State Comparison:**

| Parameter | Distance Vector (RIP) | Link State (OSPF) |
| :--- | :--- | :--- |
| **Topology View** | Blind (knows only neighbor rumors) | Complete global map (identical LSDB) |
| **Routing Algorithm**| Bellman-Ford | Dijkstra Shortest Path First (SPF) |
| **Exchange Scope** | Entire table sent to immediate neighbors only | Small LSPs flooded to all routers |
| **Convergence Speed**| Very slow (suffers Count-to-Infinity) | Extremely fast (converges in seconds) |
| **Loop Vulnerability**| High (routing loops can form) | Loop-free (Dijkstra tree has no cycles) |
| **Resource Usage** | Low CPU and memory | High memory (LSDB) and CPU (SPF recalculation) |
| **Metric Used** | Hop count strictly (max 15) | Cost based on bandwidth: `10^8 / Bandwidth` |

**Examiner Marking Rubric:**
* 5 steps of Link State operation: 2.5 Marks.
* OSPF hierarchical areas (Area 0, ABR, ASBR): 2.5 Marks.
* Comparison table with at least 4 parameters: 3.0 Marks.

---

### TOPIC 16: Traffic Shaping: Leaky Bucket vs Token Bucket
**Exam Question Statement:**
> Explain traffic shaping techniques. Compare Leaky Bucket and Token Bucket algorithms. How does a token bucket accommodate bursty traffic while maintaining an average rate? [May 2019 / Practice Pattern]

**1. Concepts & Diagrams:**
```
       LEAKY BUCKET ALGORITHM                   TOKEN BUCKET ALGORITHM
    +--------------------------+             +--------------------------+
    |   Bursty Packet Input    |             |  Token Generator (Rate r)|
    +--------------------------+             +--------------------------+
                 |                                         |
          +------v------+                           +------v------+
          | \          / |                          | \  Tokens  / | (Capacity b)
          |  \ Buffer /  |                          |  \________/  |
          |   \______/   |                          +------+-------+
          +------+-------+                                 |
                 | (Fixed Leak Hole)                +------v------+
          +------v------+                           | Bursty Data | Token matches
          | Strict Constant                         | Packet Queue| packet size
          | Smooth Rate R |                         +------+-------+
          +-------------+                                  |
                                                    +------v------+
                                                    | Controlled  |
                                                    | Bursts Out  |
                                                    +-------------+
```

**2. Detailed Comparison:**
* **Leaky Bucket**:
  - Treats packets like water entering a bucket with a fixed hole at the bottom.
  - Regardless of how bursty the incoming traffic is, the output rate is strictly **constant and uniform**.
  - If incoming bursts exceed the bucket capacity, excess packets overflow and are discarded.
  - *Drawback*: Completely suppresses bursts even when the downstream network has ample idle capacity. Unsuitable for bursty web/file traffic.
* **Token Bucket**:
  - The bucket holds **tokens** generated at a constant rate `r` tokens/sec up to capacity `b`.
  - When a packet arrives, it consumes tokens proportional to its byte length and transmits immediately.
  - If the bucket is full of tokens, a burst of accumulated packets can transmit at the maximum physical link rate `M` for a duration of `S = b / (M - r)`.
  - When tokens are exhausted, traffic is throttled to the average arrival rate `r`.
  - *Advantage*: Perfectly accommodates bursty applications without dropping packets while enforcing a strict long-term average bandwidth limit.

**Examiner Marking Rubric:**
* Clean diagrams of Leaky vs Token Bucket: 2.0 Marks.
* Explanation of burst handling and average rate enforcement: 2.0 Marks.
* Mathematical formula for burst duration `S = b / (M - r)`: 1.0 Mark.

---

# UNIT 5: TRANSPORT LAYER & CONGESTION CONTROL

---

### TOPIC 17: TCP vs UDP Protocol Architectures
**Exam Question Statement:**
> Compare TCP and UDP in detail. Explain the scenarios where UDP is preferred over TCP despite having no flow or error control. [Nov 2024 IT2266 Q5a, Q5b, 4M / May 2019 CS3025 Q4d, 3M]

**1. Comparative Analysis Matrix:**

| Feature | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Connection Model** | Connection-oriented (requires 3-way handshake) | Connectionless (sends immediately with 0 setup) |
| **Reliability** | 100% reliable (acknowledgments, retransmissions) | Best-effort (no ACKs, packets can drop/duplicate) |
| **Data Stream Format**| Continuous, unstructured **Byte Stream** | Discrete, independent **Datagram Messages** |
| **Ordering** | In-order guaranteed (sequence numbers reorder) | No ordering (packets can arrive out-of-order) |
| **Flow & Congestion** | Full sliding window flow control + AIMD congestion | **None** (injects packets as fast as app generates) |
| **Header Overhead** | **20 to 60 Bytes** (complex flags and options) | Fixed **8 Bytes** strictly (minimal overhead) |
| **Communication** | Strictly Point-to-Point Unicast (1 sender, 1 receiver)| Supports Unicast, Multicast, and Broadcast |
| **Speed & Latency** | Higher latency (handshake RTTs + retransmissions) | Extremely fast and low latency |

**2. Why UDP is Preferred in Modern Applications:**
1. **Real-Time Multimedia Streaming (VoIP, Video Conferences, Online Games)**: Human ears and eyes tolerate 1-2% packet loss without noticeable disruption. However, TCP's retransmission delays cause audio freezing, stutter, and intolerable lag. Dropping a lost audio packet is far better than playing it 500 ms late!
2. **Simple Request-Response Protocols (DNS, DHCP, NTP)**: The transaction takes a single request and a single response. Performing a 3-way TCP handshake (3 packets) to send a 50-byte DNS query wastes bandwidth and introduces unnecessary RTT latency.
3. **Broadcasting & Discovery**: Routing protocols (RIP), address configuration (DHCP), and LAN discovery require broadcasting to `255.255.255.255`, which TCP cannot do.
4. **Custom Reliability in User-Space (QUIC / HTTP/3)**: QUIC runs over UDP Port 443, implementing modern encryption (TLS 1.3) and independent multiplexed streams without TCP Head-of-Line blocking.

**Examiner Marking Rubric:**
* Comparison table with at least 5 key parameters: 2.0 Marks.
* Justification of why UDP is chosen for real-time and DNS/DHCP: 2.0 Marks.

---

### TOPIC 18: TCP 3-Way Handshake & The Delayed Duplicate FIN Mystery
**Exam Question Statement:**
> Illustrate the TCP 3-Way Handshake with a neat sequence ladder diagram. Why is a 2-way handshake insufficient? Suppose a FIN packet from an old closed connection is delayed and delivered to a new connection on the same socket pair; will TCP terminate the new connection? Justify. [May 2019 CS3025 Q4a, 8M / Nov 2024 IT2266 Q4b-OR, 4M]

**1. TCP 3-Way Handshake Sequence Diagram:**
```
  Client Host                                          Server Host
       |                                                    |
       |  1. [SYN] Seq = x                                  |
       |--------------------------------------------------->| (Allocates TCB memory,
       |                                                    |  sets state to SYN_RCVD)
       |  2. [SYN + ACK] Seq = y, Ack = x + 1               |
       |<---------------------------------------------------|
       |                                                    |
       |  3. [ACK] Seq = x + 1, Ack = y + 1                 |
       |--------------------------------------------------->|
 (ESTABLISHED)                                        (ESTABLISHED)
       |                                                    |
       |============= Bidirectional Byte Stream ===========>|
```

**2. Why a 2-Way Handshake Fails:**
In a 2-way handshake, the connection would be established as soon as the server sends segment 2.
* *The Zombie Connection Disaster*: Suppose Client A sends a `SYN` that gets delayed inside congested network routers. Client A times out, abandons the request, and closes. Hours later, the delayed duplicate `SYN` finally arrives at Server B. Under a 2-way handshake, Server B replies with `SYN+ACK` and immediately enters `ESTABLISHED`, allocating memory and socket buffers for a client that has long ceased to exist! This creates a **half-open zombie connection** that ties up server resources indefinitely.
* In a 3-way handshake, Server B must wait for Client A's final ACK. When Client A receives an unexpected `SYN+ACK`, it rejects it with a `RST` (Reset) packet, terminating the zombie session.

**3. The Delayed Duplicate FIN Puzzle (Nov 2024 Exam Question):**
* **The Question**: If a duplicate `FIN` from connection 1 is delayed in the network and arrives after connection 2 is established between the same two hosts on the same port pair, will TCP terminate the new connection?
* **The Answer**: **NO, TCP WILL NOT TERMINATE THE NEW CONNECTION!**
* **Technical Justification**:
  1. **Clock-Driven Initial Sequence Numbers (ISNs)**: TCP endpoints do not start connections at sequence number 0. Modern TCP implementations (RFC 6528) use a 4-microsecond clock-driven PRNG generator. The delayed duplicate `FIN` carries a sequence number belonging to the *old* connection's sequence space, which falls completely outside the valid receive window (`rcv_wnd`) of the new connection. TCP silently discards out-of-window control segments.
  2. **TIME_WAIT State Protection ($2 \times \text{MSL}$)**: The actively closing endpoint of the first connection is quarantined in the `TIME_WAIT` state for $2 \times \text{Maximum Segment Lifetime}$ (typically 120 seconds) before the port tuple can be reused. This guarantees that all delayed wandering duplicate segments die inside network router buffers before a new connection can open.
  3. **Ephemeral Port Allocation**: The OS binds a new, different ephemeral port number for the second connection, making the 4-tuple mismatch entirely.

**Examiner Marking Rubric:**
* Clean 3-way handshake diagram with Seq/Ack numbers: 2.0 Marks.
* Explanation of why 2-way handshake creates half-open zombie sessions: 2.0 Marks.
* Delayed FIN analysis (ISN window check + TIME_WAIT): 4.0 Marks.

---

### TOPIC 19: Silly Window Syndrome (SWS): Nagle's & Clark's Solutions
**Exam Question Statement:**
> What is Silly Window Syndrome? How does it degrade network performance? Explain Nagle's Algorithm at the sender and Clark's Solution at the receiver. [Practice Pattern]

**1. The Problem Statement:**
Silly Window Syndrome is a serious performance degradation in TCP where data is transmitted in tiny, micro-sized chunks (e.g. 1 byte of payload) wrapped in full 40-byte TCP/IP headers.
* **Sender-Driven SWS**: An application generates data 1 byte at a time (e.g. keystrokes in Telnet). If TCP sends each byte immediately, network efficiency drops to:
  `Efficiency = 1 Byte / (20B IP + 20B TCP + 1B Data) = 1 / 41 = 2.44%`!
  Over 97.5% of network bandwidth is consumed by pure protocol overhead!
* **Receiver-Driven SWS**: A slow receiver reads data 1 byte at a time from its buffer. As soon as 1 byte is freed, it advertises `rwnd = 1` to the sender, tempting the sender to immediately transmit another 1-byte segment.

**2. The Solutions:**
* **Nagle's Algorithm (Sender-Side Solution)**:
  - If the application generates data in small chunks:
    1. Send the first small chunk immediately.
    2. Buffer all subsequent application bytes until **either**:
       - The accumulated data fills a full **MSS (Maximum Segment Size)**, OR
       - The sender receives an ACK for the previously transmitted segment.
  - Automatically adapts: on fast links with low RTT, ACKs return quickly and data flows smoothly; on congested slow links, data accumulates into large, efficient full-MSS packets.
* **Clark's Solution (Receiver-Side Solution)**:
  - Prevents the receiver from advertising small window increments.
  - The receiver is forced to advertise `rwnd = 0` (window closed) until its internal receive buffer has freed up at least:
    `Free Buffer >= min(MSS, Total Buffer Size / 2)`.
  - Once this threshold is crossed, the receiver sends a window update advertising a large, respectable window.

**Examiner Marking Rubric:**
* Definition and 2.44% efficiency calculation: 1.5 Marks.
* Nagle's algorithm rule (MSS or previous ACK): 1.5 Marks.
* Clark's solution rule (min of MSS or half buffer): 1.0 Mark.

---

### TOPIC 20: TCP Congestion Control State Machine: Tahoe vs Reno
**Exam Question Statement:**
> Explain TCP Congestion Control using AIMD (Additive Increase Multiplicative Decrease). Trace the states: Slow Start, Congestion Avoidance, Fast Retransmit, and Fast Recovery. Contrast TCP Tahoe and TCP Reno. [Nov 2024 / Practice Pattern]

**1. AIMD State Machine Diagram:**
```
     cwnd (MSS)
         |                                  Timeout (Drops to 1)
      16 |                  +-----+          /
         |                 /       \        /
      12 |                /         \      /
         |               /           +----+ (Reno Fast Recovery to ssthresh=8)
       8 |       +------+  ssthresh=8 \
         |      / (Slow Start)         \
       4 |     /                        \
       1 +----+                          +----+
         +----------------------------------------> Time (RTTs)
```

**2. State-by-State Breakdown:**
1. **Slow Start**:
   - Initial state: `cwnd = 1 MSS`.
   - For every ACK received, `cwnd` increases by `1 MSS` (doubles every RTT: $1 \to 2 \to 4 \to 8$).
   - Continues exponential growth until `cwnd >= ssthresh` (slow-start threshold).
2. **Congestion Avoidance**:
   - Entered when `cwnd >= ssthresh`.
   - Growth switches from exponential to conservative **linear growth**: `cwnd` increases by `1 MSS` per RTT (Additive Increase).
   - Probes for available bandwidth safely without inducing packet loss.
3. **Loss Event via Timeout (Severe Congestion)**:
   - When a retransmission timer expires:
     - `ssthresh = max(cwnd / 2, 2 MSS)`.
     - Reset `cwnd = 1 MSS`.
     - Enter **Slow Start** immediately (applies to both Tahoe and Reno).
4. **Loss Event via 3 Duplicate ACKs (Mild Congestion / Isolated Drop)**:
   - When 3 duplicate ACKs arrive, the sender knows packets are still flowing through the network (triggering Fast Retransmit of the missing packet).
   - **TCP Tahoe Behavior**: Drops `cwnd = 1 MSS` and reverts to Slow Start (wastefully penalizing throughput).
   - **TCP Reno Behavior (Fast Recovery)**:
     - `ssthresh = cwnd / 2`.
     - Sets **`cwnd = ssthresh`** (or `ssthresh + 3 MSS`).
     - Stays directly in **Congestion Avoidance** without dropping to 1! This prevents throughput collapse on isolated packet drops.

**Examiner Marking Rubric:**
* Explanation of Slow Start and Congestion Avoidance: 2.0 Marks.
* Mathematical rules for Timeout vs 3 Duplicate ACKs: 1.5 Marks.
* Precise distinction between Tahoe (drops to 1) and Reno (Fast Recovery): 1.5 Marks.

---

# UNIT 6: APPLICATION LAYER & EMERGING PROTOCOLS

---

### TOPIC 21: Domain Name System (DNS) Architecture & Resolution
**Exam Question Statement:**
> Explain the hierarchical architecture of DNS. Illustrate how `www.vit.edu` is resolved into an IP address using recursive and iterative queries. Describe A, AAAA, CNAME, and MX resource records. [Nov 2024 IT2266 Q6a, 4M / May 2019 CS3025 Q4c, 5M]

**1. Hierarchical Tree Architecture:**
```
                           [Root DNS Servers "."]
                                     |
               +---------------------+---------------------+
               |                     |                     |
          [.com TLD]            [.edu TLD]            [.org TLD]
               |                     |                     |
         [google.com]           [vit.edu]             [ieee.org]
                                     |
                               [www.vit.edu]
```

**2. Step-by-Step Resolution of `www.vit.edu`:**
1. Client checks local browser cache, OS resolver cache, and `hosts` file.
2. Client issues a **Recursive Query** to the campus Local DNS Server (LDNS). (In recursive query, the burden of finding the answer is entirely placed on the queried server).
3. LDNS issues an **Iterative Query** to one of the 13 global Root DNS Servers (`.`). Root server returns a referral NS record pointing to the **`.edu` Top-Level Domain (TLD) Name Servers**.
4. LDNS queries the `.edu` TLD Server. The TLD server returns a referral pointing to the **Authoritative DNS Server for `vit.edu`**.
5. LDNS queries the Authoritative Name Server for `vit.edu`.
6. The Authoritative Server returns the **`A` Record**: `www.vit.edu -> 103.14.120.5` with a Time-to-Live (TTL) value.
7. LDNS caches the record in memory and returns the IP address to the client.

**3. Resource Record (RR) Types:**
* **`A` Record**: Maps a canonical hostname to a 32-bit **IPv4 address** (e.g. `vit.edu -> 103.14.120.5`).
* **`AAAA` ("Quad-A") Record**: Maps a hostname to a 128-bit **IPv6 address**.
* **`CNAME` (Canonical Name)**: Alias pointing one domain name to another domain name (e.g. `web.vit.edu CNAME www.vit.edu`).
* **`MX` (Mail Exchanger)**: Specifies the mail server responsible for receiving emails for the domain along with a priority integer.
* **`PTR` (Pointer Record)**: Used in reverse DNS lookups to map an IP address back to its hostname.

**Examiner Marking Rubric:**
* Hierarchical tree diagram: 1.0 Mark.
* Step-by-step resolution flow with recursive vs iterative distinction: 2.0 Marks.
* Explanation of A, AAAA, CNAME, and MX records: 1.0 Mark.

---

### TOPIC 22: HTTP Evolution: HTTP/1.0 vs HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC)
**Exam Question Statement:**
> Trace the evolution of HTTP from HTTP/1.0 to HTTP/3. Explain persistent connections, pipelining, multiplexing, and how HTTP/3 over QUIC solves Head-of-Line blocking. [Nov 2024 IT2266 Q6b, 3M / Practice Pattern]

**Evolutionary Comparison Matrix:**

| Feature | HTTP/1.0 | HTTP/1.1 | HTTP/2 | HTTP/3 |
| :--- | :--- | :--- | :--- | :--- |
| **Transport Layer** | TCP Port 80 | TCP Port 80 | TCP Port 443 | **QUIC over UDP Port 443** |
| **Connection Model** | **Non-Persistent**: 1 TCP connection per object | **Persistent**: Single long-lived connection | **Persistent**: Single long-lived connection | **Connectionless UDP**: 0-RTT/1-RTT setup |
| **Handshake Latency**| $2 \times \text{RTT}$ per object | $2 \times \text{RTT}$ initially, $1 \times \text{RTT}$ subsequent | $2 \times \text{RTT}$ initially | **0-RTT to 1-RTT** (built-in TLS 1.3) |
| **Framing Format** | Plain Text ASCII | Plain Text ASCII | **Binary Framing Layer** | **Binary Framing over QUIC** |
| **Multiplexing** | None | Pipelining (rarely deployed) | **True Stream Multiplexing** | **Independent Non-Blocking Streams** |
| **Head-of-Line (HoL) Blocking**| Severe at TCP layer | Severe at HTTP layer (serial responses) | Eliminated at HTTP layer; **Stuck at TCP layer** | **COMPLETELY ELIMINATED** across all layers |
| **Header Compression**| None | None | **HPACK** | **QPACK** |
| **Connection Migration**| Drops on IP switch | Drops on IP switch | Drops on IP switch | **Seamless** (tracked by 64-bit Connection ID) |

**Understanding Head-of-Line (HoL) Blocking:**
* In **HTTP/1.1**, responses had to be delivered in the strict serial order of their requests. If request 1 took a long time, requests 2 and 3 were blocked behind it.
* In **HTTP/2**, application streams are multiplexed into binary frames over a single TCP socket. However, because TCP enforces strict byte-stream ordering, if a single IP packet carrying frames for Stream 1 is dropped in the network, TCP halts delivery of *all* other streams until the lost packet is retransmitted! This is **Transport-Layer HoL Blocking**.
* In **HTTP/3 (QUIC)**, each stream is independent. If a packet in Stream 1 is lost, only Stream 1 pauses; Streams 2, 3, and 4 continue rendering without delay!

**Examiner Marking Rubric:**
* Evolution steps (HTTP/1.0 to 1.1 persistence, HTTP/2 multiplexing, HTTP/3 QUIC): 2.0 Marks.
* Explanation of Head-of-Line blocking resolution: 1.0 Mark.

---

### TOPIC 23: Dynamic Host Configuration Protocol (DHCP) & DORA Process
**Exam Question Statement:**
> How does DHCP automatically assign an IP address to a client laptop? Explain the 4-step DORA process with packet directions and port numbers. [Nov 2024 IT2266 Q5c, 4M / May 2019 CS3025 Q5c, 5M]

**1. The 4-Step DORA Protocol Sequence:**
```
     Client (Port 68)                                   DHCP Server (Port 67)
           |                                                      |
           |  1. DHCP DISCOVER (Broadcast)                        |
           |  Src: 0.0.0.0:68  Dest: 255.255.255.255:67           |
           |----------------------------------------------------->|
           |                                                      |
           |  2. DHCP OFFER (Unicast / Broadcast)                 |
           |  Offered IP, Mask, Gateway, DNS, Lease Duration      |
           |<-----------------------------------------------------|
           |                                                      |
           |  3. DHCP REQUEST (Broadcast)                         |
           |  Src: 0.0.0.0:68  Dest: 255.255.255.255:67           |
           |  "I accept Server 1's offer!"                        |
           |----------------------------------------------------->|
           |                                                      |
           |  4. DHCP ACK (Unicast / Broadcast)                   |
           |  Lease confirmed & committed to database             |
           |<-----------------------------------------------------|
```

**2. Detailed Step Descriptions:**
1. **D - DHCP DISCOVER**: Client boots up with no IP address (`0.0.0.0`). It broadcasts a packet to `255.255.255.255` on UDP Port 67 containing its physical MAC address and a random transaction ID (`xid`) to discover available DHCP servers.
2. **O - DHCP OFFER**: All listening DHCP servers reserve an unallocated IP from their address pool and reply with an offer packet containing: Offered IP, Subnet Mask, Default Gateway, DNS Servers, and Lease Time (e.g. 8 hours).
3. **R - DHCP REQUEST**: Client chooses one offer (typically the first received) and **broadcasts** an acceptance packet. Why broadcast? Because broadcasting informs the chosen server that its offer was accepted, while simultaneously informing other servers to release their reserved offers back to their available pools!
4. **A - DHCP ACK**: The selected server commits the binding to its lease database and returns a DHCP ACK packet. The client configures its network interface.

**Examiner Marking Rubric:**
* Sequence diagram showing DORA with UDP ports (67/68): 2.0 Marks.
* Step-by-step explanation of each phase: 1.5 Marks.
* Explanation of why DHCPREQUEST is broadcast: 0.5 Mark.

---

### TOPIC 24: Electronic Mail Architecture: SMTP, POP3, IMAP & MIME
**Exam Question Statement:**
> Explain the electronic mail architecture. Differentiate between message transfer protocols (SMTP) and message access protocols (POP3 vs IMAP). Why is MIME essential for modern email? [May 2019 CS3025 Q5a, 8M]

**1. Email System Architecture:**
```
 [Sender MUA] --(SMTP:25/587)--> [Sender MTA Server]
                                          |
                              (SMTP:25 Server-to-Server)
                                          v
 [Receiver MUA] <--(POP3:110 / IMAP:143)-- [Receiver MTA / MDA Server]
```
* **MUA (Mail User Agent)**: Client software (Outlook, Thunderbird, Apple Mail).
* **MTA (Mail Transfer Agent)**: Server software (Postfix, Sendmail, Exchange).

**2. SMTP vs POP3 vs IMAP:**
* **SMTP (Simple Mail Transfer Protocol - Port 25 / 587)**:
  - A **Push Protocol**. Used to upload mail from client to server, and relay mail between intermediate mail servers across the Internet.
  - Plain-text ASCII protocol; terminates connection upon delivery.
* **POP3 (Post Office Protocol v3 - Port 110 / 995)**:
  - A simple **Pull Protocol**. Downloads all incoming emails from server to client hard drive and (by default) deletes them from the server mailbox.
  - *Limitation*: No synchronization across multiple devices (phone, laptop).
* **IMAP (Internet Message Access Protocol - Port 143 / 993)**:
  - An advanced **Pull & Sync Protocol**. Keeps all emails and folder hierarchies stored permanently on the central server.
  - Allows searching, flagging, and previewing headers before downloading bodies. Changes made on a phone sync instantly to a laptop.

**3. Why MIME is Essential:**
* Traditional SMTP (RFC 821) was strictly designed for **7-bit printable US-ASCII text**.
* If a user attempts to send binary files (images, audio, PDFs, Word docs) or non-English characters (Hindi, Chinese, UTF-8), the high bit (8th bit) is stripped by legacy SMTP servers, completely corrupting the file!
* **MIME (Multipurpose Internet Mail Extensions - RFC 2045)** solves this by defining headers:
  - `MIME-Version: 1.0`
  - `Content-Type: image/jpeg` (specifies media type)
  - `Content-Transfer-Encoding: base64` (encodes arbitrary 8-bit binary bytes into safe 7-bit ASCII characters).

**Examiner Marking Rubric:**
* Email architecture diagram (MUA, MTA, MDA): 2.0 Marks.
* Distinction between Push (SMTP) and Pull (POP3 vs IMAP): 3.0 Marks.
* Explanation of MIME 7-bit limitation and Base64 solution: 3.0 Marks.

---

### TOPIC 25: File Transfer Protocol (FTP): Dual Connection Architecture
**Exam Question Statement:**
> Explain the dual-connection architecture of FTP. Differentiate between Active Mode and Passive Mode (`PASV`). Why do client firewalls block Active FTP? [Practice Pattern]

**1. Dual-Connection Architecture:**
FTP does not mix control commands and data payloads on the same TCP socket. It establishes **two separate connections**:
1. **Control Connection (Port 21)**: Remains open throughout the entire FTP session. Transmits user authentication, navigation commands (`USER`, `PASS`, `CWD`, `QUIT`), and 3-digit server response codes.
2. **Data Connection (Port 20 or dynamic)**: Created on-demand for every individual directory listing (`LIST`) or file transfer (`RETR`, `STOR`), and closes immediately once the file is transferred.

**2. Active vs Passive Mode:**
* **Active Mode (Standard)**:
  1. Client connects from ephemeral port `N` to Server Port 21 (Control).
  2. Client sends `PORT IP,P` command telling server: *"Connect to my port P for data."*
  3. **The Trap**: The **Server initiates the data connection from its Port 20 to Client Port P**!
  4. *Why Client Firewalls Block It*: To the client's home/corporate firewall, an unrequested incoming TCP connection from the Internet into port `P` looks like a malicious port scan / attack. The firewall drops the packet, and file transfer fails!
* **Passive Mode (`PASV`)**:
  1. Client sends `PASV` command over Port 21.
  2. Server opens an unreserved high ephemeral port (e.g. 50000) and replies: *"Connect to my port 50000."*
  3. The **Client initiates both connections** (Control to 21, Data to 50000).
  4. Since all connections originate outbound from client to server, client firewalls allow them smoothly.

**Examiner Marking Rubric:**
* Explanation of Port 21 (Control) vs Port 20 (Data): 2.0 Marks.
* Contrast of Active vs Passive connection initiation: 2.0 Marks.
* Explanation of client firewall blockage in Active Mode: 1.0 Mark.

---

### TOPIC 26: Sliding Window Protocol: Implementation Logic & Complete Pseudocode
**Exam Question Statement:**
> Write pseudo code to implement Sliding window protocol in detail, test code with example. [Nov 2024 IT2266 Q2b, 6M]

**1. Architectural Design & Window Management:**
Sliding window protocols achieve pipelined data transmission by allowing the sender to transmit up to N frames without waiting for individual acknowledgments.
* `base`: Sequence number of the oldest unacknowledged frame.
* `next_seq_num`: Next available sequence number to be assigned to an outgoing frame.
* `N`: Sender window size (Ws).
* Sliding Condition: When an acknowledgment for `base` arrives, `base` advances forward (`base = ack_num + 1`), allowing new frames to be transmitted.

**2. Sender Pseudocode (Go-Back-N / Sliding Window):**
```c
// SENDER GLOBAL VARIABLES
int base = 0;
int next_seq_num = 0;
int N = 4; // Window size
Frame window_buffer[MAX_SEQ];

void Sender_Init() {
    base = 0;
    next_seq_num = 0;
}

// EVENT 1: Application layer passes packet to send
void From_Network_Layer(Packet packet) {
    if (next_seq_num < base + N) {
        window_buffer[next_seq_num] = make_frame(next_seq_num, packet);
        send_to_physical_layer(window_buffer[next_seq_num]);
        if (base == next_seq_num) {
            start_timer(); // Start timer for the oldest unACKed frame
        }
        next_seq_num++;
    } else {
        refuse_data(packet); // Window is full, buffer or block
    }
}

// EVENT 2: Valid ACK arrives from receiver
void From_Physical_Layer(Frame ack_frame) {
    if (is_valid_ack(ack_frame) && ack_frame.ack_no >= base) {
        base = ack_frame.ack_no + 1; // Slide the window forward
        if (base == next_seq_num) {
            stop_timer(); // All transmitted frames acknowledged
        } else {
            restart_timer(); // Restart timer for newly oldest unACKed frame
        }
    }
}

// EVENT 3: Timeout occurs
void Timeout_Handler() {
    start_timer();
    // Retransmit all unacknowledged frames currently in window
    for (int i = base; i < next_seq_num; i++) {
        send_to_physical_layer(window_buffer[i]);
    }
}
```

**3. Receiver Pseudocode (Cumulative ACK):**
```c
// RECEIVER GLOBAL VARIABLES
int expected_seq_num = 0;

void Receiver_Init() {
    expected_seq_num = 0;
}

void Receiver_From_Physical_Layer(Frame received_frame) {
    if (is_checksum_valid(received_frame) && received_frame.seq_no == expected_seq_num) {
        deliver_to_network_layer(received_frame.packet);
        send_ack(expected_seq_num); // Send ACK for received in-order frame
        expected_seq_num++;
    } else {
        // Discard out-of-order frame and re-send ACK for last correctly received frame
        send_ack(expected_seq_num - 1);
    }
}
```

**4. Test Walkthrough Example (N = 4):**
1. Sender transmits frames: F0, F1, F2, F3 (base = 0, next_seq_num = 4). Window full.
2. Receiver gets F0, delivers to network layer, sends ACK 0.
3. Sender receives ACK 0 ==> base = 1. Window slides to [1, 2, 3, 4]. Sender transmits F4.
4. Frame F1 is lost in transit!
5. Receiver gets F2 (out-of-order) ==> Discards F2, re-sends ACK 0.
6. Sender timer for F1 expires ==> Sender retransmits F1, F2, F3, F4.
7. Receiver accepts F1, sends ACK 1, accepts F2, sends ACK 2, etc.

**Examiner Marking Rubric:**
* Clean sender pseudocode with state variables (base, next_seq_num, N): 2.5 Marks.
* Clean receiver pseudocode with in-order check & cumulative ACK: 1.5 Marks.
* Clear step-by-step test example showing frame transmission, loss, and retransmission: 2.0 Marks.

---

### TOPIC 27: Tunneling in Internetworking & IPv4/IPv6 Transition
**Exam Question Statement:**
> What is tunneling? Why is it important in internetworking? Explain with reference to IPv6 transition and VPNs. [May 2019 CS3025 Q3d, 3M]

**1. Concept & Principle of Tunneling:**
* **Tunneling** is a technique where a packet belonging to one network protocol (the passenger or payload protocol) is encapsulated inside the payload field of another network protocol's packet (the transport or delivery protocol).
* It allows two incompatible or isolated networks to communicate across an intermediate transit network that does not support the passenger protocol.

**2. Architectural Diagram (IPv6 over IPv4 Tunnel):**
```
  [IPv6 Host A]                                               [IPv6 Host B]
       |                                                            ^
       v                                                            |
  +---------+                                                  +---------+
  | Router1 | ====[ Tunnel through IPv4 Intermediate Network ]===>| Router2 |
  +---------+                                                  +---------+
  Encapsulates                                                 Decapsulates
  IPv6 in IPv4                                                 IPv4 packet

  PACKET IN TRANSIT ACROSS IPv4 INTERNET:
  +------------------+------------------+-----------------------------+
  | IPv4 Header      | IPv6 Header      | Transport & Application     |
  | (Proto = 41)     | (Orig Src & Dst) | Payload (TCP / Data)        |
  +------------------+------------------+-----------------------------+
  |<-- Added by R1 ->|<---------------- Original Packet ------------->|
```

**3. Why Tunneling is Essential in Internetworking:**
1. **IPv4-to-IPv6 Transition (RFC 4213)**: The global Internet could not upgrade to IPv6 overnight. Tunneling enables isolated IPv6 islands to communicate across legacy IPv4 ISP backbones by wrapping IPv6 datagrams inside standard IPv4 headers (IPv4 Protocol field = 41).
2. **Virtual Private Networks (VPNs - IPsec / GRE)**: Enables secure private intranet communication over the public untrusted Internet by encrypting the original IP packet and encapsulating it in a new public IP header.
3. **Multiprotocol Encapsulation**: Allows non-routable protocols or incompatible network layers (e.g. AppleTalk, IPX, or MPLS) to traverse standard IP backbones.

**Examiner Marking Rubric:**
* Precise definition of tunneling (packet encapsulation across incompatible transit): 1.0 Mark.
* Architectural packet layout diagram: 1.0 Mark.
* Clear explanation of IPv6 transition and VPN applications: 1.0 Mark.

---

### TOPIC 28: Border Gateway Protocol (BGP) & Path-Vector Inter-Domain Routing
**Exam Question Statement:**
> How does BGP protocol route traffic across the Internet? Why is Path Vector routing preferred over Distance Vector or Link State for Inter-AS routing? [May 2019 CS3025 Q5d, 3M]

**1. Inter-Domain Routing & Autonomous Systems (AS):**
* The global Internet is partitioned into thousands of independently administered domains called **Autonomous Systems (AS)** (e.g., AS15169 for Google, AS7018 for AT&T).
* Interior Gateway Protocols (IGPs like RIP or OSPF) route packets inside an AS based on shortest physical metrics (hop count or delay).
* **BGP (Border Gateway Protocol - RFC 4271)** is the standard **Exterior Gateway Protocol (EGP)** that connects different Autonomous Systems together.

**2. Path-Vector Routing Mechanism:**
* Instead of advertising scalar distance costs (like RIP: '10 hops'), a BGP speaker advertises the **exact sequence of Autonomous Systems (AS-Path)** traversed to reach a target prefix.
* *Example Advertisement for Network 192.0.2.0/24*:
  `Route: 192.0.2.0/24 | AS-Path: [AS 200, AS 300, AS 400]`

**3. Why Path Vector is Essential for Inter-AS Routing:**
1. **Guaranteed Loop Prevention**: When an AS receives a BGP update, it inspects the AS-Path list. If its own AS number is already present in the list, it **immediately discards the route**. This eliminates the Count-to-Infinity problem instantly!
2. **Policy-Based Routing (Commercial Agreements)**: Inter-domain routing is driven by business contracts and peering policies, not shortest physical paths. BGP allows ASes to enforce rules such as: *'Never route traffic from Competitor X across our network'* or *'Prefer free peering link over paid transit link'*.
3. **Scalability**: Avoids flooding full link-state databases across millions of global routers.

**Examiner Marking Rubric:**
* Definition of BGP as inter-AS exterior protocol: 1.0 Mark.
* Explanation of Path-Vector mechanism (AS-Path attribute): 1.0 Mark.
* Loop prevention and policy-based routing justification: 1.0 Mark.

---

### TOPIC 29: Mobile IP Architecture: Home Agent, Foreign Agent & Triangle Routing
**Exam Question Statement:**
> Illustrate the roles of foreign agent and home agent in the routing for mobile host. Explain the complete packet forwarding sequence and triangle routing problem. [May 2019 CS3025 Q6a, 9M]

**1. Core Entities in Mobile IP (RFC 5944):**
* **Mobile Node (MN)**: The roaming host (e.g., laptop/smartphone) that changes its point of attachment from its home network to a foreign network.
* **Home Address (HoA)**: Permanent IP address assigned to MN on its home network (DNS records always point to this address).
* **Home Agent (HA)**: A router on the home network that maintains current location directory of MN and intercepts packets when MN is roaming.
* **Foreign Agent (FA)**: A router on the visited foreign network that assists the roaming MN and assigns it a temporary address.
* **Care-of Address (CoA)**: A temporary IP address identifying the MN's current physical attachment point in the foreign network.
* **Correspondent Node (CN)**: Any standard Internet host communicating with the MN.

**2. Architectural Flow Diagram:**
```
                          [Correspondent Node (CN)]
                                /           ^
              (1) Normal IP    /             \  (4) Direct Reverse
                 Packet to    /               \     Traffic
                  Home IP    /                 \    (Standard IP)
                            v                                       [Home Agent (HA)]                                        |                                        (2) IP-in-IP Encapsulated                              Tunnel to CoA                                            |                                                    v                                            [Foreign Agent (FA)]                                            |                                              (3) Decapsulate & Deliver                                         v                                                 [Mobile Node (MN)] --------------------+
```

**3. Step-by-Step Operation:**
1. **Agent Discovery**: When MN roams into a foreign subnet, FA broadcasts periodic Agent Advertisements. MN detects it is away from home and acquires a Care-of Address (CoA).
2. **Registration**: MN sends a Registration Request to FA, which forwards it to HA. HA records: `MN Home IP <--> Current CoA`. HA replies with Registration Reply.
3. **Packet Interception & Tunneling (Forward Direction)**:
   - CN sends a standard packet addressed to MN's Home Address (`Dest = HoA`).
   - The packet arrives at MN's home subnet. HA uses Proxy ARP to intercept the packet.
   - HA encapsulates the original packet inside an outer IP packet (`Dest = CoA`) and tunnels it to FA.
4. **Decapsulation & Delivery**: FA strips the outer IP header, extracts original packet, and delivers it to MN over the local wireless link.
5. **Reverse Traffic**: When MN sends packets back to CN, it transmits them directly with `Src = HoA, Dest = CN` via standard IP routing.

**4. The Triangle Routing Problem & Solution:**
* *The Problem*: Packets from CN must travel all the way to HA first, and then double-back to FA, creating a highly inefficient triangular path (high latency, unnecessary WAN bandwidth consumption).
* *Solution (Route Optimization)*: HA sends a Binding Update to CN containing MN's CoA. CN caches this binding and tunnels subsequent packets directly to CoA, bypassing HA entirely!

**Examiner Marking Rubric:**
* Clear definition of all 5 entities (MN, HoA, HA, FA, CoA): 2.5 Marks.
* Complete step-by-step communication flow with diagram: 3.5 Marks.
* Triangle routing explanation and route optimization solution: 3.0 Marks.

---

### TOPIC 30: Crash Recovery at the Transport Layer in the Internet
**Exam Question Statement:**
> How crash recovery is done at the transport layer in the Internet? Analyze the client-server crash scenarios and explain why application-layer assistance is required. [May 2019 CS3025 Q6c, 4M]

**1. The Fundamental Problem of Host Crashes:**
* When a host (client or server) crashes during an active TCP connection, its internal volatile memory (RAM) is erased, including all TCP Transmission Control Blocks (TCB), socket tables, sequence numbers, and unwritten socket buffers.
* When the crashed machine reboots, it receives packets from the peer for an unknown connection and sends a `RST` (Reset) segment, abruptly terminating the connection.

**2. Analysis of Server Crash Scenarios:**
Consider a client sending a request that modifies a database (e.g., banking transaction) to a server. The server must perform two steps:
1. **A**: Send transport ACK back to client.
2. **W**: Write transaction to permanent disk / database.

Depending on implementation order, two strategies exist:

* **Strategy 1: Send ACK first, then Write (Strategy AW)**
  - Server receives segment, sends ACK, and then crashes *before* executing the write.
  - Client receives ACK, assumes the transaction succeeded!
  - *Result*: **Unrecoverable data loss** (client thinks write happened, but server has no record).

* **Strategy 2: Write first, then send ACK (Strategy WA)**
  - Server writes transaction to disk, and crashes *before* sending ACK.
  - Client's timer expires without receiving ACK. Client assumes write failed and retransmits!
  - Server reboots, receives retransmission, and executes write a second time!
  - *Result*: **Duplicate execution** (user account debited twice!).

**3. Why Transport Layer Cannot Solve Crash Recovery Alone:**
* Under the **End-to-End Argument** and the theoretical limits of distributed consensus (Two Generals Problem), no zero-loss, zero-duplicate recovery is possible purely within the transport layer.
* **The Internet Solution**:
  1. Transport layer only provides crash detection (connection reset via `RST` or keepalive timeout).
  2. Recovery is delegated to the **Application Layer**:
     - **Idempotent Operations**: Request designs where duplicate executions have identical side effects (`PUT` instead of `POST`).
     - **Transaction IDs & Write-Ahead Logs (WAL)**: Client assigns unique 64-bit UUID to each operation; server checks log before applying updates.

**Examiner Marking Rubric:**
* Explanation of TCB loss and RST generation upon reboot: 1.0 Mark.
* Detailed breakdown of AW vs WA crash failure modes: 1.5 Marks.
* Justification of End-to-End argument and application-layer idempotent solutions: 1.5 Marks.

---

### TOPIC 31: Transport Protocol Selection & Socket Inter-Process Communication Pseudocode
**Exam Question Statement:**
> (a) Choose the correct transport layer protocol to transfer a character array from host A to host B and write pseudo code for host A to host B interprocess file transfer. [Nov 2024 IT2266 Q5a, 4M]  
> (b) Choose the correct transport layer protocol to transfer an Audio file / real-time audio stream from host A to host B and write pseudo code for interprocess transfer. [Nov 2024 IT2266 Q5b, 4M]

**1. Protocol Selection Justification:**
* **Character Array / File Transfer**:
  - **Choice: TCP (Transmission Control Protocol)**
  - *Justification*: Text and character arrays represent programmatic data or source code where **even a single bit flip or lost character completely invalidates the payload**. TCP provides 100% reliable, byte-stream, ordered delivery with error checksums, flow control, and automatic retransmission.
* **Audio File / Real-Time Streaming**:
  - **Choice: UDP (User Datagram Protocol) with RTP / RTSP**
  - *Justification*: Real-time audio streaming is strictly delay-sensitive and jitter-intolerant. A delayed retransmitted audio packet arriving 200 ms late is completely useless and causes annoying audio stutter/glitches. Human hearing easily tolerates a 1-2% random packet loss, but cannot tolerate TCP's Head-of-Line blocking stalls.

**2. TCP Socket Pseudocode for Character Array Transfer (Host A Sender -> Host B Receiver):**
```c
// ==================== HOST A: SENDER (CLIENT) ====================
int main() {
    int sockfd = socket(AF_INET, SOCK_STREAM, 0); // TCP Socket
    struct sockaddr_in serv_addr;
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(8080);
    inet_pton(AF_INET, "192.168.1.10", &serv_addr.sin_addr);

    // Establish 3-way handshake connection
    connect(sockfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    char text_data[] = "VIT_PUNE_COMPUTER_NETWORKS_EXAM_2026";
    int bytes_to_send = strlen(text_data);

    // Send character array reliably
    send(sockfd, text_data, bytes_to_send, 0);

    close(sockfd); // Graceful 4-way FIN teardown
    return 0;
}

// ==================== HOST B: RECEIVER (SERVER) ====================
int main() {
    int server_fd = socket(AF_INET, SOCK_STREAM, 0);
    struct sockaddr_in address;
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(8080);

    bind(server_fd, (struct sockaddr*)&address, sizeof(address));
    listen(server_fd, 5); // Listen with queue size 5

    int client_sock = accept(server_fd, NULL, NULL); // Accept incoming connection
    char buffer[1024];
    int bytes_received = recv(client_sock, buffer, sizeof(buffer) - 1, 0);
    buffer[bytes_received] = '\0';

    printf("Received Character Array: %s\n", buffer);

    close(client_sock);
    close(server_fd);
    return 0;
}
```

**3. UDP Socket Pseudocode for Audio Stream Transfer (Host A Sender -> Host B Receiver):**
```c
// ==================== HOST A: AUDIO SENDER ====================
int main() {
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0); // UDP Datagram Socket
    struct sockaddr_in dest_addr;
    dest_addr.sin_family = AF_INET;
    dest_addr.sin_port = htons(5004); // Standard RTP audio port
    inet_pton(AF_INET, "192.168.1.20", &dest_addr.sin_addr);

    FILE *audio_fp = fopen("audio_sample.raw", "rb");
    char audio_chunk[512]; // Small 20ms audio frames

    while (!feof(audio_fp)) {
        int bytes = fread(audio_chunk, 1, sizeof(audio_chunk), audio_fp);
        // Connectionless transmission: no handshake, no retransmission overhead
        sendto(sockfd, audio_chunk, bytes, 0, 
               (struct sockaddr*)&dest_addr, sizeof(dest_addr));
        usleep(20000); // 20 ms pacing for smooth real-time stream
    }
    fclose(audio_fp);
    close(sockfd);
    return 0;
}

// ==================== HOST B: AUDIO RECEIVER ====================
int main() {
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    struct sockaddr_in recv_addr;
    recv_addr.sin_family = AF_INET;
    recv_addr.sin_addr.s_addr = INADDR_ANY;
    recv_addr.sin_port = htons(5004);

    bind(sockfd, (struct sockaddr*)&recv_addr, sizeof(recv_addr));

    char audio_buffer[512];
    while (1) {
        int bytes = recvfrom(sockfd, audio_buffer, sizeof(audio_buffer), 0, NULL, NULL);
        if (bytes <= 0) break;
        play_audio_to_speakers(audio_buffer, bytes); // Direct playback, zero wait
    }
    close(sockfd);
    return 0;
}
```

**Examiner Marking Rubric:**
* Accurate justification of TCP vs UDP selection: 2.0 Marks.
* TCP client-server pseudocode (socket, bind, listen, accept, connect, send, recv): 3.0 Marks.
* UDP audio pseudocode (socket, sendto, recvfrom, real-time pacing): 3.0 Marks.
