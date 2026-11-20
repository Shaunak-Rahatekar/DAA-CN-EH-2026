# Computer Networks (CS3213) — Unit IV: Network Layer Model Answers & Marking Scheme

**Target Exam**: End-Semester Written Examination  
**Scope**: Model Solutions with Examiner Marking Rubric for 4M, 8M, and 10M Questions

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. Differentiate between Datagram Packet Switching and Virtual Circuit Packet Switching.
**Model Answer**:

| Parameter | Datagram Packet Switching (Connectionless) | Virtual Circuit Packet Switching (Connection-Oriented) |
| :--- | :--- | :--- |
| **Connection Setup** | No setup phase; packets transmitted immediately upon arrival. | Explicit setup phase required to establish logical virtual path before data transfer. |
| **Routing Overhead** | Each packet carries full 32-bit (IPv4) or 128-bit (IPv6) Source & Destination IP addresses. | Packets carry short, lightweight Virtual Circuit Identifiers (VCI / LCN) in header. |
| **Packet Sequencing** | Packets traverse independent paths, subject to varying delays; **may arrive out-of-order**. | All packets follow the exact same pre-negotiated route; **strictly arrive in-order**. |
| **Fault Resilience** | Highly robust; intermediate switch failure automatically bypasses failed node for next packets. | Fragile; if any intermediate switch fails, the entire virtual circuit crashes and aborts. |
| **Examples** | Internet Protocol (IPv4, IPv6), UDP. | ATM (Asynchronous Transfer Mode), X.25, Frame Relay, MPLS. |

**Marking Scheme**:
- 1 Mark per correctly contrasted parameter (Setup, Routing Overhead, Ordering, Resilience) with examples = **Total 4 Marks**.

---

### Q2. For the IP address `172.16.20.0/22`, determine Subnet Mask, Host Counts, Network ID, and Broadcast Address.
**Model Answer**:
1. **Subnet Mask**:
   - Prefix length = `/22`, meaning 22 contiguous leading 1s followed by 10 zeros.
   - Binary: `11111111.11111111.11111100.00000000`
   - Dotted Decimal: **`255.255.252.0`** *(1 Mark)*
2. **Total and Usable Host Addresses**:
   - Host bits `h = 32 - 22 = 10` bits.
   - Total addresses = 2¹⁰ = **1024 addresses**.
   - Usable host addresses = 2¹⁰ - 2 = **1022 usable hosts** (subtracting Network ID and Directed Broadcast Address). *(1 Mark)*
3. **Network ID**:
   - In the 3rd octet, the mask is 252. Block increment size = 256 - 252 = 4 (or 2^(8 - 6) = 4).
   - Multiples of 4 in the 3rd octet: `0, 4, 8, 12, 16, 20, 24...`
   - The given address has 20 in the 3rd octet, which is an exact multiple of 4 with host bits 0!
   - Therefore, Network ID = **`172.16.20.0`**. *(1 Mark)*
4. **Directed Broadcast Address**:
   - The subnet spans 4 blocks in the 3rd octet: 20, 21, 22, 23.
   - Setting all 10 host bits to 1 gives 3rd octet = 23 and 4th octet = 255.
   - Directed Broadcast Address = **`172.16.23.255`**.
   - *(Valid host range = `172.16.20.1` to `172.16.23.254`)*. *(1 Mark)*

**Marking Scheme**:
- 1 Mark each for Mask, Usable Hosts calculation, Network ID, and Directed Broadcast = **Total 4 Marks**.

---

### Q3. Explain IHL, TTL, Protocol, and Header Checksum fields in the IPv4 header.
**Model Answer**:
1. **Internet Header Length (IHL) (4 bits)**:
   - Measures header length in units of **32-bit (4-byte) words**.
   - Minimum value is `5` (5 × 4 = 20 Bytes, when options = 0). Maximum is `15` (15 × 4 = 60 Bytes, with 40 Bytes options). Enables routers to locate payload start. *(1 Mark)*
2. **Time to Live (TTL) (8 bits)**:
   - Hop counter to prevent datagrams from circulating endlessly in routing loops.
   - Each intermediate router decrements TTL by 1.
   - When TTL reaches 0, the router discards the packet and returns an **ICMP Time Exceeded (Type 11, Code 0)** message to the sender. *(1 Mark)*
3. **Protocol (8 bits)**:
   - Identifies the Layer 4 transport protocol receiving the payload.
   - Common values: `1` = ICMP, `6` = TCP, `17` = UDP, `89` = OSPF. *(1 Mark)*
4. **Header Checksum (16 bits)**:
   - 1's complement sum of all 16-bit words in the IP header only (excludes data payload).
   - Recomputed at **every router hop** because TTL decrements at each hop, altering header contents! *(1 Mark)*

**Marking Scheme**:
- 1 Mark per field with accurate technical explanation and ICMP Type 11 mention = **Total 4 Marks**.

---

### Q4. Describe the operation of ARP. Distinguish Request vs Reply, and define Gratuitous ARP.
**Model Answer**:
1. **ARP Operation**:
   - Resolves a known 32-bit Layer 3 IP address to an unknown 48-bit Layer 2 hardware MAC address on a local broadcast domain.
   - Host checks local **ARP Cache**. If no entry exists:
     - **ARP Request**: Broadcasts a frame with destination MAC `FF:FF:FF:FF:FF:FF` asking *"Who has IP X.X.X.X? Tell my IP and MAC."* All hosts on the local segment process this packet. *(1.5 Marks)*
     - **ARP Reply**: The host that owns the queried IP sends a **Unicast** frame directly back to the requester's MAC address containing its hardware MAC address. The requester caches the mapping. *(1.5 Marks)*
2. **Gratuitous ARP**:
   - An ARP Request sent by a host asking for its **own** IP address (`Source IP = Target IP`).
   - Purposes:
     1. **IP Conflict Detection**: If any reply is received, another machine on the LAN is illegally using that IP.
     2. **ARP Cache Update**: Informs switches and neighboring hosts of a changed MAC address (e.g., after NIC replacement or clustering failover). *(1 Mark)*

**Marking Scheme**:
- 1.5 Marks for ARP Request mechanism, 1.5 Marks for ARP Reply mechanism, 1 Mark for Gratuitous ARP = **Total 4 Marks**.

---

### Q5. What causes the Count-to-Infinity problem in Distance Vector routing? How do Split Horizon and Poison Reverse resolve it?
**Model Answer**:
1. **Cause of Count-to-Infinity**:
   - In Distance Vector (Bellman-Ford), routers "route by rumor", exchanging tables only with immediate neighbors.
   - When a link fails (e.g., Link B-C breaks), Router B detects the failure and sets cost(C) = ∞.
   - However, neighbor Router A previously advertised cost(C) = 2 (which routed via B).
   - Router B receives A's advertisement and mistakenly assumes A has an *alternate independent path* to C of cost 2!
   - B updates its cost to C as `2 + 1 = 3` (via A). A then updates its cost to `3 + 1 = 4` (via B).
   - This mutual loop increments the distance vector in a ping-pong manner until cost reaches the defined infinity (16 in RIP), delaying network convergence for minutes. *(2 Marks)*
2. **Mitigation Techniques**:
   - **Split Horizon**: A router never advertises a route back out the same interface through which it learned that route. (A never tells B about route C because A learned C through B). *(1 Mark)*
   - **Poison Reverse (Split Horizon with Poisoned Reverse)**: Instead of suppressing the advertisement, Router A explicitly advertises the route back to Router B with a cost set to **infinity (16)**, explicitly breaking the loop immediately. *(1 Mark)*

**Marking Scheme**:
- 2 Marks for root cause explanation, 1 Mark for Split Horizon, 1 Mark for Poison Reverse = **Total 4 Marks**.

---

### Q6. Compare Leaky Bucket with Token Bucket traffic shaping. Why is Token Bucket preferred for bursty computer traffic?
**Model Answer**:

| Feature | Leaky Bucket Algorithm | Token Bucket Algorithm |
| :--- | :--- | :--- |
| **Operation** | Packets enter a finite buffer and leak out at a strictly constant rate `r`. | Tokens enter a bucket of capacity `b` at rate `r`. Packets capture tokens to transmit. |
| **Traffic Nature** | Strictly rigid, smooth output rate regardless of prior idle time. | Permits **controlled bursts** at wire speed when accumulated tokens are available. |
| **Discard Policy** | Packets are discarded when packet buffer capacity `C` overflows. | Tokens are discarded when token bucket `b` overflows; packets wait in queue. |
| **Bursty Data Suitability** | Poor; throttles bursty data transfers even during periods of completely idle network. | **Ideal**; accumulates transmission credits during idle silence and releases high-speed bursts. |

- **Why Token Bucket is Preferred**: Computer data traffic is inherently bursty (e.g., web page fetch, file download, database query). A client is idle for seconds, then requests a 5 MB file. Leaky bucket unnecessarily penalizes the client by forcing transmission at a low fixed rate. Token Bucket accumulates tokens during idle time, allowing the entire burst to transmit instantaneously at full link speed up to bucket limit `b`. *(4 Marks)*

**Marking Scheme**:
- 2.5 Marks for comparison matrix, 1.5 Marks for bursty computer data justification = **Total 4 Marks**.

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. IPv4 Datagram Header Format & Fragmentation Numerical
**Model Answer**:

#### Part 1: IPv4 Header Format Diagram & Fields (4 Marks)
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
|                    Options + Padding (0 - 40 Bytes)           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
- Minimum header = 20 Bytes (when IHL = 5); Maximum = 60 Bytes (IHL = 15).

#### Part 2: Fragmentation Numerical (4 Marks)
- **Given**: Total size = 4000 Bytes (Header = 20 Bytes, Data Payload = 3980 Bytes). MTU = 1500 Bytes.
- **Maximum Data per Fragment**:
  `Max Payload = MTU - Header = 1500 - 20 = 1480 Bytes`.
  Checking 8-byte boundary condition: `1480 / 8 = 185` (exact integer; valid!).
- **Number of Fragments**:
  - Fragment 1 takes 1480 Bytes payload (leaving 3980 - 1480 = 2500 Bytes).
  - Fragment 2 takes 1480 Bytes payload (leaving 2500 - 1480 = 1020 Bytes).
  - Fragment 3 takes remaining 1020 Bytes payload.
  Total = **3 Fragments**.

- **Fragment Calculation Table**:

| Parameter | Original Datagram | Fragment 1 | Fragment 2 | Fragment 3 |
| :--- | :--- | :--- | :--- | :--- |
| **Data Payload (Bytes)** | 3980 Bytes | 1480 Bytes (0 to 1479) | 1480 Bytes (1480 to 2959) | 1020 Bytes (2960 to 3979) |
| **Total Length (Header+Data)** | 4000 Bytes | **1500 Bytes** | **1500 Bytes** | **1040 Bytes** |
| **More Fragments (MF) Flag** | 0 | **1** | **1** | **0** |
| **Don't Fragment (DF) Flag** | 0 | **0** | **0** | **0** |
| **Fragment Offset** | 0 | `0 / 8 =` **0** | `1480 / 8 =` **185** | `2960 / 8 =` **370** |

**Marking Scheme**:
- 4 Marks for accurate 32-bit header diagram and field explanations.
- 1 Mark for MTU boundary analysis (1480 bytes payload check).
- 3 Marks (1 mark per fragment) for correct Total Length, MF flag, and Offset = **Total 8 Marks**.

---

### Q8. Subnet Design for Block `130.34.12.64/26` into 4 Equal Subnets
**Model Answer**:

1. **Analysis of Given Block**:
   - Initial Prefix = `/26`.
   - Host bits in block = 32 - 26 = 6 bits. Total IP addresses = 2⁶ = 64 (Range: `130.34.12.64` to `130.34.12.127`).
2. **Subnet Bits Required**:
   - Required Subnets = 4.
   - Borrowed bits `s`: `2ˢ ≥ 4 ⟹ s = 2` bits. *(1 Mark)*
3. **Subnet Mask**:
   - New Prefix Length = 26 + 2 = **`/28`**.
   - Mask Binary: `11111111.11111111.11111111.11110000`
   - Dotted Decimal Mask: **`255.255.255.240`**. *(1 Mark)*
4. **Addresses & Usable Hosts per Subnet**:
   - Remaining host bits `h = 32 - 28 = 4` bits.
   - Total addresses per subnet = 2⁴ = **16 addresses**. *(1 Mark)*
   - Usable host addresses per subnet = 2⁴ - 2 = **14 hosts**. *(1 Mark)*
5. **Subnet Allocation Table**: *(4 Marks — 1 Mark per subnet)*

| Subnet Index | Network ID | First Usable Host IP | Last Usable Host IP | Directed Broadcast Address |
| :---: | :---: | :---: | :---: | :---: |
| **Subnet 0** | `130.34.12.64/28` | `130.34.12.65` | `130.34.12.78` | `130.34.12.79` |
| **Subnet 1** | `130.34.12.80/28` | `130.34.12.81` | `130.34.12.94` | `130.34.12.95` |
| **Subnet 2** | `130.34.12.96/28` | `130.34.12.97` | `130.34.12.110` | `130.34.12.111` |
| **Subnet 3** | `130.34.12.112/28`| `130.34.12.113`| `130.34.12.126`| `130.34.12.127`|

**Marking Scheme**:
- 1 Mark for subnet bits `s = 2`, 1 Mark for `/28` & `255.255.255.240`, 1 Mark for Total IPs (16), 1 Mark for Usable Hosts (14), 4 Marks for accurate allocation table = **Total 8 Marks**.

---

### Q9. Exhaustive Comparison: Distance Vector vs Link State Routing
**Model Answer**:

| Parameter | Distance Vector Routing | Link State Routing |
| :--- | :--- | :--- |
| **1. Underlying Algorithm** | **Bellman-Ford Algorithm**: Iterative, distributed calculation based on neighbor distance vectors. | **Dijkstra's SPF Algorithm**: Centralized calculation run independently on local copy of global topology graph. |
| **2. Information Shared** | Router sends its **entire routing table** (vector of distances to all known destinations). | Router sends only the **state and cost of its directly connected links** (Link State Packet - LSP). |
| **3. Recipients of Updates** | Shared **only with directly connected immediate neighbors**. | Flooded reliably to **all routers in the entire autonomous system/area**. |
| **4. Update Frequency** | **Periodic** timers (e.g., every 30 seconds in RIP), plus triggered updates on failure. | **Event-driven** triggered updates (sent immediately when a link status changes). |
| **5. Convergence Speed** | **Slow convergence**; takes many periodic cycles to propagate changes across hops. | **Very fast convergence**; LSPs flood rapidly across the topology. |
| **6. Loop Susceptibility** | Prone to **routing loops** and the **Count-to-Infinity** problem. | **Loop-free**; every router calculates a cycle-free shortest path tree. |
| **7. Router Overhead** | **Low CPU and low memory**; stores only vectors, requires trivial arithmetic. | **High CPU and memory**; stores full network graph (LSDB) and computes Dijkstra SPF. |
| **8. Real-World Examples** | **RIP (v1, v2)**, IGRP, Cisco EIGRP (advanced DV). | **OSPF (Open Shortest Path First)**, IS-IS. |

**Marking Scheme**:
- 1 Mark per correctly contrasted parameter across all 8 rows = **Total 8 Marks**.

---

### Q10. NAT and PAT Mechanics with Worked Concurrent Session Table
**Model Answer**:

#### Part 1: NAT & PAT Principles (4 Marks)
- **NAT (Network Address Translation)**: Translates private RFC 1918 IPv4 addresses (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) to globally routable public IP addresses.
- **PAT (Port Address Translation / NAT Overload)**:
  - While basic NAT requires one public IP per active host, PAT maps **thousands of internal private hosts to a single public IP**.
  - Mechanism: The router rewrites the source IP to its public IP and assigns a **unique Layer 4 source port number** (from dynamic port range 1024-65535) to each outgoing session.
  - Why Essential: Overcomes IPv4 address exhaustion, allowing an entire enterprise/home network of hundreds of devices to operate on one single public IP allocated by an ISP. *(4 Marks)*

#### Part 2: Step-by-Step Translation Walkthrough & Table (4 Marks)
- **Scenario**:
  - `Host A (192.168.1.10:5000)` and `Host B (192.168.1.20:6000)` concurrently connect to web server `93.184.216.34:80`.
  - NAT Router has public interface IP `203.0.113.5`.

**NAT Translation Table**:

| Session ID | Internal Source IP:Port | External NAT IP:Port | Destination Server IP:Port | Status |
| :---: | :---: | :---: | :---: | :---: |
| 1 | `192.168.1.10:5000` | `203.0.113.5:10001` | `93.184.216.34:80` | Established |
| 2 | `192.168.1.20:6000` | `203.0.113.5:10002` | `93.184.216.34:80` | Established |

- **Packet Flow**:
  1. **Outbound**: Host A sends packet `[Src: 192.168.1.10:5000, Dst: 93.184.216.34:80]`. NAT router rewrites source to `[Src: 203.0.113.5:10001]` and records entry 1.
  2. **Inbound Reply**: Server replies `[Src: 93.184.216.34:80, Dst: 203.0.113.5:10001]`. NAT router matches destination port `10001` in table, rewrites destination to `192.168.1.10:5000`, and forwards to Host A.

**Marking Scheme**:
- 4 Marks for theory of NAT, PAT, and why PAT is essential.
- 4 Marks for clear translation table, port mapping, and bidirectional packet flow walkthrough = **Total 8 Marks**.

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. IPv4 vs IPv6 Architecture, Header Analysis, and Transition Strategies
**Model Answer**:

#### Part 1: IPv6 Base Header Format & Field Explanation (5 Marks)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label (20 bits)        |  Row 0
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |  Row 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                    Source IP Address (128 bits)               +  Rows 2 - 5
|                            (16 Bytes)                         |  (128 bits)
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                 Destination IP Address (128 bits)             +  Rows 6 - 9
|                            (16 Bytes)                         |  (128 bits)
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
- **The 8 Fields**:
  1. **Version (4 bits)**: Set to `6` (`0110`).
  2. **Traffic Class (8 bits)**: Equivalent to IPv4 ToS/DiffServ; handles QoS packet priority and ECN.
  3. **Flow Label (20 bits)**: Identifies packets of a specific real-time flow (e.g., VoIP/streaming) to receive identical routing without deep header inspection.
  4. **Payload Length (16 bits)**: Size of data payload following the fixed 40-byte base header (includes extension headers).
  5. **Next Header (8 bits)**: Replaces IPv4 Protocol field. Points to the first Extension Header (Hop-by-hop, Routing, Fragment) or Upper-Layer Protocol (TCP `6`, UDP `17`).
  6. **Hop Limit (8 bits)**: Replaces IPv4 TTL. Decremented by 1 at each router. Discarded at 0.
  7. **Source Address (128 bits / 16 Bytes)**: Originating host interface address.
  8. **Destination Address (128 bits / 16 Bytes)**: Final destination interface address.

#### Part 2: Why Header Checksum and Router Fragmentation Were Removed (2 Marks)
1. **Header Checksum Removed**:
   - In IPv4, every router had to recompute the checksum at every hop because TTL changed, creating a major CPU bottleneck.
   - High-reliability optical networks and Layer 2 CRC-32 combined with Layer 4 end-to-end TCP/UDP checksums make IP-level checksumming redundant.
2. **Router Fragmentation Removed**:
   - Routers buffering and fragmenting packets caused massive memory latency and out-of-order reassembly issues.
   - In IPv6, routers simply drop packets exceeding MTU and send an **ICMPv6 Packet Too Big** message. The sending host discovers Path MTU (PMTUD) and fragments at the source.

#### Part 3: IPv4 to IPv6 Transition Strategies (3 Marks)
1. **Dual Stack**:
   - Routers and hosts run both IPv4 and IPv6 protocol stacks simultaneously. Devices communicate in IPv6 when both support it, falling back to IPv4 for legacy services. *(1 Mark)*
2. **Tunneling (6to4, Teredo)**:
   - Encapsulates IPv6 packets inside standard IPv4 packet payloads (`Protocol = 41`) to cross legacy IPv4-only transit backbones between IPv6 islands. *(1 Mark)*
3. **Header Translation (NAT64 / DNS64)**:
   - Gateway routers translate IPv6 packet headers directly into equivalent IPv4 headers (and vice-versa), enabling purely IPv6 hosts to communicate directly with legacy IPv4 servers. *(1 Mark)*

**Marking Scheme**:
- 5 Marks for IPv6 header diagram and 8 field descriptions.
- 2 Marks for Checksum & Fragmentation elimination rationale.
- 3 Marks for Dual Stack, Tunneling, and NAT64 = **Total 10 Marks**.

---

### Q12. Dijkstra's Algorithm Execution & Routing Protocols (BGP & MPLS)
**Model Answer**:

#### Part 1: Dijkstra's Algorithm Execution from Node A (6 Marks)
- **Given Topology**:
  - `(A, B) = 3`, `(A, C) = 1`, `(B, C) = 7`, `(B, D) = 5`, `(B, E) = 1`, `(C, D) = 2`, `(D, E) = 7`
- **Initialization**:
  - Source `A`: `D(A) = 0`, `p(A) = -`
  - Direct neighbors of A: `D(B) = 3 (via A)`, `D(C) = 1 (via A)`.
  - Non-neighbors: `D(D) = ∞`, `D(E) = ∞`.
  - Permanent Set `S = { A }`.

- **Step-by-Step Iteration Table**:

| Step | Selected Min Node `u ∉ S` | Permanent Set `S` | Tentative Distance `D(B), p(B)` | Tentative Distance `D(C), p(C)` | Tentative Distance `D(D), p(D)` | Tentative Distance `D(E), p(E)` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Init** | - | `{ A }` | `3, A` | `1, A` | `∞, -` | `∞, -` |
| **1** | **C** (min `D=1`) | `{ A, C }` | `3, A` *(3 < 1+7)* | **1, A (Perm)** | `1 + 2 = 3, C` | `∞, -` |
| **2** | **B** (tie `D=3`) | `{ A, C, B }` | **3, A (Perm)** | `1, A` | `3, C` *(3 < 3+5)* | `3 + 1 = 4, B` |
| **3** | **D** (min `D=3`) | `{ A, C, B, D }`| `3, A` | `1, A` | **3, C (Perm)** | `4, B` *(4 < 3+7)* |
| **4** | **E** (min `D=4`) | `{ A, C, B, D, E }`| `3, A` | `1, A` | `3, C` | **4, B (Perm)** |

- **Final Shortest Paths from A**:
  - Destination `A`: Cost = 0, Path = `[A]`
  - Destination `B`: Cost = 3, Path = `A -> B`
  - Destination `C`: Cost = 1, Path = `A -> C`
  - Destination `D`: Cost = 3, Path = `A -> C -> D`
  - Destination `E`: Cost = 4, Path = `A -> B -> E`

- **Resulting Routing Table for Router A**:

| Destination Network/Host | Shortest Cost | Next Hop Router | Outgoing Interface |
| :---: | :---: | :---: | :---: |
| **B** | 3 | `B` | `Interface 1` |
| **C** | 1 | `C` | `Interface 2` |
| **D** | 3 | `C` | `Interface 2` |
| **E** | 4 | `B` | `Interface 1` |

*(4 Marks for execution table, 2 Marks for shortest path tree and routing table)*

#### Part 2: Technical Notes on BGP-4 and MPLS (4 Marks)
1. **Border Gateway Protocol (BGP-4)**:
   - Classified as a **Path Vector Protocol** used for inter-domain routing between Autonomous Systems (AS) across the global Internet.
   - Instead of transmitting simple distance metrics, BGP advertisements carry the **complete sequence of Autonomous System numbers (`AS-PATH`)** traversed to reach a destination network.
   - **Loop Prevention**: If a border router receives an update containing its own AS number in the `AS-PATH` attribute, it immediately discards the route, guaranteeing loop-free inter-domain routing. Runs over reliable **TCP port 179**. *(2 Marks)*
2. **Multi-Protocol Label Switching (MPLS)**:
   - High-performance packet forwarding technology inserting a **32-bit shim header** between Layer 2 and Layer 3 (`[Label (20b) | TC (3b) | S (1b) | TTL (8b)]`).
   - **Label Edge Routers (LER)**: Located at network boundary; inspect IP header, assign an MPLS label (push label), and map packet to a Forwarding Equivalence Class (FEC).
   - **Label Switch Routers (LSR)**: Core routers that forward packets strictly based on short fixed label index lookups in hardware (label swap), bypassing slow IP routing table lookups. Greatly improves throughput and enables traffic engineering. *(2 Marks)*

**Marking Scheme**:
- 6 Marks for Dijkstra step-by-step table and resulting routing table.
- 2 Marks for BGP-4 Path Vector and AS-PATH loop prevention.
- 2 Marks for MPLS shim header and LER/LSR operations = **Total 10 Marks**.
