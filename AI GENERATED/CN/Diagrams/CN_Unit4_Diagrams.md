# Computer Networks (CS3213) — Unit IV: Network Layer Diagrams

This document contains high-resolution reference diagrams, exam visual breakdowns, and hand-drawable ASCII blueprints for fast reproduction during the 2-hour written examination.

---

## Diagram 1: IPv4 Datagram Header Format (32-Bit Word Alignment)

![IPv4 Header Format](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit4_Diagram1_IPv4_Header.png)

### Hand-Drawn Exam Redraw Guide (ASCII Blueprint)
When asked to *"Draw and explain the IPv4 header format"* (typically 6 to 8 Marks), draw this exact 5-row grid. Each row represents exactly 32 bits (4 bytes).

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |  Row 0 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|     Fragment Offset     |  Row 1 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |        Header Checksum        |  Row 2 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |  Row 3 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |  Row 4 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options + Padding (Optional)               |  Row 5 (0-40 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Key Field Reference & Exam Scoring Points:
1. **Version (4 bits)**: Specifies IP version (`0100` = IPv4).
2. **IHL / HLEN (4 bits)**: Internet Header Length in 4-byte 32-bit words. Minimum value is 5 (5 × 4 = 20 Bytes); Maximum is 15 (15 × 4 = 60 Bytes).
3. **Type of Service (ToS) / DSCP+ECN (8 bits)**: First 6 bits = Differentiated Services Code Point (DSCP for QoS priority); last 2 bits = Explicit Congestion Notification (ECN).
4. **Total Length (16 bits)**: Entire datagram size (header + data) in bytes. Maximum theoretical size = 2¹⁶ - 1 = 65,535 Bytes.
5. **Identification (16 bits)**: Unique sequence number assigned by the source host to identify all fragments belonging to the same original datagram.
6. **Flags (3 bits)**:
   - Bit 0: Reserved (must be 0).
   - Bit 1: **DF (Don't Fragment)**: 1 = do not fragment (drop packet and return ICMP Type 3 Code 4 if MTU exceeded); 0 = fragmentation permitted.
   - Bit 2: **MF (More Fragments)**: 1 = more fragments follow; 0 = last fragment (or unfragmented packet).
7. **Fragment Offset (13 bits)**: Position of the fragment data relative to the original unfragmented payload, measured in **8-byte units**. (Byte Offset = Field Value × 8).
8. **Time to Live (TTL) (8 bits)**: Hop count limit to prevent infinite routing loops. Each router decrements TTL by 1. When TTL reaches 0, the packet is discarded and an ICMP "Time Exceeded" (Type 11) message is sent to the sender.
9. **Protocol (8 bits)**: Identifies higher-layer protocol receiving the payload: `1` = ICMP, `2` = IGMP, `6` = TCP, `17` = UDP, `89` = OSPF.
10. **Header Checksum (16 bits)**: 1's complement sum of all 16-bit words in the header only. Must be recomputed by every router because TTL changes!
11. **Source & Destination IP (32 bits each)**: 4-byte IPv4 addresses.

---

## Diagram 2: IPv4 vs IPv6 Header Comparison & Architecture

![IPv4 vs IPv6](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit4_Diagram2_IPv4_vs_IPv6.png)

### Hand-Drawn Exam Redraw Guide
```
         IPv4 Header (20-60 Bytes)                       IPv6 Header (Fixed 40 Bytes)
+---------------------------------------+       +---------------------------------------+
| Ver (4) | IHL (4) | ToS (8) | TotLen (16)|   | Ver (4) | Traffic Class (8) | Flow (20) |
+---------------------------------------+       +---------------------------------------+
| Ident (16)  | Flags (3) | FragOffset(13)|   | Payload Length (16) | NextHdr(8) | Hop(8) |
+---------------------------------------+       +---------------------------------------+
|  TTL (8)  | Protocol (8) | Checksum (16)|   |                                       |
+---------------------------------------+       |        Source Address (128 bits)      |
|         Source Address (32 bits)      |       |              (16 Bytes)               |
+---------------------------------------+       +---------------------------------------+
|       Destination Address (32 bits)   |       |                                       |
+---------------------------------------+       |     Destination Address (128 bits)    |
|      Options + Padding (0-40 Bytes)   |       |              (16 Bytes)               |
+---------------------------------------+       +---------------------------------------+
                                                | Next Header Daisy Chain:              |
                                                | IPv6 Base -> Hop-by-Hop -> Routing    |
                                                |           -> Frag -> TCP/UDP Payload   |
                                                +---------------------------------------+
```

### Critical Exam Distinctions (10-Mark Answer Points):
- **Fixed 40-byte base header in IPv6**: Fast hardware routing without parsing variable options.
- **No Header Checksum in IPv6**: Dropped to eliminate per-hop recalculation overhead; L2 CRC and L4 TCP/UDP checksum guarantee integrity.
- **No router fragmentation in IPv6**: Routers never fragment. Fragmentation is strictly performed by the sending host via Path MTU Discovery (PMTUD).
- **Extension Headers Daisy Chain**: The `Next Header` field in the IPv6 header points to the first extension header, which in turn points to the next, terminating at the upper-layer protocol (TCP/UDP).

---

## Diagram 3: Subnetting & VLSM Architecture

![Subnetting and VLSM](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit4_Diagram3_Subnetting_VLSM.png)

### Hand-Drawn Exam Redraw Guide (VLSM Splitting)
```
                              192.168.1.0 /24 (256 Addresses)
                               Mask: 255.255.255.0 (h = 8 bits)
                                             |
                     +-----------------------+-----------------------+
                     |                                               |
             Subnet A (Needs 60 Hosts)                       Remaining Block
             192.168.1.0 /26                                 192.168.1.64 /26
             Mask: 255.255.255.192                           (64 total addresses)
             Range: .1 to .62 (62 usable)                            |
             Bcast: 192.168.1.63                     +---------------+---------------+
                                                     |                               |
                                             Subnet B1 (25 Hosts)            Subnet B2 (20 Hosts)
                                             192.168.1.64 /27                192.168.1.96 /27
                                             Mask: 255.255.255.224           Mask: 255.255.255.224
                                             Range: .65 to .94 (30 usable)   Range: .97 to .126 (30 usable)
                                             Bcast: 192.168.1.95             Bcast: 192.168.1.127
```

### Subnet Calculation Golden Rules:
1. **Subnet Mask**: Network prefix bits = 1s, Host bits = 0s.
2. **Total Addresses per Subnet** = 2^(32 - prefix) = 2^h.
3. **Usable Host Addresses** = 2^h - 2 (subtract Network ID and Directed Broadcast Address).
4. **Network ID**: Bitwise AND of any Host IP with the Subnet Mask.
5. **Directed Broadcast Address**: Network prefix bits kept as-is, all host bits set to 1.

---

## Diagram 4: Distance Vector (Bellman-Ford) vs Link State Routing (Dijkstra)

![Distance Vector vs Link State](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit4_Diagram4_DistanceVector_vs_LinkState.png)

### Hand-Drawn Exam Redraw Guide
```
  [Distance Vector - Bellman-Ford]              [Link State - Dijkstra SPF]
          (Routing by Rumor)                       (Global Map View)

       A -------- B -------- C                   A -------- B
          cost=1     cost=1                      |  \      / |
                                                 |   \ 1  /  |
  - Shares table with NEIGHBORS only.            | 1  \  / 2 |
  - Periodic updates (every 30s in RIP).         |     \/    |
  - Suffers Count-to-Infinity on failure.        D ---------- C
    (cost increments: 3 -> 4 -> ... -> 16)             3
  - Fixes: Split Horizon & Poison Reverse.       - Floods LSPs to ALL routers.
                                                 - Synchronizes identical LSDB.
                                                 - Each node runs Dijkstra SPF.
                                                 - Rapid loop-free convergence.
```

### Bellman-Ford Formula:
`D_x(y) = min_v { c(x, v) + D_v(y) }` for all neighbors `v` of node `x`.

---

## Diagram 5: Traffic Shaping — Leaky Bucket vs Token Bucket

![Leaky vs Token Bucket](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit4_Diagram5_Leaky_vs_TokenBucket.png)

### Hand-Drawn Exam Redraw Guide
```
      [ Leaky Bucket Algorithm ]                      [ Token Bucket Algorithm ]

       Bursty Inflow of Packets                         Token Generator (rate r)
               |                                                   |
               v                                                   v
        +-------------+                                    +---------------+
        \   PACKETS   / (Capacity C)                       |    TOKENS     | (Capacity b)
         \  QUEUED   /  Discard if full                    +---------------+
          \         /                                              |
           +---+---+                                               v
               |                                            +------------+
               v Hole                               Packets |    GATE    | -> Bursty Output
         Constant Outflow                               --->+------------+    Allowed!
         Rate (r packets/sec)
```

### Key Mathematical Comparison:
- **Leaky Bucket**: Rigid policing. Regardless of prior idle time, maximum output rate is strictly constant `r`. Packets exceeding capacity `C` are dropped.
- **Token Bucket**: Flexible shaping. Idle periods allow tokens to accumulate up to bucket capacity `b`. A burst of size `b` can be transmitted instantaneously at peak rate `M`.
- **Maximum Burst Duration Formula**:
  `S = b / (M - r)`
  Where:
  - `b` = Token bucket capacity (bits or bytes).
  - `M` = Maximum link transmission rate (peak rate).
  - `r` = Token arrival rate (sustainable average rate).
  - Maximum burst volume transmitted = `S × M = (b × M) / (M - r)`.
