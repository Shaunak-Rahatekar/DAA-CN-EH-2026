# CN Unit II: Logical Link Control — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: Framing, Error Control (Hamming & CRC), Flow Control (Stop-and-Wait, GBN, SR), HDLC, PPP

---

### 1. Framing Methods
- **Byte Stuffing**: Delineated by `FLAG = 0x7E`. Prepend `ESC = 0x7D` before any accidental `FLAG` or `ESC` in payload.
- **Bit Stuffing**: Delineated by `FLAG = 01111110`. Insert `0` after five consecutive `1`s (`11111 ➔ 111110`). Receiver strips `0`.

---

### 2. Error Control: Hamming Code & CRC
- **Hamming Code Formula**: `2^r ≥ m + r + 1` (where `m` = data bits, `r` = parity bits).
  - Parity bit positions: Powers of 2 (`P1, P2, P4, P8, ...`).
  - Syndrome `S = (s4 s2 s1)`: Decimal value gives exact corrupted bit position!
  - Distance bounds: Detect `s` errors: `d_min ≥ s + 1`; Correct `t` errors: `d_min ≥ 2t + 1`.
- **Cyclic Redundancy Check (CRC)**:
  - Modulo-2 XOR division. Append `r` zeros (`r = degree of G(x)`).
  - Remainder `R(x)` is CRC checksum. Transmitted = `Data ⊕ Remainder`.
  - Receiver divides received frame by `G(x)`. Remainder `0` ⟹ No error!

---

### 3. Sliding Window Flow Control Protocols

| Feature | Stop-and-Wait ARQ | Go-Back-N (GBN) ARQ | Selective Repeat (SR) ARQ |
| :--- | :---: | :---: | :---: |
| **Sender Window (W_S)** | `1` | `≤ 2^m - 1` | `≤ 2^(m - 1)` |
| **Receiver Window (W_R)**| `1` | `1` (strictly) | `W_R = W_S ≤ 2^(m - 1)` |
| **Receiver Buffer** | None | None (discards out-of-order) | Buffers out-of-order frames |
| **ACK Type** | Individual | **Cumulative ACK** | **Individual / Selective ACK** |
| **Retransmission** | 1 frame | **All frames in window** | **Only the lost frame** |
| **Efficiency (η)** | `1 / (1 + 2a)` | `W_S / (1 + 2a)` | `W_S / (1 + 2a)` |

*Parameter `a = T_prop / T_trans = (Distance / Speed) / (Frame Size / Bandwidth)`.*

---

### 4. HDLC vs PPP Protocols

| Feature | HDLC Protocol | Point-to-Point Protocol (PPP) |
| :--- | :--- | :--- |
| **Orientation** | Bit-oriented (Flag `01111110`) | Byte-oriented (Flag `0x7E`) |
| **Framing** | Bit stuffing (insert `0` after 5 ones) | Byte stuffing (Escape `0x7D`) |
| **Frame Types** | I-frame (Data), S-frame (RR/REJ), U-frame | LCP packets, NCP packets, Data |
| **Subprotocols** | Monolithic single protocol | **LCP** (Link setup) & **NCP** (IP config) |
| **Authentication** | None built-in | **PAP** (2-way plain) & **CHAP** (3-way MD5) |
| **Modern Form** | Legacy synchronous lines | **PPPoE** (standard broadband DSL/Fiber) |
