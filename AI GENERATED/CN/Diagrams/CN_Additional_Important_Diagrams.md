# Computer Networks (CS3213 / IT2266 / CS3052)
# Master Visual Blueprint: 22 Additional Essential Exam Diagrams
## Unit-by-Unit Hand-Drawing Guide, Protocol Formats, State Machines & Rubrics

> **Scope**: Covers all 22 additional high-frequency exam diagrams across Units 1 to 6 that complement the initial top 5 diagrams per unit.  
> **Purpose**: Complete visual arsenal for drawing neat, high-scoring diagrams in your 2-hour written exam.  
> **For Every Diagram**:  
> - **Exact Exam Context & Syllabus Topic**  
> - **Clean Universal ASCII Hand-Draw Template** (reproducible in < 60 seconds with pen and ruler)  
> - **Essential Labels & Key Values Required for Full Marks**  
> - **Examiner Marking Rubric** (exact point distribution evaluators check)  
> **Formatting**: 100% clean universal ASCII text with zero unreadable symbols.

---

# UNIT 1: FUNDAMENTALS & PHYSICAL LAYER

---

### DIAGRAM 1: DTE-DCE Interface Model & Transmission Modes
**Exam Context**: Unit 1 - Physical Layer Interface Standards, DTE vs DCE, Transmission Modes.

**ASCII Hand-Draw Template:**
```
  [Data Terminal Equipment]                       [Data Circuit-terminating Equipment]
  +-----------------------+                       +----------------------------------+
  |    COMPUTER / HOST    | <==== RS-232 / =====> |          MODEM / CSU/DSU         | === (Analog Line /
  | (Generates/Consumes)  |       EIA-232         |   (Signal Conversion / Timing)   |      WAN Cable)
  +-----------------------+                       +----------------------------------+
           (DTE)                                                 (DCE)

  TRANSMISSION MODES (Directionality):
  1. Simplex       : [Sender] =======================================> [Receiver]   (e.g., TV/Keyboard)
  2. Half-Duplex   : [Node A] <==== (Alternating: One direction) ====> [Node B]     (e.g., Walkie-Talkie)
  3. Full-Duplex   : [Node A] <==== (Simultaneous Bi-directional) ===> [Node B]     (e.g., Telephone/Switch)
```
**Drawing Notes & Memory Tips:**
* DTE = Producer/Consumer of digital data (PC, terminal). DCE = Physical layer converter (modem, network interface card).
* Clearly label direction arrows for Simplex (unidirectional), Half-Duplex (alternating), and Full-Duplex (simultaneous).
**Examiner Marking Rubric:**
* DTE vs DCE blocks with interconnecting standard (RS-232): 2.5 Marks.
* Simplex, Half-Duplex, Full-Duplex directional flow arrows: 2.5 Marks.

---

### DIAGRAM 2: Optical Fiber Cable Internal Structure & Light Propagation
**Exam Context**: Unit 1 - Transmission Media, Optical Fiber Construction, Single-Mode vs Multi-Mode.

**ASCII Hand-Draw Template:**
```
  CROSS-SECTIONAL STRUCTURE:
  +-------------------------------------------------------------+ --- Outer Jacket (PVC/Teflon)
  |  +-------------------------------------------------------+  | --- Strength Fibers (Kevlar)
  |  |  +-------------------------------------------------+  |  | --- Buffer Coating (Silicone)
  |  |  |  +-------------------------------------------+  |  |  | --- Cladding (n2, lower index)
  |  |  |  |  =======================================  |  |  |  | --- Core (n1, high index glass)
  |  |  |  |  +----------------------------------------+  |  |  |
  |  |  |  +----------------------------------------------+  |  |
  |  |  +----------------------------------------------------+  |
  |  +----------------------------------------------------------+  |
  +-------------------------------------------------------------+

  LIGHT PROPAGATION (Total Internal Reflection: n1 > n2):
             Cladding (Refractive Index n2)
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                / \             / \             /       ===> ----/   \-----------/   \-----------/   \===> Light Ray (Angle > Critical Angle)
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
             Cladding (Refractive Index n2)

  SINGLE-MODE (9/125 um)        vs        MULTI-MODE (50/125 or 62.5/125 um)
  [=== Core 9 um (Laser) ===]             [======= Core 50 um (LED) =======]
  ---> Single straight axial ray          ---> Multiple bouncing zigzag paths
```
**Drawing Notes & Memory Tips:**
* Condition for light guiding: Refractive index of Core ($n_1$) MUST be strictly greater than Cladding ($n_2$), and angle of incidence must exceed the Critical Angle ($	heta > 	heta_c$).
* Single-mode: 9 um core (one path, long distance). Multi-mode: 50/62.5 um core (multiple paths, modal dispersion).
**Examiner Marking Rubric:**
* 5 concentric structural layers labelled: 2.0 Marks.
* Total internal reflection ray path and $n_1 > n_2$ condition: 1.5 Marks.
* Single-mode vs Multi-mode core diameter and ray contrast: 1.5 Marks.

---

### DIAGRAM 3: VLAN Architecture & IEEE 802.1Q Tagged Trunk Link
**Exam Context**: Unit 1 - Switched Networks, Virtual LANs, Trunking, IEEE 802.1Q.

**ASCII Hand-Draw Template:**
```
  [SWITCH A (Floor 1)]                                            [SWITCH B (Floor 2)]
  +---------------------------------+                             +---------------------------------+
  | Ports 1-12: VLAN 10 (Faculty)   |                             | Ports 1-12: VLAN 10 (Faculty)   |
  | Ports 13-24: VLAN 20 (Students) |                             | Ports 13-24: VLAN 20 (Students) |
  +----------------+----------------+                             +----------------+----------------+
                   |                                                               |
  [PC1 (VLAN 10)] -+                                              [PC3 (VLAN 10)] -+
  [PC2 (VLAN 20)] -+             TRUNK LINK (IEEE 802.1Q)         [PC4 (VLAN 20)] -+
                   |                                                               |
                   +==================== [TRUNK PORT] =============================+
                        Carries BOTH VLAN 10 & 20 Frames Tagged

  IEEE 802.1Q 4-BYTE TAG INSERTION:
  +---------------+---------------+--------------------+---------------+---------------+
  | Dest MAC (6B) | Src MAC (6B)  | * 802.1Q TAG (4B) *| EtherType(2B)| Data Payload  |
  +---------------+---------------+--------------------+---------------+---------------+
                                            |
             +------------------------------+------------------------------+
             | TPID (2 Bytes = 0x8100)      | Priority (3b)| DEI (1b) | VLAN ID (12 Bits)  |
             +------------------------------+----------------------------------------------+
                                                                        (1 to 4094 VLANs)
```
**Drawing Notes & Memory Tips:**
* Access ports connect to PCs and carry **untagged** frames. Trunk ports connect switches and carry **tagged** frames.
* The 802.1Q tag is inserted directly after the Source MAC address. VLAN ID is 12 bits ($2^{12} = 4096$ VLANs).
**Examiner Marking Rubric:**
* Dual-switch VLAN topology with access and trunk ports: 2.0 Marks.
* Ethernet frame layout showing 802.1Q tag position: 1.5 Marks.
* 4-byte tag breakdown (TPID `0x8100`, Priority, VLAN ID 12b): 1.5 Marks.

---

### DIAGRAM 4: Spread Spectrum Modulation: FHSS vs DSSS
**Exam Context**: Unit 1/3 - Physical Layer Wireless Modulation, Spread Spectrum.

**ASCII Hand-Draw Template:**
```
  FHSS (Frequency Hopping Spread Spectrum):
  Frequency ^
     f4     |                     [Hop 3]
     f3     |                                     [Hop 5]
     f2     |         [Hop 2]
     f1     | [Hop 1]                 [Hop 4]
            +----------------------------------------------> Time
            Pseudo-random carrier frequency switching across 79 channels (Bluetooth)

  DSSS (Direct Sequence Spread Spectrum):
  Data Bit (Tb)     : [                 1                 ]
  PN Chips (Tc)     : [ 1 | 0 | 1 | 1 | 0 | 1 | 1 | 1 | 0 | 0 | 0 ] (11-bit Barker Code)
  Transmitted XOR   : Wideband spread spectrum signal (Wi-Fi 802.11b)
```
**Drawing Notes & Memory Tips:**
* FHSS hops frequencies over time (Bluetooth, 1600 hops/s). DSSS expands each bit into $N$ chips using XOR (Wi-Fi).
**Examiner Marking Rubric:**
* Frequency-time hopping staircase for FHSS: 2.5 Marks.
* Data bit vs Barker chip sequence multiplication for DSSS: 2.5 Marks.

---

# UNIT 2: LOGICAL LINK CONTROL & FRAMING

---

### DIAGRAM 5: Point-to-Point Protocol (PPP) Lifecycle State Machine
**Exam Context**: Unit 2 - WAN Protocols, PPP Connection Establishment, LCP/NCP.

**ASCII Hand-Draw Template:**
```
                                 +--------------+
                                 |     DEAD     | <------------------------------------+
                                 +-------+------+                                      |
                                         | Carrier Detected                            |
                                         v                                             |
                                 +--------------+                                      |
                     +---------> |  ESTABLISH   | (LCP Config-Request / Ack)           |
                     |           +-------+------+                                      |
       LCP Failed or |                   | LCP Opened                                  |
         Closed Down |                   v                                             |
                     |           +--------------+                                      |
                     |           | AUTHENTICATE | (PAP / CHAP Credentials Exchange)    |
                     |           +-------+------+                                      |
                     |                   | Auth Succeeded (or none)                    |
                     |                   v                                             |
                     |           +--------------+                                      |
                     |           |   NETWORK    | (NCP / IPCP Assigns Dynamic IP)      |
                     |           +-------+------+                                      |
                     |                   | NCP Opened                                  |
                     |                   v                                             |
                     |           +--------------+                                      |
                     +---------- |     OPEN     | (Data Transfer: TCP/IP Payloads)     |
                                 +-------+------+                                      |
                                         | Close Request / Carrier Lost                |
                                         v                                             |
                                 +--------------+                                      |
                                 |  TERMINATE   | (LCP Terminate-Request / Ack) -------+
                                 +--------------+
```
**Drawing Notes & Memory Tips:**
* 6 distinct states: **Dead -> Establish -> Authenticate -> Network -> Open -> Terminate -> Dead**.
* LCP operates in Establish/Terminate; PAP/CHAP operates in Authenticate; NCP/IPCP operates in Network.
**Examiner Marking Rubric:**
* All 6 states drawn in circular transition: 2.5 Marks.
* Labelling transition triggers (Carrier, LCP Open, Auth Succeeded, IPCP Open): 2.5 Marks.

---

### DIAGRAM 6: Sliding Window Buffer States: Go-Back-N vs Selective Repeat
**Exam Context**: Unit 2 - Flow Control, Sliding Window Pipelining, Buffer Management.

**ASCII Hand-Draw Template:**
```
  GO-BACK-N ARQ SENDER & RECEIVER BUFFERS:
  Sender Buffer:
  [ 0 | 1 | 2 ] [ 3 | 4 | 5 | 6 ] [ 7 | 8 | 9 ... ]
  |<-- ACKed ->|<- Window Ws=4 ->|<- Cannot Send ->|
               ^                 ^
              base           next_seq_num
  Receiver Buffer:
  [ Single Slot: Expected = 3 ]  ===> Any out-of-order frame (4, 5, 6) is DISCARDED!

  SELECTIVE REPEAT ARQ SENDER & RECEIVER BUFFERS:
  Sender Buffer:
  [ 0 | 1 | 2 ] [ 3 | 4 | 5 | 6 ] [ 7 | 8 | 9 ... ]
               |<- Window Ws=4 ->|
  Receiver Buffer (Window Wr = Ws = 4):
  [ 0 | 1 | 2 ] [ 3 (Lost) | 4 (Buffered) | 5 (Buffered) | 6 (Buffered) ]
  ===> Accepts and buffers out-of-order frames 4, 5, 6; retransmits ONLY frame 3!
```
**Drawing Notes & Memory Tips:**
* GBN: Receiver window $W_R = 1$ (no buffer). SR: Receiver window $W_R = W_S = 2^{m-1}$ (sorting buffer).
**Examiner Marking Rubric:**
* GBN sender sliding pointers (`base`, `next_seq`) and 1-slot receiver: 2.5 Marks.
* SR receiver buffer accepting out-of-order frames without discarding: 2.5 Marks.

---

### DIAGRAM 7: Character-Oriented Byte Stuffing (BISYNC / DLE Doubling)
**Exam Context**: Unit 2 - Data Link Framing, Byte Stuffing, Character-Oriented Protocols.

**ASCII Hand-Draw Template:**
```
  ORIGINAL DATA FROM NETWORK LAYER:
  +---------------------------------------------------------------+
  | A  |  B  |  DLE  |  C  |  D  |  ETX  |  E  |  DLE  |  ETX  | F |
  +---------------------------------------------------------------+

  DATA LINK TRANSMITTED FRAME (After DLE Doubling + Framing Flags):
  +---------+---------+----+----+---------+----+----+-----+----+---------+-----+----+---------+---------+
  | DLE STX |    A    | B  |DLE | DLE     | C  | D  | ETX | E  | DLE     | DLE |ETX | DLE ETX |   CRC   |
  +---------+---------+----+----+---------+----+----+-----+----+---------+-----+----+---------+---------+
  ^ Frame               ^ Accidental DLE        ^ Real ETX     ^ Accidental DLE  ^ Real Frame  ^ Error
  Start Delimiter       Stuffed with 2nd DLE    not escaped    Stuffed with DLE  End Delimiter Check
```
**Drawing Notes & Memory Tips:**
* Frame boundaries: `DLE STX` (Start of TeXt) and `DLE ETX` (End of TeXt).
* Any `DLE` appearing in data is doubled (`DLE DLE`). Receiver converts `DLE DLE` back to a single `DLE`.
**Examiner Marking Rubric:**
* Identification of Start (`DLE STX`) and End (`DLE ETX`) delimiters: 2.0 Marks.
* Accurate stuffing rule showing doubled `DLE` bytes in payload: 3.0 Marks.

---

# UNIT 3: MEDIUM ACCESS CONTROL & ETHERNET

---

### DIAGRAM 8: Collision-Free MAC Protocols: Bit-Map vs Binary Countdown
**Exam Context**: Unit 3 - Multiple Access Protocols, Collision-Free Channel Allocation.

**ASCII Hand-Draw Template:**
```
  1. BIT-MAP RESERVATION PROTOCOL:
  Contention Period (8 Slots)              Data Frame Transmission Period
  +---+---+---+---+---+---+---+---+        +----------------+----------------+----------------+
  | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 1 | -----> | Frame from     | Frame from     | Frame from     |
  +---+---+---+---+---+---+---+---+        | Station 1      | Station 3      | Station 7      |
    0   1   2   3   4   5   6   7          +----------------+----------------+----------------+
  (Stations 1, 3, 7 asserted 1)            (Transmitted in order, zero collisions!)

  2. BINARY COUNTDOWN PROTOCOL:
  Competing Stations: 0010 (2), 0100 (4), 1010 (10), 1001 (9)
  Bit 3 (MSB): 0010 and 0100 sense 1 on channel and DROP OUT! Remaining: 1010 & 1001.
  Bit 2      : Both have 0. Both remain.
  Bit 1      : 1010 broadcasts 1; 1001 broadcasts 0 -> 1001 DROPS OUT!
  Winner     : Station 1010 (10) wins and transmits complete frame with ZERO collision.
```
**Drawing Notes & Memory Tips:**
* Bit-map: $N$ contention bits for $N$ stations. Binary countdown: $O(\log_2 N)$ arbitration bits using Wired-OR.
**Examiner Marking Rubric:**
* Bit-map contention slot reservation sequence: 2.5 Marks.
* Binary countdown bit-by-bit elimination trace: 2.5 Marks.

---

### DIAGRAM 9: CSMA Channel Sensing Persistence Decision Trees
**Exam Context**: Unit 3 - Random Access MAC, 1-Persistent, Non-Persistent, p-Persistent CSMA.

**ASCII Hand-Draw Template:**
```
         1-PERSISTENT CSMA                       NON-PERSISTENT CSMA                     p-PERSISTENT CSMA
        +------------------+                    +------------------+                    +------------------+
        | Frame to Send    |                    | Frame to Send    |                    | Frame to Send    |
        +--------+---------+                    +--------+---------+                    +--------+---------+
                 |                                       |                                       |
                 v                                       v                                       v
         +-------+--------+                      +-------+--------+                      +-------+--------+
    +--> | Sense Channel  |                 +--> | Sense Channel  |                 +--> | Sense Channel  |
    |    +-------+--------+                 |    +-------+--------+                 |    +-------+--------+
    |            |                                       |                                       |
    | Idle?      | Busy?                         Idle?   | Busy?                         Idle?   | Busy?
    |   +--------+--------+                         +----+----+                             +----+----+
    |   |                 |                         |         |                             |         |
    |   v (Idle)          v (Busy)                  v (Idle)  v (Busy)                      v (Idle)  v (Busy)
    | Transmit     Keep Listening            Transmit   Wait Random Backoff         Probability p?  Wait till Idle
    | (Prob = 1)   Wait till Idle                       Then Sense Again                 +---+---+
    +---------------------+                                                              |       |
                                                                                    Yes(p)      No(1-p)
                                                                                         |       |
                                                                                         v       v
                                                                                     Transmit  Wait Next Slot
```
**Drawing Notes & Memory Tips:**
* 1-persistent: transmits immediately when idle; non-persistent: backs off when busy; p-persistent: transmits with probability $p$.
**Examiner Marking Rubric:**
* Complete decision loops for all three persistence modes: 5.0 Marks.

---

### DIAGRAM 10: IEEE 802.11 Wi-Fi Inter-Frame Spaces (IFS) Timing Architecture
**Exam Context**: Unit 3 - Wireless LANs, CSMA/CA, MAC Timing.

**ASCII Hand-Draw Template:**
```
  Channel ^
  Busy    |               DIFS
  =======>|======| <------------------> |
          |      |                      |
          | SIFS |    PIFS              |   Contention Window (Random Backoff Slots)
          | <--> | <---------> |        | <------------------------------------------------> |
          +------+-------------+--------+----+----+----+----+----+----+----+----+----+----+--+
                                        | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 10 | ===> Transmit Frame!
                                        +----+----+----+----+----+----+----+----+----+----+--+
  PRIORITY RELATIONSHIP:
  SIFS (Shortest IFS: ACKs, CTS, Polling) < PIFS (PCF polling) < DIFS (DCF normal data) < EIFS (Extended error)
```
**Drawing Notes & Memory Tips:**
* SIFS is shortest $\implies$ highest priority (guarantees ACK/CTS cannot be preempted). DIFS is used for normal data.
**Examiner Marking Rubric:**
* Chronological IFS timeline diagram (SIFS, PIFS, DIFS, Backoff slots): 3.0 Marks.
* Explanation of priority hierarchy (SIFS < PIFS < DIFS < EIFS): 2.0 Marks.

---

# UNIT 4: NETWORK LAYER & ROUTING

---

### DIAGRAM 11: IPv4 Datagram MTU Fragmentation & Reassembly Lifecycle
**Exam Context**: Unit 4 - Network Layer Protocols, MTU Fragmentation, Fragment Offset.

**ASCII Hand-Draw Template:**
```
  [Original IP Packet: Total Length = 1500 B (20 B Header + 1480 B Data), ID = 777, DF = 0, MF = 0, Offset = 0]
                                          |
                                          v  [Router R1: Outgoing Link MTU = 1000 Bytes]
                   +----------------------+----------------------+
                   | (Max Data = 1000 - 20 = 980 -> Multiple of 8 = 976 B)
                   v                                             v
  [Fragment 1: Total = 996 B]                   [Fragment 2: Total = 524 B]
  - Header = 20 B, Data = 976 B                 - Header = 20 B, Data = 504 B
  - ID = 777, MF = 1, Offset = 0                - ID = 777, MF = 0, Offset = 976/8 = 122
                   |                                             |
                   v  [Router R2: Outgoing Link MTU = 400 Bytes] v
  (Max Data = 400 - 20 = 380 -> Mult of 8 = 376 B)
         +---------+---------+                                   +---------+---------+
         |         |         |                                   |         |
         v         v         v                                   v         v
       [F1a]     [F1b]     [F1c]                               [F2a]     [F2b]
       Data=376  Data=376  Data=224                            Data=376  Data=128
       MF=1      MF=1      MF=1                                MF=1      MF=0
       Offset=0  Off=47    Off=94                              Off=122   Off=169
```
**Drawing Notes & Memory Tips:**
* Data in every fragment (except the last) MUST be a multiple of 8 bytes. Fragment Offset = $	ext{Byte Offset} / 8$.
* Identification number remains identical (777). MF = 1 for all fragments except the final one (MF = 0).
**Examiner Marking Rubric:**
* 2-stage fragmentation tree diagram: 2.0 Marks.
* Correct Data, Length, MF, and Offset values for all 5 sub-fragments: 3.0 Marks.

---

### DIAGRAM 12: Address Resolution Protocol (ARP) Frame Format & Resolution Exchange
**Exam Context**: Unit 4 - Host-to-Host Resolution, ARP Broadcast/Unicast.

**ASCII Hand-Draw Template:**
```
  ARP RESOLUTION EXCHANGE OVER LAN:
  [Host A: 192.168.1.10]                                          [Host B: 192.168.1.20]
  [MAC: AA:AA:AA:AA:AA:AA]                                        [MAC: BB:BB:BB:BB:BB:BB]
            |                                                               |
            | ===== (1) ARP REQUEST (Broadcast: FF:FF:FF:FF:FF:FF) ======> | (All stations receive,
            |       "Who has 192.168.1.20? Tell 192.168.1.10"              |  only Host B processes)
            |                                                               |
            | <==== (2) ARP REPLY (Unicast to AA:AA:AA:AA:AA:AA) ========= |
            |       "192.168.1.20 is at BB:BB:BB:BB:BB:BB"                 |
            v                                                               v
  Caches: 192.168.1.20 -> BB:BB:BB:BB:BB:BB                     Caches: 192.168.1.10 -> AA:AA:AA:AA:AA:AA

  28-BYTE ARP PACKET FORMAT (Encapsulated in Ethernet Frame, EtherType = 0x0806):
  +-------------------------------+-------------------------------+
  | Hardware Type (16b: 1 = Eth)  | Protocol Type (16b: 0x0800 IP)|
  +---------------+---------------+---------------+---------------+
  | HLEN (8b: 6)  | PLEN (8b: 4)  | Operation (16b: 1=Req, 2=Rep) |
  +---------------+---------------+-------------------------------+
  |                  Sender Hardware MAC (Bytes 0-3)              |
  +-------------------------------+-------------------------------+
  | Sender MAC (Bytes 4-5)        | Sender IP Address (Bytes 0-1) |
  +-------------------------------+-------------------------------+
  | Sender IP Address (Bytes 2-3) | Target Hardware MAC (Bytes 0-1)|
  +-------------------------------+-------------------------------+
  |                  Target Hardware MAC (Bytes 2-5)              |
  +---------------------------------------------------------------+
  |                  Target IP Address (4 Bytes)                  |
  +---------------------------------------------------------------+
```
**Drawing Notes & Memory Tips:**
* Request is **Broadcast** (`FF:FF:FF:FF:FF:FF`); Reply is **Unicast**. Target MAC is all zeros in request.
**Examiner Marking Rubric:**
* Broadcast Request & Unicast Reply exchange: 2.0 Marks.
* 28-byte packet format layout with HLEN/PLEN/Opcode: 3.0 Marks.

---

### DIAGRAM 13: OSPF Hierarchical Area Architecture
**Exam Context**: Unit 4 - Link State Routing, OSPF Areas, ABR, ASBR.

**ASCII Hand-Draw Template:**
```
                     +---------------------------------------+
                     |         EXTERNAL AUTONOMOUS SYSTEM    | (BGP / Internet)
                     +-------------------+-------------------+
                                         |
                                      [ ASBR ] (Autonomous System Boundary Router)
                                         |
  +--------------------------------------+--------------------------------------+
  |                                                                             |
  |                           BACKBONE AREA 0                                   |
  |             [Backbone Router] -------- [Backbone Router]                    |
  |                    |                           |                            |
  +--------------------+---------------------------+----------------------------+
                       |                           |
                    [ ABR ] (Area Border Router) [ ABR ] (Area Border Router)
                       |                           |
         +-------------+-------------+             +-------------+-------------+
         |                           |             |                           |
         v                           v             v                           v
  +-----------------------+                 +-----------------------+
  |        AREA 1         |                 |        AREA 2         |
  |  [Internal Routers]   |                 |  [Internal Routers]   |
  |  (Single LSDB within) |                 |  (Single LSDB within) |
  +-----------------------+                 +-----------------------+
```
**Drawing Notes & Memory Tips:**
* All areas MUST connect directly to **Backbone Area 0**. ABR connects areas to Area 0; ASBR connects OSPF to outside world.
**Examiner Marking Rubric:**
* Backbone Area 0 with ABR and ASBR clearly positioned: 3.0 Marks.
* Internal area routing vs summary LSA flooding: 2.0 Marks.

---

### DIAGRAM 14: Mobile IP Architecture & Triangle Routing
**Exam Context**: Unit 4 - Mobile IP, Home Agent, Foreign Agent, Care-of Address.

**ASCII Hand-Draw Template:**
```
                                 [Correspondent Node (CN)]
                                      /            ^
                     (1) Normal IP   /              \  (4) Direct Reverse
                        Packet to   /                \     Traffic
                         Home IP   /                  \    (Standard IP)
                                  v                                             [Home Agent (HA)]                                                |                                               (2) IP-in-IP Encapsulated                                     Tunnel to CoA                                                   |                                                           v                                                   [Foreign Agent (FA)]                                                   |                                                     (3) Decapsulate & Deliver                                                v                                                        [Mobile Node (MN)] ---------------------+
```
**Drawing Notes & Memory Tips:**
* Forward path is triangular: CN $	o$ HA $	o$ FA $	o$ MN. Reverse path is direct: MN $	o$ CN.
* Tunnel uses IP-in-IP encapsulation with Care-of Address (CoA) as outer destination.
**Examiner Marking Rubric:**
* All 5 entities (CN, HA, FA, MN, CoA): 2.5 Marks.
* Triangular forward tunnel vs direct reverse route: 2.5 Marks.

---

### DIAGRAM 15: IPv6-over-IPv4 Tunneling Architecture
**Exam Context**: Unit 4 - Transition Mechanisms, Tunneling, Dual-Stack.

**ASCII Hand-Draw Template:**
```
  [IPv6 Island A]                                                 [IPv6 Island B]
  [IPv6 Host 1]                                                   [IPv6 Host 2]
        |                                                               ^
        v                                                               |
  +-----------+                                                   +-----------+
  | Router R1 | ======[ Tunnel across IPv4 Intermediate ISP ]====>| Router R2 |
  +-----------+                                                   +-----------+
  Dual-Stack Router                                               Dual-Stack Router
  Encapsulates IPv6 in IPv4                                       Strips IPv4 Header

  ENCAPSULATED PACKET IN TRANSIT:
  +-------------------+-------------------+-----------------------------------+
  | Outer IPv4 Header | Inner IPv6 Header | Transport & Application Payload   |
  | (Protocol = 41)   | (Orig Src & Dst)  | (TCP / UDP / Data)                |
  +-------------------+-------------------+-----------------------------------+
  |<--- Added by R1 ->|<----------------- Original Datagram ----------------->|
```
**Drawing Notes & Memory Tips:**
* Dual-stack routers encapsulate IPv6 datagrams into IPv4 packets (Protocol field = 41). Decapsulated at tunnel endpoint.
**Examiner Marking Rubric:**
* Dual-stack edge router topology: 2.5 Marks.
* Packet encapsulation format showing Outer IPv4 (Proto 41) + Inner IPv6: 2.5 Marks.

---

# UNIT 5: TRANSPORT LAYER PROTOCOLS & TIMERS

---

### DIAGRAM 16: Complete TCP Finite State Machine (FSM)
**Exam Context**: Unit 5 - TCP Connection Lifecycle, State Transitions.

**ASCII Hand-Draw Template:**
```
                              +---------+
                              | CLOSED  |
                              +----+----+
                     Passive Open  |    | Active Open
                     Create TCB    |    | Send SYN
                                   v    v
                               +----------+
                    +--------> |  LISTEN  |
                    |          +----+-----+
                    |   RCV SYN     |
                    |   Send SYN+ACK|
                    |               v
                    |          +----------+    RCV SYN+ACK
                    |          | SYN_RCVD |    Send ACK
                    |          +----+-----+ -------------+
                    |               |                    |
                    |       RCV ACK |                    v
                    |               v             +--------------+
                    |       +---------------+     |   SYN_SENT   |
                    +------ |  ESTABLISHED  | <---+--------------+
                            +-------+-------+
                       Close        |       Close
                       Send FIN     |       Receive FIN, Send ACK
                                    v       v
                            +---------------+---------------+
                            | FIN_WAIT_1    |  CLOSE_WAIT   |
                            +-------+-------+-------+-------+
                            |       |               | Close, Send FIN
                            v       v               v
                      FIN_WAIT_2  CLOSING       LAST_ACK
                            |       |               |
                    RCV FIN |       v RCV ACK       v RCV ACK
                    Send ACK|  +----------+         |
                            +->|TIME_WAIT |<--------+
                               +----+-----+
                                    | 2 MSL Timeout
                                    v
                               +---------+
                               | CLOSED  |
                               +---------+
```
**Drawing Notes & Memory Tips:**
* Active closer goes through: `ESTABLISHED -> FIN_WAIT_1 -> FIN_WAIT_2 -> TIME_WAIT (2MSL) -> CLOSED`.
* Passive closer goes through: `ESTABLISHED -> CLOSE_WAIT -> LAST_ACK -> CLOSED`.
**Examiner Marking Rubric:**
* Handshake states (`LISTEN`, `SYN_SENT`, `SYN_RCVD`, `ESTABLISHED`): 2.5 Marks.
* Teardown states (`FIN_WAIT_1`, `FIN_WAIT_2`, `CLOSE_WAIT`, `LAST_ACK`, `TIME_WAIT`): 2.5 Marks.

---

### DIAGRAM 17: Berkeley Sockets API Workflow (TCP vs UDP)
**Exam Context**: Unit 5 - Socket Programming, Connection-Oriented vs Connectionless APIs.

**ASCII Hand-Draw Template:**
```
       TCP STREAM SOCKETS (Connection-Oriented)                 UDP DATAGRAM SOCKETS (Connectionless)
       SERVER                     CLIENT                       SERVER                     CLIENT
  +---------------+          +---------------+            +---------------+          +---------------+
  |   socket()    |          |   socket()    |            |   socket()    |          |   socket()    |
  +-------+-------+          +-------+-------+            +-------+-------+          +-------+-------+
          |                          |                            |                          |
          v                          |                            v                          |
  +---------------+                  |                    +---------------+                  |
  |    bind()     |                  |                    |    bind()     |                  |
  +-------+-------+                  |                    +-------+-------+                  |
          |                          |                            |                          |
          v                          |                            |                          v
  +---------------+                  |                            |                  +---------------+
  |   listen()    |                  |                            |                  |   sendto()    |
  +-------+-------+                  v                            v                  +-------+-------+
          |                  +---------------+            +---------------+                  |
          v                  |   connect()   |            |  recvfrom()   | <----------------+
  +---------------+          +-------+-------+            +-------+-------+
  |   accept()    | <----------------+ (3-Way Handshake)          |                          |
  +-------+-------+                  |                            v                          v
          |                          v                    +---------------+          +---------------+
          | <======================> | (send/recv)        |   sendto()    | -------> |  recvfrom()   |
          v                          v                    +---------------+          +---------------+
  +---------------+          +---------------+                    |                          |
  |    close()    |          |    close()    |                    v                          v
  +---------------+          +---------------+            +---------------+          +---------------+
                                                          |    close()    |          |    close()    |
                                                          +---------------+          +---------------+
```
**Drawing Notes & Memory Tips:**
* TCP requires `listen()` and `accept()` on server, and `connect()` on client.
* UDP has **no** `listen()`, `accept()`, or `connect()`; both sides use `sendto()` and `recvfrom()` directly.
**Examiner Marking Rubric:**
* TCP full workflow (`socket`, `bind`, `listen`, `accept`, `connect`, `send`, `recv`, `close`): 2.5 Marks.
* UDP workflow (`socket`, `bind`, `sendto`, `recvfrom`): 2.5 Marks.

---

### DIAGRAM 18: Silly Window Syndrome Mechanics & Dual Solutions
**Exam Context**: Unit 5 - Flow Control, SWS, Clark's & Nagle's Algorithms.

**ASCII Hand-Draw Template:**
```
  THE SILLY WINDOW SYNDROME (Micro-packets eat 97% bandwidth in headers):
  [Sender] ====== 1 Byte Data + 40 B Header (41 Bytes total) ======> [Receiver]
           <===== 40 B ACK (rwnd = 1 Byte announced) ==============  (Application reads 1 byte!)

  CLARK'S SOLUTION (Receiver-Side):
  +----------------------------------------------------------------------------------+
  | DO NOT announce rwnd = 1. Keep advertising rwnd = 0 until:                       |
  | (1) Receiver can accommodate at least 1 Full MSS, OR                             |
  | (2) At least HALF of the total receiver buffer space is empty.                   |
  +----------------------------------------------------------------------------------+

  NAGLE'S ALGORITHM (Sender-Side):
  +----------------------------------------------------------------------------------+
  | (1) Send first byte immediately.                                                 |
  | (2) BUFFER all subsequent outgoing keystrokes / data until:                      |
  |     - Accumulated data >= 1 Full MSS, OR                                         |
  |     - An ACK for the previous packet arrives!                                    |
  +----------------------------------------------------------------------------------+
```
**Drawing Notes & Memory Tips:**
* Clark's operates at the **receiver** (suppresses window updates). Nagle's operates at the **sender** (buffers data).
**Examiner Marking Rubric:**
* Problem diagram showing 1-byte payload + 40-byte header: 1.5 Marks.
* Clark's receiver condition (1 MSS or half buffer): 1.5 Marks.
* Nagle's sender condition (1 MSS or ACK received): 2.0 Marks.

---

### DIAGRAM 19: Transport Layer Crash Recovery Failure Models
**Exam Context**: Unit 5 - Crash Recovery, End-to-End Argument, Tanenbaum's AW vs WA.

**ASCII Hand-Draw Template:**
```
  CLIENT-SERVER CRASH SCENARIOS (Data Write 'W' vs Transport Ack 'A'):

  STRATEGY AW (Acknowledge First, then Write):
  [Client] ----- (1) Send Transaction -------> [Server]
           <---- (2) Transport ACK ------------+ (ACK sent to client)
                                                 [CRASH OCCURS BEFORE DISK WRITE!]
  Result: Client believes transaction succeeded, but data is completely LOST on disk!

  STRATEGY WA (Write First, then Acknowledge):
  [Client] ----- (1) Send Transaction -------> [Server] (Writes transaction to disk)
                                                 [CRASH OCCURS BEFORE ACK IS SENT!]
  Client Timeout Expires -> Client Retransmits -> Server Reboots & Writes AGAIN!
  Result: DUPLICATE EXECUTION (User account debited twice!).
```
**Drawing Notes & Memory Tips:**
* Proves why the transport layer alone cannot solve host crashes. Real recovery requires **Application Layer Idempotence**.
**Examiner Marking Rubric:**
* Strategy AW failure timeline (data loss): 2.5 Marks.
* Strategy WA failure timeline (duplicate execution): 2.5 Marks.

---

# UNIT 6: APPLICATION LAYER SERVICES & PROTOCOLS

---

### DIAGRAM 20: Web Cookies Lifecycle & Security Flags
**Exam Context**: Unit 6 - WWW, HTTP State Management, Web Security.

**ASCII Hand-Draw Template:**
```
  CLIENT (Browser)                                                SERVER (Web Application)
         |                                                                   |
         | ----- (1) POST /login (Username & Password) --------------------> |
         |                                                                   | Authenticates User,
         |                                                                   | Creates Session XYZ
         | <---- (2) HTTP 200 OK ------------------------------------------- |
         |           Set-Cookie: session=XYZ; Secure; HttpOnly; SameSite=Strict
         |                                                                   |
  Stores in Cookie Jar                                                       |
  - HttpOnly : Block JS (XSS defense)                                        |
  - Secure   : Force HTTPS                                                   |
  - SameSite : Block CSRF                                                    |
         |                                                                   |
         | ----- (3) GET /account_summary ---------------------------------> | Reads Cookie header,
         |           Cookie: session=XYZ                                     | validates session,
         |                                                                   | serves private data
         | <---- (4) HTTP 200 OK (Personalized Account Data) --------------- |
```
**Drawing Notes & Memory Tips:**
* `Set-Cookie` in response from server; `Cookie:` in subsequent requests from client.
* `HttpOnly` stops XSS; `Secure` stops eavesdropping; `SameSite=Strict` stops CSRF.
**Examiner Marking Rubric:**
* Complete 4-step cookie lifecycle exchange: 2.5 Marks.
* Correct identification and purpose of `HttpOnly`, `Secure`, `SameSite`: 2.5 Marks.

---

### DIAGRAM 21: Forward Proxy vs Reverse Proxy Architectures
**Exam Context**: Unit 6 - Web Caching, Proxy Servers, Security & Load Balancing.

**ASCII Hand-Draw Template:**
```
  FORWARD PROXY (Client-Side Proxy - e.g. College/Corporate Web Proxy):
  [Client 1] -+
  [Client 2] -+-> [ FORWARD PROXY ] ===== (Internet) =====> [ Any Web Server ]
  [Client 3] -+   - Caches web pages to save bandwidth
                  - Enforces access control (blocks gaming/gambling)
                  - Masks client internal IP addresses

  REVERSE PROXY (Server-Side Proxy - e.g. NGINX, Cloudflare):
                  [ REVERSE PROXY ] ------> [ Backend Server 1 (App) ]
  [Public Web] => - Terminates SSL/TLS ---> [ Backend Server 2 (App) ]
  [Internet  ] => - Load-balances traffic-> [ Backend Server 3 (DB)  ]
                  - Protects origin IPs from DDoS attacks
```
**Drawing Notes & Memory Tips:**
* Forward proxy acts on behalf of **clients** (hides clients). Reverse proxy acts on behalf of **servers** (hides servers).
**Examiner Marking Rubric:**
* Forward proxy client-side diagram & functions: 2.5 Marks.
* Reverse proxy server-side diagram & functions: 2.5 Marks.

---

### DIAGRAM 22: Complete End-to-End Protocol Cascade After Laptop Reboot
**Exam Context**: Unit 6 - Comprehensive Integration, Web Browsing Protocol Sequence.

**ASCII Hand-Draw Template:**
```
  CHRONOLOGICAL PROTOCOL CASCADE TO LOAD A WEBPAGE AFTER FRESH REBOOT:

  1. DHCP (UDP 67/68)     : [Laptop] <====== 4-Way DORA ======> [DHCP Server]
                            Acquires IP (192.168.1.50), Netmask, Default Gateway (192.168.1.1), DNS (8.8.8.8)

  2. ARP (EtherType 0x0806):[Laptop] ===== Broadcast Request ====> [LAN]
                            Finds MAC address of Default Gateway Router

  3. DNS (UDP 53)         : [Laptop] ------ Query www.google.com -----> [DNS Server]
                            <----- Returns Web Server IP (142.250.x.x) --

  4. TCP 3-Way Handshake  : [Laptop] ====== SYN -> SYN+ACK -> ACK =====> [Web Server:443]
                            Establishes reliable end-to-end transport connection

  5. TLS 1.3 Handshake    : [Laptop] <===== Certificate & Keys ======> [Web Server]
                            Negotiates encryption keys for secure HTTPS

  6. HTTP GET Request     : [Laptop] ------ GET /index.html ----------> [Web Server]
                            <----- 200 OK + HTML / CSS / JS Payload ----

  7. TCP Connection Close : [Laptop] <===== 4-Way FIN/ACK Teardown ====> [Web Server]
```
**Drawing Notes & Memory Tips:**
* The exact sequence: **DHCP -> ARP -> DNS -> TCP -> TLS -> HTTP -> TCP Teardown**.
* This is the favorite 8-mark master integration question asked by examiners!
**Examiner Marking Rubric:**
* Complete chronological sequence of all 7 protocols: 5.0 Marks (with layer names and functions).
