# CN Unit I: Networking Fundamentals & Physical Layer — Complete Solutions & Marking Scheme

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Waveform Notation**: Universal Unicode text symbols.

---

## Section A Solutions (4 Marks Each)

### Q1 Solution: Simplex vs Half-Duplex vs Full-Duplex
**Marking Scheme**:
- Simplex definition and example: 1.5 Marks
- Half-Duplex definition and example: 1.5 Marks
- Full-Duplex definition and example: 1 Mark

**Answer**:
1. **Simplex Mode**:
   - *Definition*: Communication is strictly **unidirectional** (one-way only). One device only transmits while the other only receives. The entire physical bandwidth is allocated to one direction.
   - *Example*: Keyboard to CPU, or traditional television / commercial radio broadcasting.
2. **Half-Duplex Mode**:
   - *Definition*: Communication is **bidirectional**, but **only one direction at a time**. Both stations can send and receive, but when station A is transmitting, station B must only receive.
   - *Example*: Walkie-talkies (using "push-to-talk"), or early IEEE 802.3 CSMA/CD Ethernet over shared coaxial cable.
3. **Full-Duplex Mode**:
   - *Definition*: Communication is **simultaneously bidirectional**. Both stations can transmit and receive concurrently at the exact same moment. The channel capacity is divided into two transmission paths or utilizes two separate physical pairs.
   - *Example*: Modern mobile telephone calls, or modern full-duplex switched Ethernet (100BASE-TX, 1000BASE-T).

---

### Q2 Solution: Physical vs Logical vs Port Addresses
**Marking Scheme**:
- Physical MAC Address (layer, size, scope): 1.5 Marks
- Logical IP Address (layer, size, scope): 1.5 Marks
- Port Address (layer, size, scope): 1 Mark

**Answer**:

| Addressing Level | Operating Layer | Size & Format | Delivery Scope & Purpose |
| :--- | :--- | :--- | :--- |
| **Physical (MAC) Address** | **Data Link Layer** (Layer 2) | **48 bits** (6 Bytes), 12 Hex digits (`00:1A:2B:3C:4D:5E`) | **Hop-to-hop** delivery within the local network (LAN/subnet). Burned into NIC hardware; flat address space. |
| **Logical (IP) Address** | **Network Layer** (Layer 3) | **32 bits** (IPv4, `192.168.1.1`) or **128 bits** (IPv6) | **Host-to-host** global delivery across multiple interconnected networks. Hierarchical address (Net ID + Host ID); routable. |
| **Port Address** | **Transport Layer** (Layer 4) | **16 bits** unsigned integer (`0` to `65,535`) | **Process-to-process** delivery. Identifies the specific receiving application on a destination host (e.g., Port 80 for HTTP). |

---

### Q3 Solution: Collision Domain vs Broadcast Domain & Device Counts
**Marking Scheme**:
- Definition of Collision Domain vs Broadcast Domain: 2 Marks
- Device domain calculations for Hub, Switch, Router: 2 Marks

**Answer**:
1. **Definitions**:
   - **Collision Domain**: A physical or logical network segment where packets can collide with one another if two or more devices transmit simultaneously over a shared medium.
   - **Broadcast Domain**: A logical division of a computer network in which all participating nodes can be reached by a single Layer-2 broadcast frame (destination MAC `FF:FF:FF:FF:FF:FF`).
2. **Device Domain Counts (for an 8-Port Device)**:
   - **8-Port Hub**:
     - *Collision Domains*: **1** (all ports share the exact same physical collision domain).
     - *Broadcast Domains*: **1** (floods all broadcasts to all ports).
   - **8-Port Layer-2 Switch**:
     - *Collision Domains*: **8** (each port is an independent, isolated collision domain via full-duplex microsegmentation).
     - *Broadcast Domains*: **1** (broadcast frames are flooded out of all active ports).
   - **8-Port Router**:
     - *Collision Domains*: **8** (each router interface is an independent collision domain).
     - *Broadcast Domains*: **8** (routers block Layer-2 broadcasts; each interface represents an independent subnet/broadcast domain).

---

### Q4 Solution: Software-Defined Networking (SDN) Fundamentals
**Marking Scheme**:
- Definition and core decoupling principle: 1.5 Marks
- Function of Control Plane: 1 Mark
- Function of Data Plane: 1 Mark
- Standard communication protocol (OpenFlow): 0.5 Mark

**Answer**:
1. **Definition**:
   Software-Defined Networking (SDN) is an architectural framework that decouples the network's **Control Plane** (the decision-making brain) from the underlying **Data Plane** (the physical packet forwarding mechanism).
2. **Control Plane Function**:
   Logically centralized in a software-based **SDN Controller** (e.g., OpenDaylight, ONOS). It maintains a global view of the entire network topology, executes routing algorithms, and dynamically computes flow tables and QoS policies.
3. **Data Plane Function**:
   Resides on physical or virtual switches/routers (Infrastructure Layer). It performs high-speed packet inspection, table matching, and forwarding strictly according to the flow rules pushed down by the controller.
4. **Standard Protocol**:
   The standard Southbound API protocol connecting the Controller to Data Plane switches is **OpenFlow**.

---

## Section B Solutions (8 Marks Each)

### Q5 Solution: OSI 7-Layer vs TCP/IP Suite Comparison
**Marking Scheme**:
- (1) Diagram / Layer mapping between OSI and TCP/IP: 3 Marks
- (2) Explanation of Session and Presentation handling in TCP/IP: 2 Marks
- (3) Structured comparison table: 3 Marks

**Answer**:
**(1) Layer Mapping**:
```text
OSI 7-Layer Model                     TCP/IP Suite
+----------------------+              +----------------------+
| 7. Application       | -----------> |                      |
| 6. Presentation      | -----------> | Application Layer    |
| 5. Session           | -----------> | (HTTP, DNS, SSH, FTP)|
+----------------------+              +----------------------+
| 4. Transport         | -----------> | Transport (TCP / UDP)|
+----------------------+              +----------------------+
| 3. Network           | -----------> | Internet Layer (IP)  |
+----------------------+              +----------------------+
| 2. Data Link         | -----------> | Network Access Layer |
| 1. Physical          | -----------> | (Ethernet, Wi-Fi)    |
+----------------------+              +----------------------+
```

**(2) How TCP/IP Handles Presentation and Session Functions**:
- In TCP/IP, the presentation layer (encryption, compression, character translation) and session layer (dialog control, checkpoints) are **not separate standalone protocol layers**.
- Instead, these functions are implemented directly inside individual **application programs** or standardized application-layer libraries:
  - *Encryption/Decryption*: Handled by SSL/TLS integrated into HTTPS.
  - *Data Representation*: Handled by application-level MIME types, JSON, XML, or ASCII.
  - *Session Management*: Handled via HTTP cookies, session tokens, or SSH sessions.

**(3) Structured Comparison Table**:

| Comparison Criterion | OSI 7-Layer Reference Model | TCP/IP Protocol Suite |
| :--- | :--- | :--- |
| **Number of Layers** | 7 Layers | 4 Layers (or 5 Layers in modern hybrid teaching) |
| **Development Approach** | Theoretical ISO committee standard (Top-down) | Pragmatic, working implementation for ARPANET |
| **Separation of Concepts** | Strictly separates **Services, Interfaces, and Protocols** | Does not clearly distinguish services from protocols |
| **Transport Reliability**| Strictly reliable connection-oriented service | Offers choice: Reliable (TCP) or Best-Effort (UDP) |
| **Network Layer Model** | Supports both Connectionless and Connection-Oriented | Strictly **Connectionless** (IP datagram packet switching) |
| **Adoption & Usage** | Universal educational model; rarely implemented fully | The universal commercial standard powering the Internet |

---

### Q6 Solution: Comparison of Network Topologies
**Marking Scheme**:
- Structured table with all 5 topologies and 4 criteria: 8 Marks (approx 1.6M per column)

**Answer**:

| Topology | Physical Links Formula (for n nodes) | I/O Ports Required Per Node | Installation Cost & Cabling | Fault Tolerance & Failure Impact |
| :--- | :---: | :---: | :--- | :--- |
| **Full Mesh** | `n · (n - 1) / 2` | `n - 1` | **Extremely High** ($O(n²)$ scaling; difficult cable routing) | **Highest**: Failure of any single link affects only that point-to-point pair. No single point of failure. |
| **Star** | `n` | `1` | **Moderate**: Requires one dedicated cable per device to central hub/switch | **High for nodes**: Node cable break disconnects only that node. **Vulnerable**: Central switch failure collapses network! |
| **Bus** | `1` shared backbone | `1` (drop tap) | **Lowest**: Minimal cabling; single backbone cable | **Poor**: A break anywhere in the backbone halts the entire network. Difficult fault diagnosis. |
| **Ring** | `n` | `2` (in/out) | **Moderate**: Easy to install; neat loop cabling | **Poor in single ring**: A single cable or node failure breaks the token circulation, killing network (unless dual-ring). |
| **Tree (Hierarchical)**| `n - 1` | `1` to `k` (switch dependent) | **High**: Structured multi-level hierarchical wiring | **Moderate**: Failure of an edge node affects only that node. Failure of root switch isolates major branches. |

---

### Q7 Solution: DC Component, Clock Recovery, and Baseline Wander
**Marking Scheme**:
- DC Component definition and why it is undesirable: 3 Marks
- Self-Synchronization / Clock Recovery explanation: 3 Marks
- Baseline Wander explanation: 2 Marks

**Answer**:
1. **DC Component (Direct Current Component)**:
   - *Definition*: When a digital signal maintains a non-zero average voltage over time, the signal contains a low-frequency component centered at 0 Hz (Direct Current).
   - *Why Undesirable*: Most physical communication channels use transformers or capacitors for galvanic isolation (AC coupling). These components block DC signals completely. A signal with a large DC component will suffer severe signal distortion, voltage attenuation, and wasted transmission energy. **Line codes with zero DC component (e.g., Manchester, AMI) are heavily preferred**.
2. **Self-Synchronization (Clock Recovery)**:
   - *Definition*: In digital transmission, the receiver must sample the incoming signal at the exact center of each bit interval. To do this, the receiver's clock must remain perfectly synchronized with the sender's clock.
   - *Why Needed*: Even microscopic clock drift will cause the receiver to miscount bits during long runs of identical bits (`0000...` or `1111...`).
   - *Solution*: A good line code includes frequent, guaranteed voltage transitions (e.g., Manchester guarantees a transition in the middle of *every* bit), allowing the receiver's phase-locked loop (PLL) circuit to continuously adjust its clock.
3. **Baseline Wander**:
   - *Definition*: The receiver determines whether a bit is `0` or `1` by comparing the incoming voltage against an internal threshold voltage (the baseline).
   - *The Problem*: A long string of consecutive high voltages (`1`s) or low voltages (`0`s) causes the average voltage level (the baseline) to drift upward or downward. This drift causes the receiver to misclassify subsequent bits. Line codes with balanced positive and negative transitions keep the baseline perfectly stable.

---

### Q8 Solution: Legacy Communication Technologies
**Marking Scheme**:
- (a) ATM explanation and reason for obsolescence: 2 Marks
- (b) ISDN explanation and reason for obsolescence: 2 Marks
- (c) Frame Relay explanation and reason for obsolescence: 2 Marks
- (d) SNA explanation and reason for obsolescence: 2 Marks

**Answer**:
**(a) ATM (Asynchronous Transfer Mode)**:
- *Overview*: A high-speed packet-switching technology that broke all data into fixed-size **53-byte cells** (5-byte header, 48-byte payload) to provide deterministic Quality of Service (QoS) for real-time voice and video.
- *Why Outdated*: High "cell tax" overhead (~10%), hardware complexity of segmentation and reassembly (AAL layers), and the overwhelming rise of cheap, simple, high-bandwidth **Gigabit and 10-Gigabit Ethernet**.
**(b) ISDN (Integrated Services Digital Network)**:
- *Overview*: A circuit-switched telecommunications standard designed in the 1980s to deliver digital voice and data over standard copper telephone wires (BRI: 128 kbps; PRI: 1.544 to 2.048 Mbps).
- *Why Outdated*: Highly expensive and painfully slow compared to modern broadband technologies such as **DSL, DOCSIS cable modems, and Fiber-to-the-Home (FTTH)**.
**(c) Frame Relay**:
- *Overview*: A streamlined WAN packet-switching technology operating at Layers 1 and 2, delivering variable-length frames across leased telephone circuits with minimal error checking for speed.
- *Why Outdated*: Eclipsed by **MPLS (Multi-Protocol Label Switching)**, IP-VPNs, and modern SD-WAN, which provide higher throughput, automated traffic engineering, and much lower bandwidth costs.
**(d) SNA (Systems Network Architecture)**:
- *Overview*: A proprietary networking architecture created by IBM in 1974 for connecting mainframe computers with remote terminal controllers.
- *Why Outdated*: A strictly centralized, closed, and proprietary protocol suite that could not compete with the open, vendor-neutral, distributed architecture of the **TCP/IP** suite.

---

## Section C Solutions (10 Marks Each)

### Q9 Solution: Line Encoding Waveforms for Sequence: 0 1 0 0 1 1 1 0
**Marking Scheme**:
- 1. Unipolar NRZ waveform: 2 Marks
- 2. Polar NRZ-L waveform: 2 Marks
- 3. Polar NRZ-I waveform: 2 Marks
- 4. Bipolar AMI waveform: 2 Marks
- 5. Manchester waveform (IEEE 802.3): 2 Marks

**Answer**:
Given bit sequence: `0  1  0  0  1  1  1  0` (8 bit intervals).

```text
Bit Stream:       0       1       0       0       1       1       1       0
Interval:       [ 1 ]   [ 2 ]   [ 3 ]   [ 4 ]   [ 5 ]   [ 6 ]   [ 7 ]   [ 8 ]

1. Unipolar NRZ:
   +V |          +-------+               +---------------+-------+
    0 |  +-------+       +-------+-------+                       +-------+

2. Polar NRZ-L (Standard: 0 = -V, 1 = +V):
   +V |          +-------+               +---------------+-------+
    0 |...................................................................
   -V |  +-------+       +-------+-------+                       +-------+

3. Polar NRZ-I (Transition on 1; initial state assumed -V):
   +V |          +-------+-------+-------+       +---------------+-------+
    0 |...................................................................
   -V |  +-------+                       +-------+

4. Bipolar AMI (0 = 0V, 1 alternates +V and -V):
   +V |          +-------+                       +---------------+
    0 |  +-------+       +-------+-------+       +               +-------+
   -V |                                  +-------+

5. Manchester (IEEE 802.3: 0 = High-to-Low, 1 = Low-to-High):
   +V |  +--+       +--+    +--+    +--+       +--+    +--+    +--+ +--+
    0 |...................................................................
   -V |     +--+ +--+  | +--+  | +--+  | +--+ +--+  | +--+  | +--+  |  +--+
        H -> L   L -> H  H -> L  H -> L  L -> H   L -> H  L -> H    H -> L
```

*Verification Notes*:
1. **Unipolar NRZ**: `0 = 0V, 1 = +V`.
2. **Polar NRZ-L**: Level is `-V` for `0`, `+V` for `1`.
3. **Polar NRZ-I**: Inverts at start of bit 2 (`1`), stays constant for bits 3 and 4 (`0`), inverts at bit 5 (`1`), inverts at bit 6 (`1`), inverts at bit 7 (`1`), stays constant for bit 8 (`0`).
4. **Bipolar AMI**: Bit 2 (`1`) is `+V`, Bit 5 (`1`) is `-V`, Bit 6 (`1`) is `+V`, Bit 7 (`1`) is `-V`. All `0`s are strictly `0V`.
5. **Manchester**: Mid-bit transition in every bit: `0` goes High ➔ Low, `1` goes Low ➔ High.

---

### Q10 Solution: Topology Design & Domain Calculations (30 Nodes)
**Marking Scheme**:
- (a) Mesh calculation and practicality discussion: 3 Marks
- (b.1) Links required in Star: 2 Marks
- (b.2) Collision and Broadcast domains calculation: 3 Marks
- (b.3) Failure analysis: 2 Marks

**Answer**:
**(a) Full Mesh Topology Calculation (n = 30 computers)**:
1. **Number of Physical Cables**:
   ```
   Number of Duplex Links = n · (n - 1) / 2 = 30 · 29 / 2 = 435 physical cables
   ```
2. **Number of I/O Ports Required Per Device**:
   ```
   Ports per computer = n - 1 = 30 - 1 = 29 network interface ports
   ```
3. **Practicality Discussion**:
   This design is **completely impractical and unfeasible**. Standard computers have only 1 or 2 Ethernet ports; equipping every computer with 29 network ports is hardware-prohibitive. Routing 435 heavy copper cables across building floors would create immense physical clutter, maintenance nightmares, and exorbitant cost.

**(b) Redesign with Star Topology (Switches + Router)**:
Architecture: Floor 1 Switch (connected to 15 PCs), Floor 2 Switch (connected to 15 PCs), both connected to 1 Central Router.
1. **Number of Links Required**:
   - 30 cables from computers to their respective floor switches.
   - 2 uplink cables from the two floor switches to the central router.
   - **Total Links = 30 + 2 = 32 cables** (compared to 435 in Mesh!).
2. **Collision Domains & Broadcast Domains**:
   - **Collision Domains**:
     Each port on a Layer-2 switch is an isolated collision domain.
     - 15 PC ports on Switch 1 = 15 Collision Domains.
     - 15 PC ports on Switch 2 = 15 Collision Domains.
     - 2 uplink ports connecting switches to the router = 2 Collision Domains.
     - **Total Collision Domains = 15 + 15 + 2 = 32 Collision Domains**.
   - **Broadcast Domains**:
     A router breaks broadcast domains. Since the router connects the two floor subnets across two distinct interfaces:
     - Floor 1 (Switch 1 subnet) = 1 Broadcast Domain.
     - Floor 2 (Switch 2 subnet) = 1 Broadcast Domain.
     - **Total Broadcast Domains = 2 Broadcast Domains**.
3. **Failure Impact**:
   - *If Floor 1 Switch Fails*: Only the 15 computers on Floor 1 lose network connectivity. Floor 2 computers and the central router remain fully operational.
   - *If Central Router Fails*: Inter-floor communication (between Floor 1 and Floor 2) and external Internet access are severed. However, local communication between computers on the *same* floor continues functioning through their local Layer-2 switch.

---

### Q11 Solution: OSI Protocol Data Units & Encapsulation Journey
**Marking Scheme**:
- 1. Specific PDU name at all 7 layers: 3 Marks
- 2. Step-by-step encapsulation and addressing details (Layers 4, 3, 2): 4 Marks
- 3. Decapsulation process at receiving server: 3 Marks

**Answer**:
#### 1. Specific PDU Name at Each OSI Layer:
- Layer 7 (Application): **Data / Message**
- Layer 6 (Presentation): **Data**
- Layer 5 (Session): **Data**
- Layer 4 (Transport): **Segment** (for TCP) or **Datagram** (for UDP)
- Layer 3 (Network): **Packet**
- Layer 2 (Data Link): **Frame**
- Layer 1 (Physical): **Bits**

#### 2. The Process of Header Encapsulation (Down the Stack):
When a user requests `http://www.vit.edu`:
1. **Application Layer (L7)**: Formulates an HTTP GET request string (`GET / HTTP/1.1`).
2. **Presentation Layer (L6)**: Formats data representation; encrypts via TLS/SSL if HTTPS.
3. **Session Layer (L5)**: Establishes a session token/dialogue context.
4. **Transport Layer (L4 Encapsulation)**:
   - Prepends a **TCP Header** to form a **Segment**.
   - **Addressing Added**: Source Port (ephemeral e.g., `51234`) and Destination Port (`80` for HTTP or `443` for HTTPS). Adds Sequence and Acknowledgment numbers for reliable delivery.
5. **Network Layer (L3 Encapsulation)**:
   - Prepends an **IP Header** to form a **Packet**.
   - **Addressing Added**: Source IP Address (client's IP e.g., `192.168.1.50`) and Destination IP Address (resolved web server IP e.g., `115.240.101.5`). Adds TTL (Time to Live) and Protocol ID (`6` for TCP).
6. **Data Link Layer (L2 Encapsulation)**:
   - Prepends an **Ethernet Header** and appends a **Trailer** to form a **Frame**.
   - **Addressing Added**: Source MAC Address (client's NIC) and Destination MAC Address (default gateway router's MAC, resolved via ARP).
   - **Error Control Added**: Frame Check Sequence (FCS) containing a 32-bit CRC in the trailer.
7. **Physical Layer (L1)**: Encodes frame bits into electrical voltages or optical pulses and transmits them onto the physical cable.

#### 3. Decapsulation at the Receiving Server (Up the Stack):
1. **Physical Layer (L1)**: Receives raw signals and reconstitutes bits.
2. **Data Link Layer (L2)**: Computes CRC; if no errors, verifies destination MAC matches server's NIC. Strips Ethernet header and trailer; hands Packet up to Network Layer.
3. **Network Layer (L3)**: Verifies destination IP matches server. Strips IP header; identifies Transport protocol (TCP); hands Segment up.
4. **Transport Layer (L4)**: Checks TCP checksum and sequence number; checks destination Port 80; passes data stream to the Web Server process (Apache/Nginx).
5. **Application Layers (L5–L7)**: Server application parses HTTP GET request and serves the requested web page!

---

### Q12 Solution: Manchester vs Differential Manchester & Transmission Metrics
**Marking Scheme**:
- (a.1) Manchester waveform for `1 0 1 1 0 0 1`: 3 Marks
- (a.2) Differential Manchester waveform for `1 0 1 1 0 0 1`: 3 Marks
- (b.1) Baud rate calculation and bit vs baud explanation: 2 Marks
- (b.2) Why Manchester is not used for Gigabit Ethernet and 4B/5B, 8B/10B: 2 Marks

**Answer**:
**(a) Waveforms for Sequence `1  0  1  1  0  0  1`**:
- **Manchester (IEEE 802.3)**: Mid-bit transition: `0` = High ➔ Low, `1` = Low ➔ High.
- **Differential Manchester**: Always mid-bit transition. `0` = Transition at start; `1` = No transition at start. (Assume initial signal begins at `-V`).

```text
Bit Sequence:        1       0       1       1       0       0       1
Interval:          [ 1 ]   [ 2 ]   [ 3 ]   [ 4 ]   [ 5 ]   [ 6 ]   [ 7 ]

1. Manchester (IEEE 802.3: 0 = H->L, 1 = L->H):
   +V |               +--+ +--+       +--+    +--+ +--+    +--+       +--+
    0 |...................................................................
   -V |         +--+  |  +--+  | +--+  |  +--+  |  +--+ |  +--+ |  +--+  |
                L -> H   H -> L  L -> H   L -> H   H -> L   H -> L  L -> H

2. Differential Manchester (0 = Transition at start; 1 = No transition at start):
   (Assumed starting level: -V, mid-bit goes to +V for Bit 1)
   +V |         +--+  |  +--+  |       +--+  |  +--+  | +--+  |  +--+  |
    0 |...................................................................
   -V |               +--+  |  +--+ +--+  |  +--+  |  +--+  | +--+  |  +--+
```

**(b) Baud Rate vs Bit Rate & Gigabit Ethernet**:
1. **Baud Rate Calculation for 10 Mbps Manchester**:
   - **Bit Rate**: Number of data bits transmitted per second ($N = 10 \text{ Mbps}$).
   - **Baud Rate (Signal Rate)**: Number of signal level changes (symbols) per second ($S$).
   - In Manchester encoding, every single bit interval requires at least one mid-bit transition and potentially an edge transition:
     ```
     S = 2 · N = 2 · 10 Mbps = 20 Megabaud (20 MBaud)
     ```
   - *Relationship*: Baud rate is twice the bit rate ($c = 1/2$, factor $r = 1/2$).
2. **Why Manchester is NOT used for Gigabit Ethernet (1000 Mbps)**:
   - If Manchester were used for Gigabit Ethernet, transmitting 1000 Mbps would require a signal rate of **2000 MBaud (2 GHz)**!
   - High-frequency signals suffer severe attenuation, electromagnetic interference (EMI), and require exorbitantly expensive cabling and transceiver components.
   - **Modern Solutions**: Instead of Manchester, modern high-speed Ethernet standards use **block coding** and multi-level signaling:
     - Fast Ethernet (100 Mbps) uses **4B/5B** block coding with **MLT-3** (only 25% overhead).
     - Gigabit Ethernet (1000BASE-T) uses **8B/10B** or **4D-PAM5** (Pulse Amplitude Modulation with 5 voltage levels over 4 pairs), transmitting 2 bits per symbol at only 125 MBaud!
