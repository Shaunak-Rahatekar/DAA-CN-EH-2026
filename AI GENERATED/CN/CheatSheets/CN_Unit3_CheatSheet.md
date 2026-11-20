# CN Unit III: Medium Access Control (MAC Sublayer) — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: ALOHA, CSMA Variants, CSMA/CD, Binary Exponential Back-off, Ethernet Standards, CSMA/CA (Wi-Fi)

---

### 1. ALOHA Protocols Summary

| Parameter | Pure ALOHA | Slotted ALOHA |
| :--- | :---: | :---: |
| **Transmission Timing** | Anytime (Continuous) | Slot boundaries only |
| **Vulnerable Period** | `2 · T_frame` | `1 · T_frame` |
| **Throughput Formula** | `S = G · e^(-2G)` | `S = G · e^(-G)` |
| **Maximum Efficiency** | **18.4%** (`1 / 2e`) at `G = 0.5` | **36.8%** (`1 / e`) at `G = 1.0` |
| **Synchronization** | None required | Global clock synchronization required |

---

### 2. CSMA Variants (Carrier Sensing)
- **1-Persistent**: Channel idle ➔ Transmit immediately (p=1). If busy, listen continuously. High collision risk when channel clears.
- **Non-Persistent**: Channel idle ➔ Transmit. If busy, wait random delay and re-sense. Low collisions, but idle channel waste.
- **p-Persistent**: Slotted channel. If idle, transmit with probability `p`, defer to next slot with probability `1 - p`.

---

### 3. CSMA/CD & Binary Exponential Back-off (BEB)
- **Golden Condition for CD**: `T_trans ≥ 2 · T_prop` ⟹ `L_min ≥ 2 · T_prop · Bandwidth`.
  - Standard Ethernet: `L_min = 64 Bytes (512 bits)`. Slot time = `51.2 μs` at 10 Mbps.
- **BEB Algorithm**:
  - Collision `n` (1 to 16). Abort if `n > 16`.
  - Range ceiling: `k = min(n, 10)`. Pick random `K ∈ [0, 2^k - 1]`.
  - Back-off wait time = `K · 51.2 μs`.

---

### 4. IEEE 802.3 Ethernet Frame Format
- `Preamble (7 B)` + `SFD (1 B: 10101011)` + `Dest MAC (6 B)` + `Src MAC (6 B)` + `Type/Len (2 B)` + `Data (46 to 1500 B)` + `FCS (4 B: CRC-32)`.
- Total Frame Size: **64 to 1518 Bytes** (excluding Preamble & SFD).
- Padding added if payload < 46 Bytes to reach 64 Bytes minimum.

---

### 5. High-Speed Ethernet Standards Evolution
- **10BASE-T**: 10 Mbps, Cat 3/5 UTP, Manchester, Half/Full duplex.
- **Fast Ethernet (100BASE-TX)**: 100 Mbps, Cat 5 UTP, 4B/5B + MLT-3.
- **Gigabit Ethernet (1000BASE-T)**: 1000 Mbps, Cat 5e UTP, 4D-PAM5, Full-duplex switched (CSMA/CD obsolete; Carrier Extension & Frame Bursting in half-duplex).
- **10-Gigabit Ethernet**: 10 Gbps, Cat 6A/Fiber, strictly full-duplex, NO CSMA/CD.

---

### 6. Wireless LAN (IEEE 802.11) & CSMA/CA
- **Why No CSMA/CD**: Self-interference ($1/d^2$ signal drowning), Hidden & Exposed terminals.
- **CSMA/CA 4-Way Handshake**:
  - Sender sends **RTS** (carrying Duration).
  - Receiver broadcasts **CTS** (carrying Duration).
  - Hidden nodes hear CTS and set **NAV (Network Allocation Vector)** timer to stay silent.
  - Sender sends **Data Frame**. Receiver returns **ACK** after **SIFS**.
- **IFS Priorities**: `SIFS` (shortest, highest priority: ACK/CTS) < `PIFS` < `DIFS` (data).
