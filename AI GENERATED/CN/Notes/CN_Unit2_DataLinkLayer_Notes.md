# CN Unit II: Logical Link Control (Data Link Layer) — Complete Exam Notes

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Protocol Notation**: Universal Unicode text symbols (no raw LaTeX tags).

---

## 1. Data Link Layer Design Issues & Services to Network Layer

### 1.1 Role of the Data Link Layer (DLL)
The Data Link Layer transforms a raw, error-prone physical transmission facility into a reliable, error-free communication link for the Network Layer. It is subdivided into two distinct sublayers:
1. **LLC (Logical Link Control)**: Manages framing, error control, and flow control; provides a uniform interface to the network layer independent of the underlying physical transmission medium.
2. **MAC (Medium Access Control)**: Coordinates access to shared multi-access media (e.g., CSMA/CD in Ethernet, CSMA/CA in Wi-Fi).

### 1.2 The Three Types of Services Provided to the Network Layer
1. **Unacknowledged Connectionless Service**:
   - The source sends independent frames without establishing a logical connection, and the destination does not return acknowledgments.
   - Lost or damaged frames are not detected or recovered at the DLL; recovery is left to higher layers (e.g., TCP).
   - *Use Case*: Real-time traffic (VoIP, streaming video) where speed is critical and delays are intolerable, or over highly reliable physical channels (fiber optics, short copper runs).
   - *Example*: Ethernet (IEEE 802.3).

2. **Acknowledged Connectionless Service**:
   - No logical connection is established beforehand, but every individually transmitted frame is explicitly acknowledged by the receiver.
   - If an acknowledgment is not received within a specified timeout, the frame is retransmitted.
   - *Use Case*: Unreliable physical media prone to high burst error rates where establishing a full connection has too much overhead.
   - *Example*: Wi-Fi (IEEE 802.11 stop-and-wait ACK mechanism over wireless air).

3. **Acknowledged Connection-Oriented Service**:
   - A logical connection is explicitly established before any data frames are transmitted.
   - Frames are assigned sequential sequence numbers, ensuring that every frame is delivered **exactly once**, in the **correct sequential order**, without loss or duplication.
   - Operates in three distinct phases: Connection Establishment, Reliable Data Transfer, and Connection Release.
   - *Example*: HDLC in Asynchronous Balanced Mode (ABM), legacy X.25.

---

## 2. Framing Techniques

Since the physical layer merely transmits an unstructured stream of bits, the Data Link Layer must break this raw bit stream into discrete, identifiable units called **frames**.

### 2.1 Character Count / Byte Count Method
- The frame header contains an integer field specifying the total number of bytes in the frame (including the count byte itself).
- *Fatal Flaw (Synchronization Disaster)*: If transmission noise corrupts the count field (e.g., a count of 5 becomes a 7), the receiver counts the wrong number of bytes, falls completely out of synchronization, and cannot determine where the next frame begins. Consequently, this method is rarely used alone.

### 2.2 Byte Stuffing (Character-Oriented Framing)
- Special **FLAG bytes** (e.g., `0x7E` or `DLE STX` / `DLE ETX`) are placed at the beginning and end of each frame to delineate frame boundaries.
- **The Problem**: If the user payload data itself happens to contain the FLAG byte pattern (e.g., in binary files, images, compiled code), the receiver will prematurely interpret it as the end of the frame.
- **The Solution (Byte Stuffing)**:
  - The sender inserts an **Escape byte (ESC, e.g., `0x7D`)** immediately before any accidental FLAG byte or ESC byte occurring inside the data payload.
  - The receiver scans the incoming stream: whenever it encounters an ESC byte, it deletes (unstuffs) the ESC byte and treats the immediately following byte as literal data, not a delimiter.
  - *Example*:
    - Original Data: `A  FLAG  B  ESC  C`
    - Transmitted Frame: `FLAG  A  [ESC] FLAG  B  [ESC] ESC  C  FLAG`
  - *Standard*: Used in **PPP (Point-to-Point Protocol)**.

### 2.3 Bit Stuffing (Bit-Oriented Framing)
- Frames begin and end with an identical 8-bit special pattern called a **FLAG sequence**:
  ```
  FLAG = 0 1 1 1 1 1 1 0   (0, followed by six consecutive 1s, followed by 0)
  ```
- **The Rule of Bit Stuffing**:
  - **At the Sender**: Whenever the sender's data link layer detects **five consecutive `1`s** in the user data stream, it **automatically inserts (stuffs) a `0` bit** immediately after the fifth `1`, regardless of whether the next bit was a `0` or a `1`.
  - **At the Receiver**: Whenever the receiver observes five consecutive `1`s followed by a `0` bit, it **automatically removes (unstuffs) the `0` bit** and restores the original data.
  - If the receiver encounters five `1`s followed by a `1` and then a `0` (`01111110`), it recognizes it as a true **FLAG delimiter**.
  - If the receiver encounters seven consecutive `1`s, it flags a physical channel error.
- *Advantage*: Completely transparent to the user data; operates on arbitrary bit streams without requiring 8-bit byte alignment.
- *Standard*: Used in **HDLC (High-Level Data Link Control)**.

---

## 3. Error Control: Detection and Correction

Transmission media are susceptible to physical impairments (thermal noise, electromagnetic interference, signal attenuation), causing bit inversions:
- **Single-Bit Error**: Only one bit in a given data unit is corrupted.
- **Burst Error**: Two or more consecutive bits are corrupted.

### 3.1 Parity Checks
1. **Simple Parity (1D)**: Appends a single parity bit to ensure the total number of `1`s is even (Even Parity) or odd (Odd Parity).
   - Detects all **odd numbers** of bit errors.
   - Completely fails to detect even numbers of bit errors (e.g., a 2-bit error passes undetected).
2. **Two-Dimensional (2D) Parity**: Arranges data into a 2D matrix (rows and columns) and computes parity for each row and each column.
   - Detects all 1-bit, 2-bit, and 3-bit errors.
   - Can **correct single-bit errors** (the intersection of the corrupted row and corrupted column pinpoints the exact erroneous bit).

---

### 3.2 Hamming Code (Forward Error Correction)
Invented by Richard Hamming, this code uses redundant parity bits embedded at specific positions to both **detect and correct single-bit errors**.

#### 1. Number of Redundant Bits (The Hamming Rule):
If `m` is the number of data bits and `r` is the number of redundant parity bits, `r` must satisfy the inequality:
```
2^r ≥ m + r + 1
```
*(For m = 4 data bits: 2³ = 8 ≥ 4 + 3 + 1 = 8 ⟹ r = 3 parity bits, total codeword = 7 bits: Hamming(7, 4)).*  
*(For m = 7 data bits: 2⁴ = 16 ≥ 7 + 4 + 1 = 12 ⟹ r = 4 parity bits, total codeword = 11 bits: Hamming(11, 7)).*

#### 2. Bit Placement:
Parity bits `P1, P2, P4, P8, ...` are placed strictly at positions that are **powers of 2**:
```
Position:  1   2   3   4   5   6   7
Role:     P1  P2  D3  P4  D5  D6  D7
Binary:  001 010 011 100 101 110 111
```

#### 3. Parity Bit Coverage (Even Parity):
Each parity bit checks all bit positions whose binary representation has a `1` in that parity bit's position:
- **P1 (bit 1, binary `..1`)**: Checks bits with least significant bit = 1: **Bits 1, 3, 5, 7**.
  `P1 ⊕ D3 ⊕ D5 ⊕ D7 = 0`
- **P2 (bit 2, binary `.1.`)**: Checks bits with 2nd bit = 1: **Bits 2, 3, 6, 7**.
  `P2 ⊕ D3 ⊕ D6 ⊕ D7 = 0`
- **P4 (bit 4, binary `1..`)**: Checks bits with 3rd bit = 1: **Bits 4, 5, 6, 7**.
  `P4 ⊕ D5 ⊕ D6 ⊕ D7 = 0`

#### 4. Error Detection & Syndrome Decoding:
At the receiver, the parity check equations are evaluated to form the **Syndrome Word** `S = (s4 s2 s1)`:
- `s1 = Bit 1 ⊕ Bit 3 ⊕ Bit 5 ⊕ Bit 7`
- `s2 = Bit 2 ⊕ Bit 3 ⊕ Bit 6 ⊕ Bit 7`
- `s4 = Bit 4 ⊕ Bit 5 ⊕ Bit 6 ⊕ Bit 7`
If `S = 000`, no error occurred.
If `S ≠ 000`, the decimal value of `S` indicates the **exact corrupted bit position**! Simply flipping that bit corrects the error.

#### 5. Hamming Distance & Error Bounds:
- The **Hamming Distance** `d(x, y)` between two codewords is the number of bit positions in which they differ.
- For a code with minimum Hamming distance `d_min`:
  - To **detect `s` errors**: `d_min ≥ s + 1`
  - To **correct `t` errors**: `d_min ≥ 2t + 1`

---

### 3.3 Cyclic Redundancy Check (CRC)
CRC is a powerful, hardware-friendly polynomial code used in Ethernet, Wi-Fi, and storage devices.

#### 1. Mathematical Principle:
Data and codewords are treated as polynomials with binary coefficients modulo-2 (where addition and subtraction are identically bitwise **XOR** without carries or borrows).
Let:
- Data polynomial: `D(x)` of length `k`.
- Generator polynomial: `G(x)` of degree `r` (length `r + 1`).
- CRC remainder: `R(x)` of degree `r - 1` (length `r`).

#### 2. Sender Algorithm:
1. Append `r` zeros to the original data stream (representing `D(x) · x^r`).
2. Divide this extended bit sequence by the generator `G(x)` using binary modulo-2 division.
3. The resulting remainder of length `r` is the **CRC Checksum**.
4. The transmitted frame is:
   ```
   Transmitted Frame T(x) = D(x) · x^r ⊕ R(x)
   ```
   *(The appended zeros are replaced by the remainder).*

#### 3. Receiver Algorithm:
1. Divide the received frame `T'(x)` by the identical generator polynomial `G(x)` using modulo-2 division.
2. **If Remainder == 0**: The frame is accepted as error-free.
3. **If Remainder ≠ 0**: A transmission error occurred; the frame is discarded.

#### 4. Error Detection Capabilities of CRC:
A properly designed generator polynomial `G(x)` of degree `r` guarantees:
1. Detection of **all single-bit errors** (if `G(x)` has at least two terms).
2. Detection of **all double-bit errors** (if `G(x)` does not divide `x^k + 1`).
3. Detection of **all odd numbers of errors** (if `(x + 1)` is a factor of `G(x)`).
4. Detection of **all burst errors of length ≤ r**.
5. Detection of burst errors of length `r + 1` with probability `1 - (1/2)^(r-1)`.

---

## 4. Flow Control & Sliding Window Protocols

**Flow Control** is a set of procedures that prevents a fast sender from overwhelming a slow receiver with data before the receiver can process and buffer it.

### 4.1 Stop-and-Wait ARQ
- **Rule**: The sender transmits a single frame and stops. It starts a retransmission timer. It waits until an acknowledgment (ACK) is received before sending the next frame.
- **Formulas**:
  - Transmission time: `T_trans = Frame Size / Bandwidth`
  - Propagation time: `T_prop = Distance / Propagation Speed`
  - Total cycle time: `T_total = T_trans + 2 · T_prop`
  - Parameter `a`: `a = T_prop / T_trans`
  - **Efficiency (Channel Utilization)**:
    ```
    η = T_trans / (T_trans + 2 · T_prop) = 1 / (1 + 2a)
    ```
- *Limitation*: If `T_prop` is large (e.g., satellite links where `a >> 1`), the sender spends 99% of its time idling, resulting in abysmal channel efficiency.

---

### 4.2 Sliding Window Protocols (Pipelined Transmission)
To maximize channel utilization, the sender is permitted to transmit multiple frames before waiting for an acknowledgment. The outstanding unacknowledged frames are held in a **Sliding Window**.

### 4.3 Go-Back-N (GBN) ARQ
- **Sender Window Size**: `W_S ≤ 2^m - 1` (where `m` is the number of bits in the sequence number field).
  *(If m = 3 bits, sequence numbers are 0 to 7, and max sender window W_S = 7).*
- **Receiver Window Size**: `W_R = 1` (strictly 1).
- **Receiver Behavior**:
  - The receiver accepts frames **strictly in sequential order**.
  - If a frame arrives out of order (e.g., Frame 2 is lost and Frame 3 arrives), the receiver **silently discards Frame 3** and re-sends an ACK for the last correctly received frame.
  - Uses **Cumulative ACKs**: `ACK n` acknowledges that all frames up to `n - 1` have been received safely, and the receiver expects Frame `n`.
- **Sender Behavior on Timeout**:
  - The sender maintains a single timer for the oldest unacknowledged frame.
  - If the timer expires, the sender must **go back and retransmit ALL frames** currently in the window starting from the lost frame!
- **Efficiency**:
  ```
  η = min( 1, W_S / (1 + 2a) )
  ```

---

### 4.4 Selective Repeat (SR) ARQ
- **Sender Window Size**: `W_S ≤ 2^(m - 1)`.
- **Receiver Window Size**: `W_R = W_S ≤ 2^(m - 1)`.
  *(For m = 3 bits: W_S = 4 and W_R = 4).*
- **Why Window Size is Capped at 2^(m-1)**:
  If `W_S + W_R > 2^m`, the receiver's window would overlap with the sender's next window cycle, causing the receiver to misidentify retransmitted frames as brand-new frames!
- **Receiver Behavior**:
  - The receiver maintains a buffer of size `W_R`.
  - It accepts and buffers valid out-of-order frames within its window.
  - Sends **Negative Acknowledgments (NAK)** or **Selective ACKs** for specific missing frames.
- **Sender Behavior on Timeout**:
  - The sender maintains an individual timer for *each* transmitted frame.
  - On timeout, the sender retransmits **ONLY the specific lost or damaged frame**, avoiding redundant retransmissions.
- **Efficiency**:
  ```
  η = min( 1, W_S / (1 + 2a) )
  ```
  *(Same theoretical efficiency as GBN, but achieves vastly superior practical throughput on error-prone channels).*

---

### 4.5 Comparison Table: Stop-and-Wait vs Go-Back-N vs Selective Repeat

| Parameter | Stop-and-Wait ARQ | Go-Back-N ARQ | Selective Repeat ARQ |
| :--- | :--- | :--- | :--- |
| **Sender Window (W_S)** | `1` | `≤ 2^m - 1` | `≤ 2^(m - 1)` |
| **Receiver Window (W_R)**| `1` | `1` (strictly) | `W_R = W_S ≤ 2^(m - 1)` |
| **Receiver Buffering** | None | None (discards out-of-order) | Buffers out-of-order frames |
| **Acknowledgment Type** | Individual ACK | **Cumulative ACK** | **Individual / Selective ACK** |
| **Retransmission Scope** | 1 frame | **All frames in window** | **Only the lost frame** |
| **Hardware Complexity** | Minimal | Low | High (timers & buffer logic) |
| **Bandwidth Efficiency**| `1 / (1 + 2a)` (Very Low) | `W_S / (1 + 2a)` | `W_S / (1 + 2a)` |

---

## 5. Data Link Protocols for WAN: HDLC and PPP

### 5.1 High-Level Data Link Control (HDLC)
HDLC is an ISO-standard, bit-oriented protocol operating over point-to-point and multipoint links.

#### 1. HDLC Frame Format:
```text
+----------+----------+----------+-----------------------+----------+----------+
|   FLAG   | ADDRESS  | CONTROL  |  INFORMATION (DATA)   |   FCS    |   FLAG   |
| (8 bits) | (8 bits) |(8/16 bit)|   (Variable Length)   |(16/32 bit| (8 bits) |
| 01111110 |          |          |                       |   CRC    | 01111110 |
+----------+----------+----------+-----------------------+----------+----------+
```

#### 2. The Three HDLC Frame Types (Defined by Control Field):
1. **Information Frames (I-Frames)**:
   - Carry user data from the network layer.
   - Include flow control sequence numbers: `N(S)` (send sequence number) and `N(R)` (receive sequence number / piggybacked ACK).
2. **Supervisory Frames (S-Frames)**:
   - Carry flow and error control information when there is no user data to piggyback.
   - Commands:
     - `RR` (Receive Ready): Acknowledges frames up to `N(R) - 1` and indicates readiness to receive.
     - `RNR` (Receive Not Ready): Acknowledges frames but halts transmission due to temporary buffer exhaustion.
     - `REJ` (Reject): Requests Go-Back-N retransmission starting from `N(R)`.
     - `SREJ` (Selective Reject): Requests Selective Repeat retransmission of frame `N(R)`.
3. **Unnumbered Frames (U-Frames)**:
   - Used for link management, mode setting, and connection initialization/termination.
   - Commands: `SABM` (Set Asynchronous Balanced Mode), `DISC` (Disconnect), `UA` (Unnumbered Acknowledgment).

---

### 5.2 Point-to-Point Protocol (PPP)
PPP is the industry-standard, byte-oriented protocol used for establishing direct connections between two nodes (e.g., home PC to ISP over broadband).

#### 1. PPP Architectural Components:
1. **HDLC-like Framing**: Encapsulates datagrams using byte stuffing with Flag `0x7E`.
2. **LCP (Link Control Protocol)**: Establishes, configures, tests, negotiates options (e.g., MRU size), and terminates the data link connection.
3. **NCP (Network Control Protocol)**: A family of protocols (one per network layer protocol, e.g., **IPCP** for IPv4, **IPv6CP** for IPv6) that negotiates network-layer addresses and configurations over the established link.

#### 2. Authentication in PPP:
- **PAP (Password Authentication Protocol)**:
  - Simple **2-way handshake**.
  - Client sends username and password in **unencrypted plain text** over the wire. Highly insecure.
- **CHAP (Challenge Handshake Authentication Protocol)**:
  - Secure **3-way handshake** using cryptographic hashing.
  - 1. Server sends a random **Challenge string** to the client.
  - 2. Client computes `Response = MD5(Challenge + Secret Password)` and returns the hash.
  - 3. Server performs the identical hash calculation; if values match, access is granted.
  - The secret password is **never sent over the wire**!

#### 3. Modern WAN Questions & Broadband Variants:
- **PPPoE (PPP over Ethernet)**: Encapsulates PPP frames inside Ethernet frames. Ubiquitously used by DSL and fiber broadband ISPs to authenticate subscribers via username/password and assign public IP addresses.
- **Is DOCSIS used today?**: **YES**. Data Over Cable Service Interface Specification (DOCSIS 3.1 and DOCSIS 4.0) powers high-speed cable Internet worldwide, delivering multi-gigabit speeds over hybrid fiber-coaxial (HFC) networks.
- **Is DSL used today?**: DSL is actively being **phased out** in favor of Fiber-to-the-Home (FTTH), though VDSL2 is still maintained in regions where fiber deployment is incomplete.
- **Is PPP still used?**: **YES**, predominantly through **PPPoE** in residential broadband access networks.
