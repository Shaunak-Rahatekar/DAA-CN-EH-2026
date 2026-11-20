# CN Unit I: Networking Fundamentals & Physical Layer — Complete Exam Notes

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Waveform Notation**: Universal Unicode text symbols.

---

## 1. Data Communication Model & Fundamental Concepts

### 1.1 The General Data Communication Model
Data communication is the exchange of data between two devices via some form of transmission medium.
The standardized communication system consists of 5 fundamental components:
1. **Message**: The information (data) to be communicated (text, numbers, images, audio, video).
2. **Sender (Source)**: The device that creates and transmits the data message (e.g., computer, workstation, telephone handset, video camera).
3. **Receiver (Destination)**: The device that receives the message (e.g., computer, printer, television).
4. **Transmission Medium**: The physical path over which a message travels from sender to receiver (twisted pair, coaxial cable, fiber-optic cable, or radio waves).
5. **Protocol**: A set of rules and conventions that govern data communications. It represents an agreement between the communicating devices; without a protocol, two devices may be connected but cannot communicate.

### 1.2 DTE vs DCE
- **DTE (Data Terminal Equipment)**: Any device that serves as a source or destination for digital data (e.g., personal computers, terminals, servers, routers). DTE converts user information into digital signals.
- **DCE (Data Circuit-terminating Equipment)**: Any device that establishes, maintains, and terminates a connection over a communication line (e.g., modems, network interface cards, CSU/DSU). DCE converts DTE signals into a format suitable for the transmission medium.

### 1.3 Data Transmission Modes
Data transmission between two stations can be categorized into three operational modes:

1. **Simplex Mode**:
   - Communication is **strictly unidirectional** (one-way only).
   - Only one of the two stations can transmit; the other can only receive.
   - The entire channel bandwidth is dedicated to one direction.
   - *Examples*: Keyboard to computer monitor, traditional television and radio broadcasting.

2. **Half-Duplex Mode**:
   - Communication is **bidirectional**, but **only one direction at a time**.
   - Both stations can transmit and receive, but not simultaneously. When one device is sending, the other must listen.
   - *Example*: Walkie-talkies (push-to-talk), original IEEE 802.3 CSMA/CD Ethernet on shared coaxial cable.

3. **Full-Duplex Mode**:
   - Communication is **simultaneous bidirectional** (both directions at the exact same time).
   - Both stations can transmit and receive concurrently. The channel capacity is divided between the two directions (or two separate physical circuits are used).
   - *Examples*: Modern telephone conversations, modern full-duplex switched Ethernet.

### 1.4 Transmission Formats: Serial vs Parallel & Synchronous vs Asynchronous
- **Serial Transmission**: Bits are sent sequentially, one after another, over a single physical channel. Slower per clock cycle than parallel, but eliminates clock skew over long distances; cheaper and used for all modern LAN/WAN communications.
- **Parallel Transmission**: Multiple bits (e.g., 8 bits / 1 byte) are sent simultaneously across multiple parallel wires. Fast over very short distances (e.g., computer motherboard buses), but suffers from **clock skew** (bits arriving out of phase due to slight wire length differences) and high cost over longer distances.
- **Asynchronous Transmission**:
  - Data is transmitted in small, independent characters (typically 1 byte).
  - Each character is framed with a **Start Bit** (binary `0`) to alert the receiver, and one or two **Stop Bits** (binary `1`) to signal the end.
  - An idle line stays at constant high voltage (`1`).
  - Advantage: Cheap, no synchronized master clock needed. Disadvantage: 20–30% overhead due to start/stop bits.
- **Synchronous Transmission**:
  - Data is transmitted as large, continuous blocks (frames) without individual start/stop bits per byte.
  - Sender and receiver clocks are tightly synchronized using embedded clock transitions or a shared clock line.
  - Advantage: High efficiency, minimal overhead. Used in modern high-speed networks.

---

## 2. Network Classification & Network Architectures

### 2.1 Network Types by Geographic Scale
1. **PAN (Personal Area Network)**:
   - Span: Within a range of ~10 meters (individual workspace).
   - Technologies: Bluetooth (IEEE 802.15.1), Zigbee, USB, NFC.
2. **LAN (Local Area Network)**:
   - Span: A room, office building, or university campus (up to a few kilometers).
   - Characteristics: Privately owned, high data rate (100 Mbps to 10 Gbps), low latency, very low error rates.
   - Technologies: Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11).
3. **MAN (Metropolitan Area Network)**:
   - Span: An entire city or town (5 to 50 km).
   - Characteristics: Often owned by a consortium or single provider (cable TV operator, telecom company).
   - Technologies: Cable TV networks, Metro Ethernet, WiMAX.
4. **WAN (Wide Area Network)**:
   - Span: A state, country, continent, or the entire globe.
   - Characteristics: Transits public rights-of-way; owned by multiple telecom service providers; uses leased lines, fiber backbones, and satellite links.
   - Technologies: MPLS, Frame Relay, Internet backbones.

### 2.2 Internet vs an internet vs Intranet
- **The Internet (capital 'I')**: The singular, globally interconnected public network of billions of devices communicating via the TCP/IP protocol suite.
- **an internet (lowercase 'i')**: Any generic interconnection of two or more independent physical networks connected by routers.
- **Intranet**: A strictly private network confined within an organization (e.g., a corporate or university network) that uses standard Internet protocols (TCP/IP, HTTP) but is protected from the public Internet by firewalls and access control.

### 2.3 Network Architectures: Client-Server vs Peer-to-Peer (P2P)

| Feature | Client-Server Architecture | Peer-to-Peer (P2P) Architecture |
| :--- | :--- | :--- |
| **Control Model** | Centralized server controls resources | Decentralized; all nodes have equal status (servents) |
| **Role of Nodes** | Dedicated roles: Clients request, Servers respond | Each node acts as both a client and a server concurrently |
| **Scalability** | Server can become a bottleneck under heavy load | Highly scalable; adding nodes adds serving capacity |
| **Fault Tolerance** | Single point of failure: If server goes down, system halts | Highly robust; failure of individual peers does not stop network |
| **Security & Backup**| Centralized, easy to manage and back up | Difficult to enforce uniform security, access control, or backup |
| **Examples** | Web browsing (HTTP/Apache), DNS, Email (SMTP/IMAP) | BitTorrent, Bitcoin/Blockchain, Skype (early architecture) |

---

## 3. Network Topologies

A **network topology** defines the geometric arrangement of links and nodes.

### 3.1 Full Mesh Topology
- Every device has a dedicated point-to-point link to every other device in the network.
- **Formula for Number of Physical Links**:
  ```
  Number of Duplex Links = n · (n - 1) / 2
  ```
  *(For n = 6 nodes: 6 × 5 / 2 = 15 links).*
- **Number of I/O Ports required per device**: `n - 1`.
- **Advantages**:
  1. *High Fault Tolerance / Robustness*: If one link fails, all other communications continue unaffected.
  2. *Privacy and Security*: Dedicated lines ensure only the intended recipient receives the data.
  3. *No Traffic Congestion*: Each link carries only its own point-to-point traffic.
- **Disadvantages**:
  1. *Excessive Cabling*: High cost of installation and physical cable routing ($O(n²)$ scaling).
  2. *Hardware Overhead*: Each node requires `n - 1` physical I/O ports.

### 3.2 Star Topology
- Each device has a dedicated point-to-point link to a central controller, traditionally a **Hub** or modern **Switch**.
- Number of links: `n`. Number of I/O ports per device: `1`.
- **Advantages**:
  1. *Easy Installation & Reconfiguration*: Adding or removing a device requires only one cable connection to the central hub.
  2. *Fault Isolation*: If a single cable fails, only that specific connected device is disconnected; the rest of the network remains operational.
- **Disadvantages**:
  1. *Single Point of Failure*: If the central switch/hub fails, the **entire network goes down**.
  2. *Cable Dependency*: More cabling required than in a bus topology.

### 3.3 Bus Topology
- A single long cable (the **backbone**) connects all network devices through drop lines and taps.
- Terminators (resistors) are installed at both physical ends of the bus to absorb signals and prevent **signal reflection**.
- **Advantages**:
  1. *Low Cost & Minimal Cabling*: Easy to install in small networks.
- **Disadvantages**:
  1. *Difficult Fault Isolation*: A break anywhere along the backbone cable halts all communication across the entire bus.
  2. *Signal Degradation & Collisions*: Performance degrades rapidly as the number of taps increases.

### 3.4 Ring Topology
- Each device is connected to exactly two adjacent devices, forming an unbroken closed loop.
- Data circulates in a single direction (unidirectional) from device to device using a circulating **Token**. Each device incorporates a repeater that regenerates the signal.
- **Advantages**:
  1. *Deterministic Access*: Every node is guaranteed access when it receives the token (no collisions).
- **Disadvantages**:
  1. *Unidirectional Vulnerability*: A single cable break or node failure breaks the entire loop (mitigated in dual-ring architectures like FDDI).

### 3.5 Tree (Hierarchical) Topology
- A variation of the star topology where central nodes are arranged in a multi-level hierarchy (root hub connects to intermediate switches, which connect to workstations).
- **Advantages**:
  1. *Highly Scalable*: Allows expansion of network reach through sub-branches.
  2. *Point-to-point wiring for individual segments*.
- **Disadvantages**:
  1. *Dependency on Root Node*: If the primary root switch fails, communication between different secondary branches is severed.

---

## 4. Addressing Schemes Across Network Layers

In computer networking, four distinct levels of addresses are used to identify nodes and applications:

```text
Application Layer  ──► Specific Address (URL, email e.g. https://vit.edu)
        │
Transport Layer    ──► Port Address (16-bit integer e.g. Port 80, 443)
        │
Network Layer      ──► Logical Address (32-bit IPv4 / 128-bit IPv6 e.g. 192.168.1.1)
        │
Data Link Layer    ──► Physical / MAC Address (48-bit hex e.g. 00:1A:2B:3C:4D:5E)
```

### 4.1 Physical (MAC) Address
- **Layer**: Data Link Layer (Layer 2).
- **Size & Format**: 48 bits (6 bytes), represented as 12 hexadecimal digits separated by colons or hyphens (e.g., `00:1A:2B:3C:4D:5E`).
- **Structure**:
  - First 24 bits: **OUI (Organizationally Unique Identifier)**, assigned to the manufacturer by IEEE.
  - Last 24 bits: **NIC Serial Number**, uniquely assigned by the manufacturer.
- **Scope**: Local hop-to-hop delivery within the same broadcast domain (LAN). MAC addresses are **flat** and non-routable across the Internet.

### 4.2 Logical (IP) Address
- **Layer**: Network Layer (Layer 3).
- **Size & Format**: 32 bits (IPv4, written in dotted-decimal e.g., `192.168.1.50`) or 128 bits (IPv6, written in colon-hexadecimal).
- **Structure**: Hierarchical address divided into **Network ID** and **Host ID**.
- **Scope**: Global end-to-end host-to-host delivery. IP addresses are routable across multiple independent networks worldwide.

### 4.3 Port Address
- **Layer**: Transport Layer (Layer 4).
- **Size & Format**: 16-bit unsigned integer, ranging from `0` to `65,535`.
- **Purpose**: Identifies a specific process or application running on a host computer (Process-to-Process delivery).
- **Socket Address**: The combination of an IP Address and a Port Number:
  ```
  Socket Address = IP Address : Port Number   (e.g., 192.168.1.50:80)
  ```
- **Port Ranges**:
  - *Well-Known Ports (0 – 1023)*: Assigned to standard protocols (HTTP = 80, HTTPS = 443, DNS = 53, SSH = 22, FTP = 20/21, SMTP = 25).
  - *Registered Ports (1024 – 49151)*: Used by third-party applications.
  - *Dynamic / Ephemeral Ports (49152 – 65535)*: Assigned temporarily by the client OS for outbound sessions.

### 4.4 Specific Address
- **Layer**: Application Layer (Layer 7).
- **Format**: Human-readable strings such as URLs (`https://www.google.com`) or email addresses (`student@vit.edu`).
- **Resolution**: Translated to underlying IP addresses using directory protocols such as the **Domain Name System (DNS)**.

---

## 5. Network Connecting Devices: Operational Layers & Domains

### 5.1 Collision Domain vs Broadcast Domain
- **Collision Domain**: A logical network segment where data packets can collide with one another when two stations transmit simultaneously on a shared medium.
- **Broadcast Domain**: A logical division of a computer network in which all nodes can reach each other by a layer-2 broadcast transmission (`FF:FF:FF:FF:FF:FF`).

### 5.2 Connecting Devices by Layer

| Device | Operating OSI Layer | Collision Domains | Broadcast Domains | Forwarding Basis & Behavior |
| :--- | :--- | :---: | :---: | :--- |
| **Repeater** | Layer 1 (Physical) | 1 | 1 | Regenerates electrical signals; no filtering. |
| **Hub** | Layer 1 (Physical) | **1** | **1** | Multi-port repeater. Blindly floods incoming signal to all ports in half-duplex. |
| **Bridge** | Layer 2 (Data Link) | N (per port) | **1** | Filters and forwards frames using a 2-port MAC address table. |
| **Layer-2 Switch** | Layer 2 (Data Link) | **N** (per port) | **1** | Multi-port bridge. Full-duplex microsegmentation using hardware MAC table (CAM). |
| **Router** | Layer 3 (Network) | **N** (per port) | **N** (per port) | Routes packets using IP Routing Table. **Breaks broadcast domains**! |
| **Layer-3 Switch** | Layer 2 & 3 | **N** (per port) | **N** (per VLAN) | Performs high-speed ASIC-based packet routing between VLANs. |
| **Gateway** | All Layers (1 to 7) | N | N | Converts data and protocol formats between incompatible network architectures. |

> **HIGH-YIELD EXAM SUMMARY**:
> - **Hub**: 1 Collision Domain, 1 Broadcast Domain.
> - **Switch**: Separate Collision Domain per port ($N$ CDs), 1 Broadcast Domain.
> - **Router**: Separate Collision Domain per port ($N$ CDs), Separate Broadcast Domain per port ($N$ BDs).

---

## 6. Reference Models: OSI 7-Layer vs TCP/IP Protocol Stack

### 6.1 The OSI 7-Layer Reference Model (ISO/IEC 7498)
Developed by the International Organization for Standardization (ISO) as a theoretical blueprint for open communication:

1. **Layer 7: Application Layer**
   - Provides network services directly to end-user applications.
   - Functions: File transfer, email, web browsing, directory lookup.
   - Protocols: HTTP, HTTPS, FTP, SMTP, DNS, DHCP, Telnet, SSH.
   - PDU: **Data / Message**.

2. **Layer 6: Presentation Layer**
   - Handles the syntax, semantics, and representation of data.
   - Functions:
     1. *Translation*: Converting between character encodings (e.g., EBCDIC to ASCII).
     2. *Encryption / Decryption*: Securing data for privacy (e.g., SSL/TLS cryptographic formatting).
     3. *Compression*: Reducing the number of bits to be transmitted (JPEG, MPEG, ZIP).
   - PDU: **Data**.

3. **Layer 5: Session Layer**
   - Establishes, maintains, synchronizes, and terminates sessions (dialogues) between communicating applications.
   - Functions:
     1. *Dialog Control*: Determining which party speaks and when (simplex, half-duplex, or full-duplex).
     2. *Synchronization & Checkpointing*: Inserting checkpoints into data streams so that in case of a crash, transmission resumes from the last checkpoint rather than the beginning.
   - PDU: **Data**.

4. **Layer 4: Transport Layer**
   - Responsible for **end-to-end (process-to-process)** delivery of the entire message.
   - Functions:
     1. *Port Addressing (Service-Point Addressing)*: Directs data to the exact receiving application.
     2. *Segmentation and Reassembly*: Divides large messages into numbered segments at source and reassembles them at destination.
     3. *Connection Control*: Connection-oriented (TCP) with three-way handshakes, or connectionless (UDP).
     4. *Flow Control & Error Control*: End-to-end sliding window and segment retransmission.
   - PDU: **Segment** (TCP) or **Datagram** (UDP).

5. **Layer 3: Network Layer**
   - Responsible for **host-to-host delivery** across multiple independent networks.
   - Functions:
     1. *Logical Addressing*: Assigns unique global IP addresses.
     2. *Routing*: Determining the optimal physical path through routers using routing protocols (OSPF, RIP, BGP).
     3. *Packet Switching & Fragmentation*.
   - PDU: **Packet**.

6. **Layer 2: Data Link Layer**
   - Responsible for **node-to-node (hop-to-hop)** error-free delivery of frames across a single physical link.
   - Sublayers:
     - *LLC (Logical Link Control)*: Flow control, error checking, multiplexing network protocols.
     - *MAC (Medium Access Control)*: Controls access to shared transmission media (CSMA/CD, CSMA/CA).
   - Functions:
     1. *Framing*: Packing network-layer packets into discrete frames with headers and trailers.
     2. *Physical (MAC) Addressing*: Adding source and destination hardware MAC addresses.
     3. *Error Control*: Detecting or correcting bit errors using CRC (Cyclic Redundancy Check).
     4. *Flow Control*: Preventing receiver buffer overflow.
   - PDU: **Frame**.

7. **Layer 1: Physical Layer**
   - Responsible for the transmission of **unstructured raw bit streams** over physical media.
   - Functions:
     1. *Signal Representation*: Line encoding and modulation (converting bits to voltage levels or light pulses).
     2. *Data Rate (Bit Rate)*: Number of bits sent per second.
     3. *Physical Topologies & Transmission Modes*: Cable pinouts, connectors (RJ-45), voltages.
   - PDU: **Bits**.

---

### 6.2 The TCP/IP Protocol Stack (4-Layer Architectural Model)
Unlike the theoretical OSI model, the TCP/IP model was designed pragmatically for the ARPANET and is the foundation of the modern Internet:

1. **Application Layer**: Corresponds directly to OSI Layers 5, 6, and 7. Combines user interface, representation, and session management. Protocols: HTTP, DNS, SMTP, FTP, SSH.
2. **Transport Layer**: Matches OSI Layer 4. Protocols: **TCP** (reliable, connection-oriented, ordered delivery, flow/congestion control) and **UDP** (unreliable, connectionless, low-latency, best-effort).
3. **Internet Layer**: Matches OSI Layer 3. Handles packet routing and logical addressing. Protocols: **IP** (IPv4, IPv6), **ICMP** (diagnostics/ping), **ARP** (IP-to-MAC resolution), **IGMP**.
4. **Network Access (Link) Layer**: Combines OSI Layers 1 and 2. Defines hardware interfaces, device drivers, framing, and physical transmission media. Protocols: Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11).

---

### 6.3 Comprehensive Comparison: OSI vs TCP/IP

| Feature | OSI Reference Model | TCP/IP Protocol Suite |
| :--- | :--- | :--- |
| **Number of Layers** | 7 Layers | 4 Layers (or 5 Layers in modern hybrid teaching) |
| **Origin & Design** | Developed theoretically by ISO committee | Implemented practically for ARPANET |
| **Concept Separation** | Strictly distinguishes **Services, Interfaces, and Protocols** | Loosely separates services and protocols |
| **Transport Reliability**| Strictly reliable connection-oriented transport | Choice between reliable (TCP) and unreliable (UDP) |
| **Network Layer Model** | Supports both Connectionless and Connection-Oriented | Strictly **Connectionless** (IP datagram service) |
| **Market Status** | Primarily an educational reference blueprint | The universally deployed global standard |

---

## 7. Digital Line Encoding Techniques

**Line Encoding** is the process of converting digital binary data (bits) into digital signals (voltage waveforms) for transmission over a physical wire.

### 7.1 Evaluation Criteria for Line Coding
1. **DC Component**: When a signal maintains a constant non-zero voltage, it creates a Direct Current (DC) component. Frequencies near zero Hz cannot pass through transformers and capacitors (AC coupling). **Zero DC component is ideal**.
2. **Self-Synchronization (Clock Recovery)**: The signal should have frequent voltage transitions so the receiver's clock can lock onto the sender's clock without losing synchronization during long strings of `0`s or `1`s.
3. **Bandwidth Utilization**: Signal rate (Baud rate, changes per second) versus data rate (Bit rate, bits per second). Lower bandwidth is preferred.
4. **Error Detection**: Built-in signal properties that allow detection of physical transmission errors.

---

### 7.2 Detailed Line Coding Schemes

#### 1. Unipolar NRZ (Non-Return to Zero)
- Binary `1`: High positive voltage (`+V`).
- Binary `0`: Zero voltage (`0V`).
- *Drawbacks*: Severe DC component; total loss of synchronization on long runs of `0`s or `1`s. Rarely used today.

#### 2. Polar NRZ-L (Level)
- Signal level depends strictly on the state of the bit.
- Standard convention: Binary `1` = Positive voltage (`+V`), Binary `0` = Negative voltage (`-V`) (or vice versa depending on textbook convention).
- *Drawbacks*: Substantial DC component; continuous high/low voltage during long runs of identical bits causes synchronization loss and **baseline wander**.

#### 3. Polar NRZ-I (Invert on 1)
- Signal transition indicates bit identity:
  - Binary `1`: **Transition** (voltage level inverts from previous level) at the beginning of the bit interval.
  - Binary `0`: **No transition** (voltage remains unchanged from previous level).
- *Advantage*: Long strings of `1`s provide continuous transitions for clock synchronization.
- *Drawback*: Long strings of `0`s still cause flatlines with no transitions.

#### 4. Bipolar AMI (Alternate Mark Inversion)
- Uses three voltage levels: positive (`+V`), zero (`0V`), and negative (`-V`):
  - Binary `0`: Represented by **constant zero voltage (`0V`)**.
  - Binary `1`: Represented by alternating positive and negative pulses (first `1` is `+V`, second `1` is `-V`, third `1` is `+V`, etc.).
- *Advantages*:
  1. **Zero DC Component**: The alternating positive and negative pulses cancel out average voltage.
  2. **Error Detection**: If two consecutive pulses have the same polarity (called an AMI violation), the receiver immediately detects a transmission error.
- *Drawback*: Long runs of `0`s provide no transitions (resolved in advanced codes like B8ZS and HDB3).

#### 5. Manchester Encoding (IEEE 802.3 Standard)
- Uses an explicit transition in the exact middle of **every single bit interval**:
  - Binary `0`: **High-to-Low transition** in the middle of the interval.
  - Binary `1`: **Low-to-High transition** in the middle of the interval.
  *(Note: Forouzan textbook uses the reverse convention: 0 = Low-to-High, 1 = High-to-Low. Always state your convention explicitly in exams!).*
- *Advantages*:
  1. **Self-Clocking**: Guaranteed transition in every bit interval prevents any loss of synchronization, regardless of the bit sequence.
  2. **Zero DC Component**: Equal time spent in positive and negative voltages.
- *Disadvantage*: **Baud Rate is Double the Bit Rate** (`Baud = 2 · Bit Rate`). Requires twice the bandwidth of NRZ. Used in classic 10 Mbps Ethernet (10BASE-T).

#### 6. Differential Manchester Encoding
- The mid-bit transition is used **strictly for clock synchronization**. The actual data bit is determined by the transition at the **beginning** of the bit interval:
  - Binary `0`: **Transition at the beginning** of the bit interval.
  - Binary `1`: **No transition at the beginning** of the bit interval.
  - There is *always* a transition in the middle of the bit interval.
- *Advantage*: Noise immunity and polarity reversal tolerance (differential detection). Used in Token Ring (IEEE 802.5).

---

## 8. Modern & Legacy Networking Paradigms

### 8.1 Software-Defined Networking (SDN)
Traditional networking tightly integrates control logic and packet forwarding inside individual proprietary router hardware. SDN revolutionizes network management by decoupling these functions into a 3-tier architecture:

1. **Application Plane**:
   - Network applications that define network requirements and business logic (e.g., dynamic traffic engineering, firewalls, load balancers, QoS monitoring).
   - Communicates with the Control Plane via **Northbound APIs** (typically RESTful APIs).
2. **Control Plane (Logically Centralized Controller)**:
   - The central "brain" of the network running a Network Operating System (e.g., OpenFlow Controller, OpenDaylight, ONOS).
   - Maintains a global view of the entire network topology, computes optimal forwarding paths, and generates Flow Tables.
3. **Data Plane (Infrastructure Layer)**:
   - Simple, high-speed forwarding hardware (switches/routers).
   - Executes forwarding instructions by matching incoming packets against Flow Table rules received from the controller.
   - Communicates with the Control Plane via **Southbound APIs** (standardized by the **OpenFlow** protocol).
- **Key Advantages of SDN**:
  - Centralized global network management and automated configuration.
  - Programmability: Rapid deployment of new policies without modifying physical hardware.
  - Vendor independence: Standard commodity hardware switches can be used.

### 8.2 Legacy Networking Technologies: Why Are They Outdated?
1. **ATM (Asynchronous Transfer Mode)**:
   - Uses fixed-size **53-byte cells** (5-byte header, 48-byte payload) for predictable latency in voice and data.
   - *Why Outdated*: High cell tax (nearly 10% overhead for headers), complex hardware segmentation/reassembly (AAL), completely eclipsed by cheap, high-speed **Gigabit and 10-Gigabit Ethernet**.
2. **ISDN (Integrated Services Digital Network)**:
   - Circuit-switched digital telephone line service offering Basic Rate Interface (BRI: 2B+D = 128 kbps) and Primary Rate Interface (PRI: 23B+D / 30B+D = 1.544 / 2.048 Mbps).
   - *Why Outdated*: Slow bandwidth; completely replaced by broadband **DSL, cable modems, and Fiber-to-the-Home (FTTH)**.
3. **Frame Relay**:
   - Variable-length packet switching protocol over leased T1/T3 lines; operates at physical and data link layers with minimal error checking for speed.
   - *Why Outdated*: Replaced by **MPLS (Multi-Protocol Label Switching)**, IP-VPNs, and SD-WAN which provide higher throughput and lower costs.
4. **SNA (Systems Network Architecture)**:
   - Proprietary mainframe networking architecture created by IBM in 1974.
   - *Why Outdated*: Closed, proprietary, centralized mainframe model replaced globally by the open, vendor-neutral **TCP/IP** suite.
