# Computer Networks (CS3213) — Unit IV: Network Layer Comprehensive Notes

**Target Exam**: End-Semester Written Examination (26 November 2026, 11:00 AM – 1:00 PM)  
**Weightage**: ~16 to 20 Marks  
**Universal Notation Rule**: Unicode symbols only (`≤`, `≥`, `×`, `→`, `∈`, `²`, `ⁿ`, `₁`). Clean ASCII diagrams for exam reproduction.

---

## 1. Switching Techniques

Switching is the process of forwarding data from an incoming link to an outgoing link toward its final destination across intermediate nodes.

### 1.1 Circuit Switching
A dedicated physical transmission path (circuit) is established between the sender and receiver before any data transfer begins.
- **Three Phases**:
  1. **Circuit Establishment**: Control signaling reserves dedicated bandwidth, channels, and buffers across all intermediate switches along the path.
  2. **Data Transfer**: Data is transmitted continuously with constant transmission delay and guaranteed bandwidth.
  3. **Circuit Teardown (Disconnect)**: Termination signals release dedicated resources at all intermediate nodes.
- **Key Characteristics**:
  - Continuous data flow; no per-packet header overhead during data phase.
  - Zero queuing delay during transmission (propagation delay dominates).
  - **Major Drawback**: Resource wastage during idle periods (silent gaps in voice calls waste 100% of reserved bandwidth). Blocking occurs if intermediate trunks have no free channels.
- **Applications**: Traditional Public Switched Telephone Network (PSTN), circuit-switched cellular voice.

### 1.2 Message Switching
Entire messages are treated as individual data units.
- **Store-and-Forward**: An intermediate switch receives the *entire* message, stores it completely in local secondary storage/memory, verifies checksum, and forwards it to the next node when the outgoing link is free.
- **Characteristics**:
  - No dedicated connection setup phase.
  - Dynamic routing per message.
  - **Major Drawbacks**: Extreme storage requirements at intermediate nodes; massive latency (a 10 MB message over 10 hops requires receiving and writing 10 MB ten times before final delivery); head-of-line blocking for smaller messages.
- **Applications**: Historical telegram services, early electronic mail store-and-forward systems.

### 1.3 Packet Switching
Messages are partitioned into smaller, standardized chunks called **packets** (typically 64 to 1500 bytes). Each packet carries header metadata (source, destination, sequence numbers). Intermediate switches buffer and forward individual packets.

#### A. Datagram Approach (Connectionless Packet Switching)
- Each packet is treated completely independently of all other packets of the same message.
- Packets carry full source and destination IP addresses.
- Intermediate routers dynamically determine the outgoing route for each packet based on current routing tables.
- **Consequences**:
  - Packets may traverse different paths, encounter varying delays, and arrive **out-of-order**.
  - Packets may be lost or duplicated.
  - Reassembly and ordering must be handled by the transport layer at the destination host (e.g., TCP).
  - Robust against node/link failures: if a router fails mid-session, subsequent packets automatically bypass it.
- **Applications**: The Internet Protocol (IPv4, IPv6), UDP.

#### B. Virtual Circuit Approach (Connection-Oriented Packet Switching)
- A logical end-to-end path (Virtual Circuit) is established prior to data transmission via a setup packet.
- All packets belonging to a stream follow the exact same pre-determined physical route.
- Packets do not carry full 32-bit/128-bit destination IP addresses; instead, they carry a short **Virtual Circuit Identifier (VCI)** or Logical Channel Number (LCN) in their header.
- Routers maintain an internal VC translation table: `(Incoming Port, Incoming VCI) → (Outgoing Port, Outgoing VCI)`.
- **Consequences**:
  - Packets arrive guaranteed **in-order** with minimal per-packet header overhead.
  - If any switch along the virtual circuit fails, the entire virtual circuit crashes and must be re-established.
- **Applications**: ATM (Asynchronous Transfer Mode), X.25, Frame Relay, MPLS.

### 1.4 Comparison Table: Circuit vs Message vs Packet Switching

| Parameter | Circuit Switching | Message Switching | Packet Switching (Datagram) | Packet Switching (Virtual Circuit) |
| :--- | :--- | :--- | :--- | :--- |
| **Dedicated Path** | Yes (Physical) | No | No | Yes (Logical) |
| **Setup Phase** | Mandatory | None | None | Mandatory |
| **Store & Forward** | No | Yes (Entire message) | Yes (Per small packet) | Yes (Per small packet) |
| **Packet Sequencing** | Strictly in-order | Whole message | May arrive out-of-order | Strictly in-order |
| **Bandwidth Utilization** | Low (reserved link wasted if idle) | High (shared) | Very High (statistical multiplexing) | High |
| **Header Overhead** | None during transfer | High per message | High per packet (32b IPs) | Low per packet (short VCI) |
| **Node Failure Impact** | Call drops immediately | Message re-routed | Packet re-routed dynamically | Virtual circuit aborts |

---

## 2. Logical Addressing: IPv4 and IPv6

### 2.1 IPv4 Addressing Principles
An IPv4 address is a **32-bit binary number** uniquely identifying a network interface on a host or router. It is written in dotted-decimal notation: 4 octets separated by dots (e.g., `192.168.10.1`), where each octet ranges from 0 to 255.
- Total IPv4 address space = 2³² = 4,294,967,296 addresses (~4.3 Billion).

### 2.2 Classful Addressing (Legacy Architecture)
Historically, IPv4 was split into five distinct classes based on leading bits:

| Class | Leading Bits | First Octet Range | Default Mask | Network Bits (N) | Host Bits (H) | Number of Networks | Hosts per Network (2ᴴ - 2) | Purpose / Target |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Class A** | `0` | 1 – 126 | `255.0.0.0` (/8) | 8 bits | 24 bits | 128 (2⁷) | 16,777,214 | Massive organizations (IBM, DoD) |
| **Class B** | `10` | 128 – 191 | `255.255.0.0` (/16) | 16 bits | 16 bits | 16,384 (2¹⁴) | 65,534 | Mid-sized universities/enterprises |
| **Class C** | `110` | 192 – 223 | `255.255.255.0` (/24)| 24 bits | 8 bits | 2,097,152 (2²¹) | 254 | Small LANs / small businesses |
| **Class D** | `1110`| 224 – 239 | None | N/A | N/A | N/A | N/A | Multicast groups (OSPF, RIPv2) |
| **Class E** | `1111`| 240 – 255 | None | N/A | N/A | N/A | N/A | Experimental & Research |

#### Special & Reserved IPv4 Blocks (Crucial Exam Questions):
1. **Loopback Address**: `127.0.0.0/8` (specifically `127.0.0.1` = localhost). Packets never leave the local TCP/IP stack; used for local IPC and self-testing.
2. **APIPA (Automatic Private IP Addressing)**: `169.254.0.0/16`. Assigned automatically by Windows/OS when a DHCP client fails to obtain an IP lease.
3. **Private IP Address Ranges (RFC 1918)** (Non-routable on public Internet):
   - **Class A**: `10.0.0.0/8` (`10.0.0.0` to `10.255.255.255`) → 1 single huge private block.
   - **Class B**: `172.16.0.0/12` (`172.16.0.0` to `172.31.255.255`) → 16 contiguous /16 blocks.
   - **Class C**: `192.168.0.0/16` (`192.168.0.0` to `192.168.255.255`) → 256 contiguous /24 blocks.

---

### 2.3 Subnetting, CIDR, and VLSM

#### Subnetting Mechanics:
Subnetting steals bits from the host portion of an IP address and reallocates them as subnet identification bits.
- Let `n` = original network prefix length.
- Let `s` = borrowed subnet bits.
- Let `h` = remaining host bits (where `n + s + h = 32`).
- **Number of Subnets Created** = 2ˢ.
- **Total Addresses per Subnet** = 2ʰ.
- **Usable Host Addresses per Subnet** = 2ʰ - 2.
  - *Why subtract 2?*
    1. First address (all host bits 0) = **Network Identifier (NetID)**.
    2. Last address (all host bits 1) = **Directed Broadcast Address (DBA)**.
    Both cannot be configured on host NICs!

#### CIDR (Classless Inter-Domain Routing - RFC 1519):
Abolished rigid Class A, B, and C boundaries. An IP address is represented as `IP_Address / Prefix_Length` (e.g., `200.10.5.0/26`).
- The prefix length indicates the exact number of contiguous leading 1s in the subnet mask.
- Prevents routing table explosion through **Route Aggregation / Supernetting**.

#### VLSM (Variable Length Subnet Masking):
Allows allocating subnets of differing sizes to different departments to prevent address starvation.
- **Design Procedure**: Always sort subnet requirements in **descending order of host count** before allocating blocks!

---

### 2.4 Subnetting Step-by-Step Worked Numericals

#### Numerical 1 (From VIT Endsem Paper):
**Problem**: An organization is granted the block `130.34.12.64/26`. The organization needs 4 subnets with an equal number of hosts. Design the subnets and determine the subnet mask, total number of addresses, usable range, and broadcast address for each subnet.

**Step-by-Step Solution**:
1. **Analyze Given Block**:
   - Given prefix = `/26`. Total addresses in given block = 2^(32 - 26) = 2⁶ = 64 addresses (from `130.34.12.64` to `130.34.12.127`).
2. **Determine Subnet Bits Needed**:
   - Number of required subnets = 4.
   - 2ˢ ≥ 4 → `s = 2` bits borrowed.
3. **New Prefix and Subnet Mask**:
   - New prefix length = 26 + 2 = `/28`.
   - In binary: `11111111.11111111.11111111.11110000`
   - Subnet Mask = `255.255.255.240`.
4. **Capacity per Subnet**:
   - Remaining host bits `h = 32 - 28 = 4` bits.
   - Total addresses per subnet = 2⁴ = 16 addresses.
   - Usable hosts per subnet = 2⁴ - 2 = 14 hosts.
5. **Subnet Allocation Table**:
   - **Subnet 0**:
     - Network ID: `130.34.12.64/28`
     - Usable Host Range: `130.34.12.65` to `130.34.12.78`
     - Directed Broadcast Address: `130.34.12.79`
   - **Subnet 1**:
     - Network ID: `130.34.12.80/28`
     - Usable Host Range: `130.34.12.81` to `130.34.12.94`
     - Directed Broadcast Address: `130.34.12.95`
   - **Subnet 2**:
     - Network ID: `130.34.12.96/28`
     - Usable Host Range: `130.34.12.97` to `130.34.12.110`
     - Directed Broadcast Address: `130.34.12.111`
   - **Subnet 3**:
     - Network ID: `130.34.12.112/28`
     - Usable Host Range: `130.34.12.113` to `130.34.12.126`
     - Directed Broadcast Address: `130.34.12.127`

---

#### Numerical 2:
**Problem**: For the IP address `172.16.20.0/22`, calculate: (a) Subnet mask, (b) Network ID, (c) Number of usable hosts, (d) Broadcast address, (e) Valid host range.

**Step-by-Step Solution**:
1. Prefix = `/22`.
   - Mask binary: `11111111.11111111.11111100.00000000`
   - **Subnet Mask** = `255.255.252.0`.
2. Host bits `h = 32 - 22 = 10` bits.
   - **Total Hosts** = 2¹⁰ = 1024.
   - **Usable Hosts** = 1024 - 2 = **1022 hosts**.
3. Third octet block size multiplier = 2^(8 - 6) = 4 (or 256 - 252 = 4).
   - Multiples of 4: 0, 4, 8, 12, 16, 20, 24...
   - Third octet is 20, which is an exact multiple of 4!
   - Therefore, **Network ID** = `172.16.20.0`.
4. The block spans 4 values in the 3rd octet: 20, 21, 22, 23.
   - **Directed Broadcast Address** = `172.16.23.255`.
5. **Usable Host Range** = `172.16.20.1` to `172.16.23.254`.

---

### 2.5 Network Address Translation (NAT) & PAT
NAT maps multiple private non-routable IP addresses to one or more globally unique public IP addresses to alleviate IPv4 exhaustion.
- **Types of NAT**:
  1. **Static NAT**: One-to-one permanent mapping between private IP and public IP (used for hosting internal web/mail servers).
  2. **Dynamic NAT**: Many-to-many mapping from a configured pool of public IPs on a first-come, first-served basis.
  3. **PAT (Port Address Translation / NAT Overload)**: Thousands of internal private hosts share a **single public IP address** by translating Layer 4 source port numbers!
- **PAT Translation Table Operation**:
  - Internal Host `192.168.1.10:54321` sends HTTP request to `93.184.216.34:80`.
  - NAT Router rewrites Source IP:Port to `203.0.113.5:10001` and logs mapping in its NAT table.
  - Return traffic to `203.0.113.5:10001` is demultiplexed by NAT table back to `192.168.1.10:54321`.

---

### 2.6 IPv6 Architecture & Features
IPv6 was created to solve IPv4 address depletion.
- **Address Space**: **128 bits** = 2¹²⁸ ≈ 3.4 × 10³⁸ addresses (over 6.6 × 10²³ addresses per square meter of Earth's surface!).
- **Notation**: 8 groups of 4 hexadecimal digits separated by colons (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).
- **Compression Rules**:
  1. *Omission of leading zeros*: `0000` → `0`, `0db8` → `db8`.
  2. *Double Colon (`::`) Substitution*: Contiguous blocks of zeros can be compressed to `::` **only once** per address to avoid ambiguity.
     - Example: `2001:0db8:0000:0000:0000:0000:0000:0001` → `2001:db8::1`.
- **IPv6 Header Innovations**:
  - **Fixed 40-byte base header** (8 fields vs IPv4's 14 fields) enables ultra-fast router hardware processing.
  - **No Header Checksum**: Eliminates per-hop recomputation. Integrity is left to Layer 2 CRC and Layer 4 TCP/UDP.
  - **No Router Fragmentation**: Routers never fragment packets. End hosts discover Path MTU using ICMPv6 and perform source fragmentation if required.
  - **Extension Headers Daisy Chain**: Modular headers (Hop-by-Hop, Routing, Fragment, ESP, AH) inserted between base header and payload via `Next Header` field.
- **Transition Strategies from IPv4 to IPv6**:
  1. **Dual Stack**: Nodes run both IPv4 and IPv6 protocol stacks simultaneously.
  2. **Tunneling**: IPv6 packets are encapsulated inside IPv4 packets across IPv4-only transit backbones (e.g., 6to4, Teredo).
  3. **Header Translation (NAT64 / DNS64)**: Translates IPv6 packet headers directly into IPv4 packet headers for legacy interoperability.

---

## 3. Network Layer Protocols

### 3.1 IPv4 Datagram Header Details
Total size ranges from **20 bytes (minimum, options=0)** to **60 bytes (maximum, options=40 bytes)**.
- **VER (4 bits)**: Version (4 for IPv4).
- **IHL / HLEN (4 bits)**: Internet Header Length in 4-byte words. Formula: `Header Bytes = IHL × 4`. If IHL = 5, size = 20 bytes.
- **Total Length (16 bits)**: Header + Payload in bytes (max 65,535 bytes).
- **Fragmentation Fields**:
  - **Identification (16 bits)**: Links fragments to original datagram.
  - **Flags (3 bits)**: `[Reserved(0) | DF | MF]`.
    - `DF = 1`: Don't Fragment.
    - `MF = 1`: More Fragments follow. `MF = 0`: Last fragment.
  - **Fragment Offset (13 bits)**: Measured in units of **8 bytes**.
    `Byte Offset = Field Value × 8`.
- **TTL (8 bits)**: Decremented at every hop; discards looping packets at TTL = 0.
- **Protocol (8 bits)**: `1` = ICMP, `6` = TCP, `17` = UDP, `89` = OSPF.
- **Header Checksum (16 bits)**: 1's complement sum over header only.

#### Fragmentation Worked Example (High-Yield Numerical):
An IPv4 datagram of 4000 bytes (20 bytes header + 3980 bytes payload) arrives at a router whose outgoing link has an **MTU of 1500 bytes**. Show the fragmentation parameters for all generated fragments.
- **Fragment 1**:
  - Max payload per fragment = (1500 - 20) = 1480 bytes (must be divisible by 8: 1480 / 8 = 185; valid!).
  - Payload carried = 1480 bytes (Bytes 0 to 1479).
  - Total Length = 1480 + 20 = 1500 Bytes.
  - Flags: `DF = 0`, `MF = 1`.
  - Fragment Offset = 0 / 8 = **0**.
- **Fragment 2**:
  - Payload carried = 1480 bytes (Bytes 1480 to 2959).
  - Total Length = 1500 Bytes.
  - Flags: `DF = 0`, `MF = 1`.
  - Fragment Offset = 1480 / 8 = **185**.
- **Fragment 3**:
  - Remaining payload = 3980 - (1480 + 1480) = 1020 bytes (Bytes 2960 to 3979).
  - Total Length = 1020 + 20 = 1040 Bytes.
  - Flags: `DF = 0`, `MF = 0` (Last fragment).
  - Fragment Offset = 2960 / 8 = **370**.

---

### 3.2 Address Resolution Protocol (ARP)
Bridges Layer 3 logical addresses (IPv4) to Layer 2 physical addresses (MAC).
- **Operation**:
  1. Host A checks its local **ARP Cache**. If MAC for destination IP is absent:
  2. **ARP Request**: Host A broadcasts a frame to MAC `FF:FF:FF:FF:FF:FF` asking *"Who has IP 192.168.1.5? Tell 192.168.1.1"*.
  3. All hosts on the local link receive the broadcast; only the host owning `192.168.1.5` processes it.
  4. **ARP Reply**: Target host responds with a **unicast frame** directly to Host A containing its hardware MAC address.
  5. Host A stores the mapping in its ARP Cache with an expiration timer.
- **Special ARP Variants**:
  - **Proxy ARP**: A router answers ARP requests on behalf of remote hosts outside the subnet.
  - **Gratuitous ARP**: A host broadcasts an ARP request for its *own* IP address upon bootup to detect IP address conflicts and update neighbors' ARP caches.

---

### 3.3 Internet Control Message Protocol (ICMPv4)
Operates at Layer 3 (IP protocol field = 1) to provide error reporting and diagnostics. ICMP packets are encapsulated directly inside IP datagrams.
- **Error-Reporting Messages** (Never generated for errors on ICMP error messages!):
  - **Destination Unreachable (Type 3)**:
    - Code 0: Net unreachable, Code 1: Host unreachable, Code 3: Port unreachable (UDP port closed), Code 4: Fragmentation needed but DF bit set.
  - **Time Exceeded (Type 11)**:
    - Code 0: TTL expired in transit (used by `traceroute`).
    - Code 1: Fragment reassembly time exceeded.
  - **Parameter Problem (Type 12)**: Erroneous header field.
  - **Source Quench (Type 4)**: Flow control / throttle request (deprecated).
- **Query Messages**:
  - **Echo Request (Type 8)** & **Echo Reply (Type 0)**: Basis of the `ping` diagnostic utility to test reachability and round-trip time (RTT).
- **Traceroute Mechanism**:
  Sends a sequence of UDP/ICMP packets starting with TTL = 1, then TTL = 2, TTL = 3... Each intermediate router decrements TTL to 0, drops the packet, and returns an **ICMP Time Exceeded (Type 11)** packet, revealing its own IP address and round-trip delay.

---

## 4. Routing Protocols & Algorithms

Routing is the process of selecting optimal paths in a network along which to send network traffic.

### 4.1 Distance Vector Routing (Bellman-Ford Algorithm)
- **Principle**: *"Tell your immediate neighbors your view of the entire world."*
- Every router maintains a routing table containing: `(Destination, Cost/Metric, Next-Hop)`.
- Routers periodically (e.g., every 30 seconds in RIP) broadcast their complete routing table to immediate neighbors only.
- **Bellman-Ford Equation**:
  `D_x(y) = min_v { c(x, v) + D_v(y) }`
  Where `c(x, v)` is cost to direct neighbor `v`, and `D_v(y)` is neighbor `v`'s reported distance to destination `y`.

#### The Count-to-Infinity Problem:
- Distance Vector algorithms react rapidly to "good news" (link cost decrease) but very slowly to "bad news" (link failure).
- **Scenario**:
  `A ---1--- B ---1--- C`
  When link B-C fails, B sets cost to C = ∞. However, A previously advertised cost to C = 2 (via B). B receives A's advertisement and mistakenly concludes that A has an independent alternate route to C of cost 2! B sets its cost to C = 2 + 1 = 3 (next hop A). Then A updates its cost to C = 3 + 1 = 4. The routing loop ping-pongs indefinitely, incrementing by 1 until the cost reaches "infinity" (16 in RIP).
- **Solutions**:
  1. **Define Infinity**: Set a small upper limit (in RIP, ∞ = 16 hops).
  2. **Split Horizon**: A router never advertises a route back out the same interface through which it learned that route!
  3. **Poison Reverse (Split Horizon with Poisoned Reverse)**: A router advertises routes learned on an interface back through that same interface with a metric of **infinity (16)** to explicitly kill reverse loops.
  4. **Hold-Down Timers**: On receiving notice of a dead route, a router ignores any update claiming an alternate path for a fixed cooldown period (e.g., 180s) to allow convergence.

---

### 4.2 Link State Routing (Dijkstra SPF Algorithm)
- **Principle**: *"Tell the entire world your view of your immediate neighbors."*
- **Operational Steps**:
  1. **Neighbor Discovery**: Send HELLO packets on all active links to learn neighbor identities.
  2. **Metric Determination**: Measure cost (propagation delay, bandwidth, hop) to each active neighbor.
  3. **Link State Packet (LSP) Construction**: Package `(Router ID, Sequence Number, Age, List of [Neighbor ID, Link Cost])`.
  4. **Reliable Flooding**: Flood LSPs to **all routers in the entire autonomous system**. Intermediate routers acknowledge LSPs and forward them out all other interfaces.
  5. **Dijkstra Calculation**: Once all routers synchronize an identical **Link State Database (LSDB)** graph, each router independently executes **Dijkstra's Shortest Path First (SPF)** algorithm with itself as the root to construct its shortest path tree and forwarding table.
- **Advantages**:
  - Extremely fast convergence.
  - Immune to count-to-infinity and routing loops.
  - Supports hierarchical area partitioning (OSPF Areas).

---

### 4.3 Internet Routing Protocols Comparison: RIP vs OSPF vs BGP

| Feature | RIP (v1 / v2) | OSPF (Open Shortest Path First) | BGP (Border Gateway Protocol - BGP-4) |
| :--- | :--- | :--- | :--- |
| **Protocol Class** | IGP (Interior Gateway Protocol) | IGP (Interior Gateway Protocol) | EGP (Exterior Gateway Protocol) |
| **Algorithm** | Distance Vector (Bellman-Ford) | Link State (Dijkstra SPF) | Path Vector Protocol |
| **Scope** | Small local enterprise networks | Large intra-domain enterprise/campus | Inter-Autonomous System (Internet core) |
| **Metric** | Hop Count (Max 15; 16 = unreachable) | Cost = 10⁸ / Bandwidth (bps) | Policy-based (AS-PATH, Multi-Exit Disc.)|
| **Update Mechanism** | Periodic broadcasts (every 30 seconds) | Event-driven on link status change | Incremental updates on topology change |
| **Convergence** | Slow (suffers count-to-infinity) | Very fast, loop-free | Moderate (policy-dependent) |
| **Transport Protocol**| UDP port 520 | Directly over IP (Protocol 89) | TCP port 179 |
| **Hierarchy** | Flat | Hierarchical (Backbone Area 0 + Areas)| Autonomous Systems (AS Numbers) |

---

## 5. Congestion Control & Quality of Service (QoS)

Congestion occurs when aggregate packet arrival rate exceeds outgoing link transmission capacity (`Σ λ_in > C`), causing buffer overflow, queuing delays, and packet drops.

### 5.1 Open-Loop vs Closed-Loop Congestion Control
- **Open-Loop (Preventative)**: Protocol design avoids congestion before it occurs (traffic shaping, admission control, packet discard policies). No feedback from switches.
- **Closed-Loop (Reactive)**: Detects congestion dynamically and feeds information back to sources to throttle transmission (Explicit Congestion Notification ECN, Choke packets, TCP AIMD).

### 5.2 Traffic Shaping: Leaky Bucket vs Token Bucket

#### A. Leaky Bucket Algorithm (Traffic Policing):
- Packets enter a finite buffer (bucket) at arbitrary, bursty arrival rates.
- The bucket has a small hole at the bottom that leaks packets onto the network at a **strictly rigid, constant output rate `r`**.
- If packet inflow exceeds bucket buffer capacity `C`, the bucket overflows and arriving packets are **immediately dropped**.
- **Limitation**: Even if the network was idle for 1 hour, a host cannot burst. Bursty file transfers are unnecessarily slowed.

#### B. Token Bucket Algorithm (Traffic Shaping):
- Tokens are deposited into a bucket of fixed capacity `b` by a clock generator at a constant rate of `r` tokens per second.
- Arriving packets must capture tokens to be transmitted: transmitting 1 byte requires 1 token.
- If tokens are present, packets are transmitted immediately.
- If the bucket is full, incoming tokens overflow and are discarded.
- **Key Advantage**: Allows **controlled burstiness**! An idle period accumulates tokens up to capacity `b`, allowing an immediate high-speed burst.

#### Mathematical Derivation of Maximum Burst Duration:
Let:
- `b` = Token bucket capacity (bits or bytes).
- `r` = Token arrival rate (sustainable average transmission rate).
- `M` = Maximum peak link transmission rate (`M > r`).
- `S` = Maximum burst duration in seconds.

During a maximum burst of duration `S`:
- Total data transmitted = `S × M`.
- Total tokens available = Initial capacity `b` + tokens arriving during the burst `(r × S)`.
- Setting them equal:
  `S × M = b + r × S`
  `S × (M - r) = b`
  **`S = b / (M - r)`**
- Maximum volume transmitted in one burst = **`S × M = (b × M) / (M - r)`**.

---

## 6. Advanced Topics: MPLS & Mobile IP

### 6.1 Multi-Protocol Label Switching (MPLS)
Combines the speed of Layer 2 switching with the flexibility of Layer 3 IP routing.
- **Operation**:
  - Instead of inspecting the 32-bit destination IP at every router hop (longest prefix match), ingress routers encapsulate IP packets with a short **32-bit MPLS Label (Shim Header)**.
  - Core **Label Switch Routers (LSR)** forward packets based solely on fast exact-match lookup in their Label Information Base (LIB).
  - Egress routers strip the label and deliver native IP.
- **MPLS Shim Header Format (32 bits)**:
  `[ Label (20 bits) | Traffic Class / Exp (3 bits) | S (Bottom of Stack, 1 bit) | TTL (8 bits) ]`

### 6.2 Mobile IP (RFC 5944)
Allows a mobile node to change its point of attachment to the Internet without altering its permanent home IP address.
- **Entities**:
  1. **Mobile Node (MN)**: Handheld / laptop moving across subnets.
  2. **Home Agent (HA)**: Router on the mobile node's home network that intercepts packets destined for MN.
  3. **Foreign Agent (FA)**: Router on the visited network that assists MN.
  4. **Care-of Address (CoA)**: Temporary IP address assigned to MN on foreign network.
- **Triangular Routing**:
  - Correspondent Node (CN) sends packets to MN's permanent Home IP address.
  - Home Agent intercepts packets, encapsulates them in an outer IP packet (IP-in-IP tunneling), and forwards to the Care-of Address.
  - Foreign Agent decapsulates and delivers to MN.
  - MN replies directly to CN (or via reverse tunneling to bypass ingress filtering).
