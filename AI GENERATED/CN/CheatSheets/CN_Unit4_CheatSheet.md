# CN Unit IV: Network Layer — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: Switching, IPv4 & IPv6, CIDR/VLSM, NAT, ARP, ICMP, RIP, OSPF, BGP, Leaky/Token Bucket, MPLS

---

### 1. Switching Techniques Comparison

| Parameter | Circuit Switching | Message Switching | Packet Switching (Datagram) | Packet Switching (Virtual Circuit) |
| :--- | :---: | :---: | :---: | :---: |
| **Connection Setup** | Mandatory | None | None | Mandatory |
| **Path Traversed** | Dedicated physical | Independent hops | Independent dynamic hops | Fixed logical route |
| **Store & Forward** | No | Whole message | Per small packet | Per small packet |
| **Packet Sequencing**| In-order | Complete message | May arrive out-of-order | Strictly in-order |
| **Header Overhead** | None during transfer | High per message | High per packet (32b/128b IPs) | Low (short VCI label) |

---

### 2. IPv4 Addressing, CIDR & Subnetting Formulas
- **Classful Leading Bits & Masks**: Class A (`0`, /8, `255.0.0.0`), Class B (`10`, /16, `255.255.0.0`), Class C (`110`, /24, `255.255.255.0`), Class D (`1110`, Multicast), Class E (`1111`, Experimental).
- **Private RFC 1918 Blocks**: Class A: `10.0.0.0/8`, Class B: `172.16.0.0/12`, Class C: `192.168.0.0/16`. Loopback: `127.0.0.1`.
- **Subnet Formulas**: Subnet bits = `s`, Host bits = `h` (where `n + s + h = 32`).
  - Total Subnets = `2ˢ` | Total IPs/Subnet = `2ʰ` | **Usable Hosts/Subnet** = `2ʰ - 2`.
  - **NetID**: First IP (host bits = all 0s) | **Broadcast (DBA)**: Last IP (host bits = all 1s).
  - **VLSM Golden Rule**: Always satisfy the largest host requirements first in descending order!

---

### 3. IPv4 Datagram Header & Fragmentation
- **Header Length**: Minimum = 20 Bytes, Maximum = 60 Bytes. `IHL × 4 = Header Bytes` (e.g., IHL = 5 ➔ 20 Bytes).
- **Protocol Field**: `1` = ICMP, `6` = TCP, `17` = UDP, `89` = OSPF.
- **Fragmentation**:
  - `DF = 1`: Don't fragment (drop if > MTU, return ICMP Type 3 Code 4). `DF = 0`: Allowed.
  - `MF = 1`: More fragments follow. `MF = 0`: Last fragment.
  - `Fragment Offset` = `Byte Position / 8` (must be multiple of 8 bytes).

---

### 4. IPv4 vs IPv6 Header Differences

| Feature | IPv4 | IPv6 |
| :--- | :--- | :--- |
| **Address Length** | 32 bits (4 Bytes) | **128 bits** (16 Bytes) |
| **Header Size** | 20 to 60 Bytes (Variable) | **Fixed 40 Bytes** (8 fields) |
| **Header Checksum**| Yes (recalculated at every hop) | **None** (relies on L2 CRC & L4 Checksum) |
| **Fragmentation** | Performed by routers & source | **Source host only** (using Path MTU Discovery) |
| **Address Resolution** | ARP (broadcast FF:FF:FF:FF:FF:FF) | **NDP** (Neighbor Discovery Protocol / ICMPv6 multicast) |

---

### 5. Routing Protocols Quick Matrix

| Protocol | Class | Algorithm | Metric | Transport / Port | Scope / Features |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **RIP (v1/v2)** | IGP | Distance Vector (Bellman-Ford) | Hop Count (Max 15; 16 = ∞) | UDP port 520 | 30s periodic broadcast; count-to-infinity |
| **OSPF** | IGP | Link State (Dijkstra SPF) | Cost = `10⁸ / Bandwidth` | IP Protocol 89 | Areas (Area 0 backbone); fast loop-free |
| **BGP-4** | EGP | Path Vector | AS-PATH & Policy Attributes | TCP port 179 | Inter-AS exterior routing; loop-free |

---

### 6. Traffic Shaping & QoS Formulas
- **Leaky Bucket**: Rigid traffic policing. Discards overflow; transmits at strict constant rate `r`.
- **Token Bucket**: Flexible shaping. Generates tokens at rate `r`, accumulates up to capacity `b`.
  - **Max Burst Duration**: `S = b / (M - r)` seconds (where `M` = peak rate, `r` = token rate).
  - **Max Burst Data Transmitted**: `Burst Volume = (b · M) / (M - r)` bits.
