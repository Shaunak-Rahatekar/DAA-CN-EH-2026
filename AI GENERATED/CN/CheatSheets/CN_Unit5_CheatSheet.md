# CN Unit V: Transport Layer — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: Sockets API, TCP vs UDP, Handshake & Teardown, Sliding Window Flow Control, Timer Management, AIMD Congestion Control, and QUIC Protocol

---

### 1. TCP vs UDP Quick Matrix

| Parameter | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Connection Model** | Connection-Oriented (3-way handshake) | Connectionless (0-RTT setup) |
| **Stream Type** | **Byte-Stream Oriented** (no boundaries) | **Message-Oriented** (preserves boundaries) |
| **Reliability** | Guaranteed (ACKs, retransmissions, ordering) | Best-Effort (loss/corruption possible) |
| **Header Size** | **20 to 60 Bytes** (Variable, options) | **Fixed 8 Bytes** (Source/Dest Port, Len, Checksum)|
| **Flow & Congestion**| Yes (`rwnd` & `cwnd` AIMD) | None (transmits blindly at application rate) |
| **Demultiplexing** | Full 4-Tuple `(SrcIP, SrcPort, DstIP, DstPort)` | 2-Tuple `(DstIP, DstPort)` |

---

### 2. Berkeley Sockets Lifecycle
- **Server Sequence**: `socket() ➔ bind() ➔ listen() ➔ accept() [blocks] ➔ recv()/read() ➔ send()/write() ➔ close()`.
  - `accept()` returns a **new socket descriptor** dedicated to the accepted client connection.
- **Client Sequence**: `socket() ➔ connect() [triggers 3-way handshake] ➔ send()/write() ➔ recv()/read() ➔ close()`.

---

### 3. TCP Connection Management
- **3-Way Handshake**:
  1. `Client ➔ Server`: `SYN=1, Seq=x` (Client ISN).
  2. `Server ➔ Client`: `SYN=1, ACK=1, Seq=y, Ack=x+1` (Server ISN).
  3. `Client ➔ Server`: `ACK=1, Seq=x+1, Ack=y+1` (May carry payload).
  - *Why not 2-way?* Prevents old delayed duplicate SYNs from establishing half-open zombie connections on server.
- **4-Way Teardown**: `Client FIN ➔ Server ACK` (Server enters `CLOSE_WAIT`), then `Server FIN ➔ Client ACK` (Client enters `TIME_WAIT` for `2*MSL`).
  - *Why TIME_WAIT = 2*MSL?* Ensures final ACK reaches server and flushes wandering duplicate segments.

---

### 4. Sliding Window Flow Control & Silly Window Syndrome (SWS)
- **Invariant**: `LastByteSent - LastByteAcked ≤ min(cwnd, rwnd)`.
- **Silly Window Syndrome (SWS)**: Tiny payload with massive 40B header (2.4% efficiency).
  - **Nagle's Algorithm (Sender)**: Send 1st byte immediately; buffer remaining tiny data until full MSS OR previous ACK received.
  - **Clark's Solution (Receiver)**: Advertise `rwnd = 0` until free buffer `≥ min(MSS, Buffer_Total / 2)`.

---

### 5. TCP Timer Management Formulas
- **RTO (Jacobson's Algorithm)**:
  - `RTT_s = (1 - α) · RTT_s + α · RTT_m` (`α = 0.125`)
  - `D = (1 - β) · D + β · |RTT_s - RTT_m|` (`β = 0.25`)
  - **`RTO = RTT_s + 4 · D`**
- **Karn's Rule**: Never measure RTT for retransmitted segments. Double timer on timeout: `RTO_new = 2 · RTO_old`.
- **Persistence Timer**: Prevents deadlock when `rwnd = 0` update ACK is lost (sends 1-byte probe).
- **Keep-Alive Timer**: Detects crashed peers after 2 hours of idle silence.

---

### 6. Congestion Control (AIMD) & QUIC Protocol
- **Slow Start**: `cwnd < ssthresh` ⟹ For every ACK, `cwnd = cwnd + 1 MSS` (**doubles every RTT**).
- **Congestion Avoidance**: `cwnd ≥ ssthresh` ⟹ For every RTT, `cwnd = cwnd + 1 MSS` (**Additive Increase**).
- **Timeout (Severe Loss)**: `ssthresh = max(cwnd / 2, 2 MSS)`, **`cwnd = 1 MSS`** (resumes Slow Start).
- **3 Duplicate ACKs (Mild Loss)**: `ssthresh = max(cwnd / 2, 2 MSS)`.
  - **TCP Tahoe**: Drops `cwnd = 1 MSS` (resumes Slow Start).
  - **TCP Reno (Fast Recovery)**: Sets `cwnd = ssthresh`, enters Congestion Avoidance directly!
- **QUIC (HTTP/3 over UDP)**: 0-RTT handshake (built-in TLS 1.3), no Head-of-Line blocking, connection migration via 64-bit CID.
