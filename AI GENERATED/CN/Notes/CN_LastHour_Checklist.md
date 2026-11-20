# Computer Networks (CS3213) — 60-Minute Last-Hour Revision Pack

**Target Exam**: 26 November 2026, 11:00 AM – 1:00 PM  
**Usage**: Read during the final 60 minutes before entering the examination hall. Contains 20 rapid-fire facts, 5 core formulas/drawings, and 5 common student traps.

---

## 20 High-Yield Core Facts (Rapid Fire)

1. **Connecting Devices**: A Hub operates at Layer 1 (1 collision domain, 1 broadcast domain). A Layer-2 Switch operates at Layer 2 (each port is a separate collision domain; 1 broadcast domain). A Router operates at Layer 3 (each interface is a separate collision domain and separate broadcast domain).
2. **Hamming Parity Formula**: `2ʳ ≥ m + r + 1` where `m` = data bits, `r` = parity bits. Parity bits are placed strictly at power-of-2 positions (1, 2, 4, 8...).
3. **Bit Stuffing**: Insert a `0` after every five consecutive `1`s (`11111`) to prevent data from masquerading as the frame flag `01111110`.
4. **ALOHA Efficiencies**: Pure ALOHA max throughput = **18.4%** (`1 / 2e`) at `G = 0.5` (vulnerable period = `2 · T_frame`). Slotted ALOHA max throughput = **36.8%** (`1 / e`) at `G = 1.0` (vulnerable period = `1 · T_frame`).
5. **CSMA/CD Golden Rule**: Transmission time must be at least twice propagation delay: `T_trans ≥ 2 · T_prop` ⟹ `L_min ≥ 2 · T_prop · Bandwidth`. Minimum Ethernet frame is **64 Bytes (512 bits)**; max is 1518 Bytes.
6. **Binary Exponential Back-Off (BEB)**: On collision `n`, choose random `K ∈ [0, 2ᵏ - 1]` where `k = min(n, 10)` and wait `K · 51.2 μs`. Abort after 16 collisions.
7. **IPv4 Usable Hosts**: Total addresses = `2ʰ`; usable hosts = `2ʰ - 2` (subtracting Network ID where host bits = all 0s, and Directed Broadcast Address where host bits = all 1s).
8. **Private IP Ranges (RFC 1918)**: Class A: `10.0.0.0/8`, Class B: `172.16.0.0/12` (to `172.31.255.255`), Class C: `192.168.0.0/16`. Loopback: `127.0.0.1`. APIPA: `169.254.0.0/16`.
9. **IPv4 Header Minimum/Maximum**: Minimum = 20 Bytes (`IHL = 5`); Maximum = 60 Bytes (`IHL = 15`). `Header Bytes = IHL × 4`.
10. **Fragment Offset**: Measured strictly in **8-byte units**! `Byte Offset = Field Value × 8`.
11. **IPv6 Key Advances**: 128-bit addresses, fixed 40-byte base header (8 fields), **NO header checksum**, **NO router fragmentation** (source only via PMTUD). Extension headers are daisy-chained via `Next Header`.
12. **ARP vs RARP**: ARP resolves known IP ➔ unknown MAC (Request = Broadcast, Reply = Unicast). RARP resolves known MAC ➔ unknown IP.
13. **ICMP Types**: Type 3 = Destination Unreachable; Type 11 = Time Exceeded (TTL=0 in traceroute); Type 8/0 = Echo Request/Reply (ping).
14. **Distance Vector (Bellman-Ford)**: Exchange entire table with immediate neighbors only. Suffers Count-to-Infinity (mitigated by Split Horizon and Poison Reverse). RIP metric = hop count (max 15; 16 = ∞).
15. **Link State (Dijkstra SPF)**: Flood Link State Packets (LSPs) to all routers; all synchronize identical LSDB graph; each computes SPF tree. Fast, loop-free (OSPF metric = `10⁸ / Bandwidth`).
16. **TCP 3-Way Handshake**: `SYN(x) ➔ SYN(y)+ACK(x+1) ➔ ACK(y+1)`. Prevents old delayed duplicate SYNs from creating half-open zombie connections.
17. **TCP 4-Way Teardown & TIME_WAIT**: `FIN ➔ ACK (CLOSE_WAIT) ➔ FIN ➔ ACK (TIME_WAIT for 2*MSL)`. Guarantees final ACK delivery and flushes wandering duplicate segments.
18. **Silly Window Syndrome (SWS)**: 1 byte data in 40B header = 2.4% efficiency. Fixed by **Nagle's Algorithm** at sender (buffer until full MSS or previous ACK) and **Clark's Solution** at receiver (advertise `rwnd = 0` until free buffer `≥ min(MSS, Buffer/2)`).
19. **Congestion Control (AIMD)**: Slow Start doubles cwnd every RTT (`cwnd < ssthresh`). Congestion Avoidance adds 1 MSS per RTT. Timeout drops `cwnd = 1` and sets `ssthresh = cwnd / 2`. 3 duplicate ACKs in Reno enters **Fast Recovery** (`cwnd = ssthresh`).
20. **QUIC & HTTP/3**: Runs over UDP Port 443; 0-RTT/1-RTT handshake (built-in TLS 1.3); eliminates transport Head-of-Line blocking; supports Connection Migration via 64-bit CID.

---

## 5 Essential Formulas & Quick Sketches

### 1. Sliding Window Protocol Efficiencies:
- Stop-and-Wait: `η = 1 / (1 + 2a)` where `a = T_prop / T_trans`.
- Go-Back-N (Window `N`): `η = N / (1 + 2a)` where `N = 2ᵏ - 1`.
- Selective Repeat (Window `N`): `η = N / (1 + 2a)` where `N = 2ᵏ⁻¹`.
- Optimal window for 100% utilization: `W_opt = 1 + 2a`.

### 2. Traffic Shaping Burst Duration (Token Bucket):
`S = b / (M - r)`
- `b` = Token bucket capacity (bits/bytes)
- `M` = Peak link transmission rate
- `r` = Token arrival rate
- Maximum burst data volume = `S × M = (b × M) / (M - r)`.

### 3. TCP RTO Jacobson's Formula:
- `RTT_s = 0.875 × RTT_s + 0.125 × RTT_m`
- `D = 0.75 × D + 0.25 × |RTT_s - RTT_m|`
- **`RTO = RTT_s + 4 × D`**  
*(Karn's Rule: Never update RTO on retransmitted packets; double RTO on timeout).*

### 4. Usable Send Window Rule:
`Usable Window = min(cwnd, rwnd) - (LastByteSent - LastByteAcked)`

### 5. Base64 MIME Encoding:
- 3 binary bytes (24 bits) ➔ 4 printable ASCII characters (values 0-63).
- Size overhead = **`+33.3%`**. Padding: 1 byte leftover ➔ `==`, 2 bytes leftover ➔ `=`.

---

## 5 Frequent Exam Traps & Examiner Pitfalls

1. **Subnetting Trap (Subtracting 2)**:
   - *Trap*: Forgetting to subtract 2 when asked for *usable host addresses*.
   - *Rule*: Total addresses = `2ʰ`. Usable host addresses = `2ʰ - 2`. First address is Network ID (all host bits 0); last address is Directed Broadcast Address (all host bits 1). Neither can ever be assigned to a host NIC!
2. **Fragment Offset Division Trap**:
   - *Trap*: Writing the byte position directly into the IPv4 Fragment Offset field.
   - *Rule*: The Fragment Offset field measures offset in **units of 8 bytes**. Always divide the byte offset by 8! (e.g., Byte 1480 ➔ Offset = `1480 / 8 = 185`).
3. **Hamming Code Parity Placement Trap**:
   - *Trap*: Numbering bit positions starting from 0, or placing parity bits consecutively at the beginning.
   - *Rule*: Bit positions start strictly at **1** (not 0). Parity bits are placed strictly at **powers of 2** (`Position 1, 2, 4, 8, 16...`).
4. **TCP Timeout vs 3 Duplicate ACKs Trap**:
   - *Trap*: Dropping `cwnd = 1` on 3 duplicate ACKs in TCP Reno.
   - *Rule*: In TCP Reno, only a **Timeout** drops `cwnd = 1`. A **Triple Duplicate ACK** triggers Fast Recovery: `ssthresh = cwnd / 2`, and `cwnd = ssthresh` (does NOT drop to 1; stays in Congestion Avoidance!).
5. **FTP Connection Direction Trap**:
   - *Trap*: Thinking client connects to Port 20 for data transfer in Active FTP.
   - *Rule*: In Active Mode, the client opens Port 21 (Control), but the **SERVER initiates the data connection from Port 20 to the client's port**! This is why client firewalls block Active FTP. In Passive Mode (`PASV`), the client initiates both connections.
