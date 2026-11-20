# CN Unit II: Logical Link Control (Data Link Layer) — Complete Solutions & Marking Scheme

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Protocol Notation**: Universal Unicode text symbols.

---

## Section A Solutions (4 Marks Each)

### Q1 Solution: HDLC Bit Stuffing Procedure
**Marking Scheme**:
- Explanation of Bit Stuffing rule: 2 Marks
- Correctly transformed bit stream with stuffed bits indicated: 2 Marks

**Answer**:
1. **Rule of Bit Stuffing**:
   - The HDLC frame delimiter is the FLAG byte `01111110` (six consecutive `1`s bounded by `0`s).
   - To prevent user payload data from accidentally mimicking the FLAG, the sender's data link layer scans the outgoing bit stream. Whenever it detects **five consecutive `1`s**, it automatically inserts (stuffs) a **`0` bit** immediately after the fifth `1`, regardless of the value of the next bit.
   - At the receiver, whenever five consecutive `1`s are followed by a `0`, the receiver removes (unstuffs) the `0` bit to restore the original data.

2. **Step-by-Step Transformation**:
   - Original bit sequence:
     `0 1 1 1 1 1 1 0 1 1 1 1 1 0 0 1 1 1 1 1 0`
   - Evaluation:
     - First group: `0 1 1 1 1 1` (five 1s) ⟹ insert `[0]`, then the next bit `1 0`.
     - Second group: `1 1 1 1 1` (five 1s) ⟹ insert `[0]`, then the next bit `0 0`.
     - Third group: `1 1 1 1 1` (five 1s) ⟹ insert `[0]`, then the next bit `0`.
   - **Transmitted Bit Sequence (with Stuffed Zeros in brackets)**:
     ```
     0 1 1 1 1 1 [0] 1 0 1 1 1 1 1 [0] 0 0 1 1 1 1 1 [0] 0
     ```

---

### Q2 Solution: Hamming Rule & Parity Allocation for 8-Bit ASCII
**Marking Scheme**:
- Hamming inequality formula: 1 Mark
- Calculation of redundant bits `r` for `m = 8`: 1.5 Marks
- Identification of parity bit positions: 1.5 Marks

**Answer**:
1. **Hamming Rule Inequality**:
   ```
   2^r ≥ m + r + 1
   ```
   where `m` is the number of data bits and `r` is the number of redundant parity bits.
2. **Calculation for m = 8 Data Bits**:
   - If `r = 3`: `2³ = 8 ≥ 8 + 3 + 1 = 12` (False: 8 < 12).
   - If `r = 4`: `2⁴ = 16 ≥ 8 + 4 + 1 = 13` (True: 16 ≥ 13).
   - Therefore, **`r = 4` parity bits** are required.
   - **Total Codeword Length**: `n = m + r = 8 + 4 = 12 bits`.
3. **Parity Bit Positions**:
   Parity bits are strictly located at bit positions that are powers of 2:
   - **Position 1**: `P1` (`2⁰`)
   - **Position 2**: `P2` (`2¹`)
   - **Position 4**: `P4` (`2²`)
   - **Position 8**: `P8` (`2³`)
   - Data bits `D1` through `D8` occupy the remaining positions: `3, 5, 6, 7, 9, 10, 11, 12`.

---

### Q3 Solution: Sliding Window Sizes for GBN and Selective Repeat
**Marking Scheme**:
- (a) Go-Back-N window sizes: 2 Marks
- (b) Selective Repeat window sizes & proof of W_S + W_R ≤ 2^m: 2 Marks

**Answer**:
**(a) Go-Back-N (GBN) ARQ**:
- For an `m`-bit sequence number field:
  - **Sender Window Size**: `W_S ≤ 2^m - 1`
  - **Receiver Window Size**: `W_R = 1` (strictly 1).
  *(For m = 3 bits: W_S ≤ 7, W_R = 1).*

**(b) Selective Repeat (SR) ARQ**:
- **Sender Window Size**: `W_S ≤ 2^(m - 1)`
- **Receiver Window Size**: `W_R = W_S ≤ 2^(m - 1)`
  *(For m = 3 bits: W_S ≤ 4, W_R ≤ 4).*
- **Why `W_S + W_R ≤ 2^m`**:
  If the sum of window sizes exceeds `2^m`, the receiver's current receiving window will overlap with the sender's next window cycle.
  - *Example*: If `m = 2` (seq 0, 1, 2, 3) and `W_S = 3, W_R = 3` (sum = 6 > 4): Sender sends frames 0, 1, 2. Receiver accepts all three and advances its window to expect frames 3, 0, 1. But all ACKs are lost in transit!
  - Sender times out and retransmits Frame 0.
  - The receiver, whose window now includes Frame 0 from the *next* cycle, cannot determine whether this Frame 0 is a duplicate retransmission or a brand-new frame!
  - Capping `W_S = W_R = 2^(m - 1)` completely guarantees that consecutive windows never overlap.

---

### Q4 Solution: PAP vs CHAP Authentication in PPP
**Marking Scheme**:
- PAP mechanism and vulnerability: 2 Marks
- CHAP mechanism and security advantages: 2 Marks

**Answer**:

| Feature | PAP (Password Authentication Protocol) | CHAP (Challenge Handshake Authentication Protocol) |
| :--- | :--- | :--- |
| **Handshake Type** | **2-Way Handshake** (Request ➔ Response) | **3-Way Handshake** (Challenge ➔ Response ➔ Success/Failure) |
| **Password Exposure**| Sends username & password in **unencrypted plain text** | Secret password is **NEVER sent over the wire** |
| **Authentication Flow**| 1. Client sends `(User, Password)`.<br>2. Server replies `ACK` or `NAK`. | 1. Server sends a random **Challenge string**.<br>2. Client returns `MD5(Challenge + Password)`.<br>3. Server compares hash; replies `Success` or `Failure`. |
| **Replay Protection**| Vulnerable to eavesdropping and replay attacks | Immune to replay attacks (server generates fresh random challenge every session) |

---

## Section B Solutions (8 Marks Each)

### Q5 Solution: DLL Services to Network Layer & Byte Stuffing
**Marking Scheme**:
- (a) Explanation of the 3 services with examples: 4 Marks
- (b) Byte stuffing mechanism with FLAG and ESC handling: 4 Marks

**Answer**:
**(a) Three Services to Network Layer**:
1. **Unacknowledged Connectionless Service**:
   - Frames are sent independently with no logical connection and no acknowledgments returned. Lost frames are not recovered at the DLL.
   - *Example*: Ethernet (IEEE 802.3), or real-time voice streaming over local subnets.
2. **Acknowledged Connectionless Service**:
   - No logical connection is established, but every individual frame is explicitly acknowledged by the receiver. Frames lost due to noise are retransmitted.
   - *Example*: Wi-Fi (IEEE 802.11 stop-and-wait ACK mechanism over wireless air).
3. **Acknowledged Connection-Oriented Service**:
   - A logical connection is established before data transfer. Frames are sequentially numbered, guaranteeing strictly ordered, lossless, non-duplicate delivery.
   - *Example*: HDLC in Asynchronous Balanced Mode (ABM), legacy X.25.

**(b) Byte Stuffing (Character Stuffing)**:
- Frames are bounded by a special delimiter byte: `FLAG = 0x7E`.
- **Handling Payload Collisions**:
  - If the user data payload contains a byte identical to `FLAG`, the sender inserts an **Escape byte (`ESC = 0x7D`)** immediately before it.
  - If the user data payload contains an accidental `ESC` byte, the sender inserts another `ESC` byte immediately before it (`ESC ESC`).
- **Receiver Processing**:
  - When the receiver encounters an `ESC` byte, it removes (unstuffs) the `ESC` byte and treats the next byte as raw payload data.
- **Example Trace**:
  - Original Data: `A  FLAG  B  ESC  C`
  - Transmitted Stream: `FLAG  A  [ESC] FLAG  B  [ESC] ESC  C  FLAG`
  - At Receiver: Strips framing `FLAG`s; encounters `ESC FLAG` ➔ outputs `FLAG`; encounters `ESC ESC` ➔ outputs `ESC`. Reconstitutes exact original data!

---

### Q6 Solution: Hamming Code Generation & Syndrome Correction (7 Data Bits)
**Marking Scheme**:
- 1. Parity bit count and codeword length calculation: 2 Marks
- 2. Parity bit value calculation and complete codeword: 3 Marks
- 3. Syndrome calculation and bit correction proof: 3 Marks

**Answer**:
Given data bits: `m = 7` bits: `1 0 0 1 1 0 1`.
Data assignment: `D3=1, D5=0, D6=0, D7=1, D9=1, D10=0, D11=1`.

#### 1. Number of Parity Bits:
`2^r ≥ m + r + 1` ⟹ For `m = 7`:
- For `r = 4`: `2⁴ = 16 ≥ 7 + 4 + 1 = 12` (Satisfied!).
- **Number of Parity Bits**: `r = 4` (`P1, P2, P4, P8`).
- **Total Codeword Length**: `n = 7 + 4 = 11 bits`.

#### 2. Determining Parity Bit Values (Even Parity):
Codeword layout:
```
Bit:   1   2   3   4   5   6   7   8   9  10  11
Role: P1  P2  D3  P4  D5  D6  D7  P8  D9 D10 D11
Data:          1       0   0   1       1   0   1
```
- **P1** covers bits with LSB=1: `{1, 3, 5, 7, 9, 11}`:
  `P1 ⊕ D3 ⊕ D5 ⊕ D7 ⊕ D9 ⊕ D11 = 0`
  `P1 ⊕ 1 ⊕ 0 ⊕ 1 ⊕ 1 ⊕ 1 = 0 ⟹ P1 ⊕ 0 = 0 ⟹ P1 = 0`.
- **P2** covers bits with 2nd bit=1: `{2, 3, 6, 7, 10, 11}`:
  `P2 ⊕ D3 ⊕ D6 ⊕ D7 ⊕ D10 ⊕ D11 = 0`
  `P2 ⊕ 1 ⊕ 0 ⊕ 1 ⊕ 0 ⊕ 1 = 0 ⟹ P2 ⊕ 1 = 0 ⟹ P2 = 1`.
- **P4** covers bits with 3rd bit=1: `{4, 5, 6, 7}`:
  `P4 ⊕ D5 ⊕ D6 ⊕ D7 = 0`
  `P4 ⊕ 0 ⊕ 0 ⊕ 1 = 0 ⟹ P4 ⊕ 1 = 0 ⟹ P4 = 1`.
- **P8** covers bits with 4th bit=1: `{8, 9, 10, 11}`:
  `P8 ⊕ D9 ⊕ D10 ⊕ D11 = 0`
  `P8 ⊕ 1 ⊕ 0 ⊕ 1 = 0 ⟹ P8 ⊕ 0 = 0 ⟹ P8 = 0`.

**Complete Transmitted Codeword**:
```
Position:  1  2  3  4  5  6  7  8  9 10 11
Codeword:  0  1  1  1  0  0  1  0  1  0  1
```

#### 3. Syndrome Error Correction:
Suppose **Bit 5** is inverted from `0` to `1`.
Received Codeword: `0  1  1  1  [1]  0  1  0  1  0  1`.
Receiver evaluates the Syndrome `S = (s8 s4 s2 s1)`:
- `s1 = B1 ⊕ B3 ⊕ B5 ⊕ B7 ⊕ B9 ⊕ B11 = 0 ⊕ 1 ⊕ 1 ⊕ 1 ⊕ 1 ⊕ 1 = 1`
- `s2 = B2 ⊕ B3 ⊕ B6 ⊕ B7 ⊕ B10 ⊕ B11 = 1 ⊕ 1 ⊕ 0 ⊕ 1 ⊕ 0 ⊕ 1 = 0`
- `s4 = B4 ⊕ B5 ⊕ B6 ⊕ B7 = 1 ⊕ 1 ⊕ 0 ⊕ 1 = 1`
- `s8 = B8 ⊕ B9 ⊕ B10 ⊕ B11 = 0 ⊕ 1 ⊕ 0 ⊕ 1 = 0`

Syndrome Word:
```
S = (s8 s4 s2 s1) = (0 1 0 1)_2 = 5 in decimal!
```
- The non-zero syndrome directly identifies that **Bit Position 5 is corrupted**.
- The receiver flips Bit 5 (`1 ➔ 0`), perfectly recovering the original error-free codeword!

---

### Q7 Solution: Stop-and-Wait Efficiency Derivation & Satellite Link
**Marking Scheme**:
- (a) Mathematical derivation of efficiency η: 4 Marks
- (b.1) Transmission time calculation: 1 Mark
- (b.2) Parameter a calculation: 1 Mark
- (b.3) Channel utilization and throughput: 2 Marks

**Answer**:
**(a) Efficiency Derivation for Stop-and-Wait ARQ**:
- Let `L` = Frame size in bits, `B` = Bandwidth in bits/sec.
- **Frame Transmission Time**: `T_trans = L / B`.
- **One-Way Propagation Time**: `T_prop`.
- Assuming acknowledgment frame transmission time and processing delays are negligible:
  - Total time to transmit one frame and receive its ACK:
    ```
    T_total = T_trans + 2 · T_prop
    ```
- **Efficiency (Channel Utilization) η**:
  ```
  η = Useful Transmission Time / Total Cycle Time
    = T_trans / (T_trans + 2 · T_prop)
  ```
  Dividing numerator and denominator by `T_trans`, and defining `a = T_prop / T_trans`:
  ```
  η = 1 / (1 + 2 · (T_prop / T_trans)) = 1 / (1 + 2a)
  ```

**(b) Satellite Link Numerical**:
Given:
- Bandwidth `B = 1 Gbps = 10⁹ bits/sec`
- Propagation delay `T_prop = 270 ms = 0.270 sec`
- Frame size `L = 1000 Bytes = 8000 bits`

1. **Transmission Time `T_trans`**:
   ```
   T_trans = L / B = 8000 / 10⁹ = 8 × 10⁻⁶ sec = 0.008 ms
   ```
2. **Parameter `a`**:
   ```
   a = T_prop / T_trans = 270 ms / 0.008 ms = 33,750
   ```
3. **Channel Utilization `η` & Throughput**:
   ```
   η = 1 / (1 + 2a) = 1 / (1 + 2 · 33,750) = 1 / 67,501 ≈ 0.0000148 (or 0.00148%)
   ```
   **Actual Data Throughput**:
   ```
   Throughput = η · Bandwidth = 0.0000148 · 1 Gbps = 14.8 kbps!
   ```
   *Insight*: Stop-and-Wait wastes 99.998% of this satellite link's capacity. Pipelined sliding window protocols with large window sizes are mandatory!

---

### Q8 Solution: Point-to-Point Protocol (PPP) Architecture & PPPoE
**Marking Scheme**:
- 1. PPP frame format drawing and field explanations: 3 Marks
- 2. Role of LCP and NCP: 3 Marks
- 3. PPPoE explanation and modern ISP usage: 2 Marks

**Answer**:
#### 1. PPP Frame Format:
```text
+----------+----------+----------+----------+-----------------------+----------+----------+
|   FLAG   | ADDRESS  | CONTROL  | PROTOCOL |    PAYLOAD (DATA)     |   FCS    |   FLAG   |
| (1 Byte) | (1 Byte) | (1 Byte) |(1 or 2 B)|   (≤ 1500 Bytes)      | (2 or 4B)| (1 Byte) |
|   0x7E   |   0xFF   |   0x03   |          |                       |   CRC    |   0x7E   |
+----------+----------+----------+----------+-----------------------+----------+----------+
```
- **Flag (`0x7E`)**: Delineates frame boundaries.
- **Address (`0xFF`)**: Standard broadcast address (individual stations not assigned addresses).
- **Control (`0x03`)**: Unnumbered information frame command.
- **Protocol**: Identifies the payload protocol (e.g., `0x0021` for IPv4, `0xC021` for LCP, `0xC023` for PAP, `0xC223` for CHAP).
- **Payload**: User datagram; default maximum length is 1500 bytes (MRU).
- **FCS**: 16-bit or 32-bit CRC for error detection.

#### 2. Role of Subprotocols:
1. **LCP (Link Control Protocol)**:
   - Responsible for the lower data link layer management.
   - Negotiates maximum receive unit (MRU), establishes the physical link, selects authentication methods (PAP/CHAP), monitors link quality, and terminates the link when communication finishes.
2. **NCP (Network Control Protocol)**:
   - Responsible for the upper network layer configuration.
   - PPP provides a distinct NCP for each supported network layer protocol (e.g., **IPCP** for IPv4, **IPv6CP** for IPv6).
   - IPCP dynamically negotiates and assigns IP addresses, subnet masks, and DNS servers to the client.

#### 3. PPPoE (PPP over Ethernet) in Modern Broadband:
- Ethernet is a shared broadcast medium that lacks built-in authentication, billing, and session accounting features.
- **PPPoE** encapsulates individual PPP frames inside standard Ethernet frames.
- This allows broadband ISPs (DSL and Fiber-to-the-Home) to enforce user authentication (login username/password), log session connection times, dynamically allocate public IP addresses, and enforce individual bandwidth quotas over standard Ethernet infrastructure.

---

## Section C Solutions (10 Marks Each)

### Q9 Solution: Cyclic Redundancy Check (CRC) Complete Problem
**Marking Scheme**:
- 1. Degree r and zeros appended: 2 Marks
- 2. Step-by-step Modulo-2 division: 4 Marks
- 3. Transmitted codeword: 1 Mark
- 4. Receiver verification (Error-free case & Error-detected case): 3 Marks

**Answer**:
Given: Message `D = 1 0 0 1 0 0` (6 bits).  
Generator: `G(x) = x³ + x² + 1` ⟹ Binary `G = 1 1 0 1` (4 bits).

#### 1. Degree & Appended Zeros:
- Degree of `G(x)` is `r = 3`.
- Number of zeros to append: `r = 3 zeros`.
- Dividend for division: `1 0 0 1 0 0 0 0 0` (9 bits).

#### 2. Modulo-2 Division (XOR Arithmetic):
```text
              1 1 1 1 0 1   (Quotient)
        -----------------
1 1 0 1 ) 1 0 0 1 0 0 0 0 0
        ^ 1 1 0 1
        ---------
          0 1 0 0 0
          ^ 1 1 0 1
          ---------
            0 1 0 1 0
            ^ 1 1 0 1
            ---------
              0 1 1 1 0
              ^ 1 1 0 1
              ---------
                0 0 1 1 0
                ^ 0 0 0 0
                ---------
                  0 1 1 0 0
                  ^ 1 1 0 1
                  ---------
                    0 0 0 1   <-- Remainder R = 0 0 1 (3 bits)
```
- The CRC remainder is **`R = 0 0 1`**.

#### 3. Complete Transmitted Codeword:
```
T = Data + Remainder = 1 0 0 1 0 0 [ 0 0 1 ]
```

#### 4. Receiver Modulo-2 Verification:

**Case A: Error-Free Reception (`T = 1 0 0 1 0 0 0 0 1`)**:
```text
1 1 0 1 ) 1 0 0 1 0 0 0 0 1
        ^ 1 1 0 1
        ---------
          0 1 0 0 0
          ^ 1 1 0 1
          ---------
            0 1 0 1 0
            ^ 1 1 0 1
            ---------
              0 1 1 1 0
              ^ 1 1 0 1
              ---------
                0 0 1 1 0
                ^ 0 0 0 0
                ---------
                  0 1 1 0 1
                  ^ 1 1 0 1
                  ---------
                    0 0 0 0   <-- Remainder is ZERO!
```
- Remainder is `000` ⟹ **Frame is accepted as error-free**.

**Case B: Inverting the 3rd bit (`T' = 1 0 [1] 1 0 0 0 0 1`)**:
```text
1 1 0 1 ) 1 0 1 1 0 0 0 0 1
        ^ 1 1 0 1
        ---------
          0 1 1 0 0
          ^ 1 1 0 1
          ---------
            0 0 0 1 0
            ^ 0 0 0 0
            ---------
              0 0 1 0 0
              ^ 0 0 0 0
              ---------
                0 1 0 0 0
                ^ 1 1 0 1
                ---------
                  0 1 0 1 1
                  ^ 1 1 0 1
                  ---------
                    0 1 1 0   <-- Remainder R = 110 (NON-ZERO!)
```
- Remainder `110 ≠ 000` ⟹ **Transmission error detected; frame discarded!**

---

### Q10 Solution: Go-Back-N vs Selective Repeat Timeline Simulation
**Marking Scheme**:
- 1. Maximum window size calculation: 2 Marks
- 2. Go-Back-N timeline diagram and explanation: 4 Marks
- 3. Selective Repeat timeline diagram and explanation: 4 Marks

**Answer**:
Given sequence number bits: `m = 3` (Sequence numbers: `0, 1, 2, 3, 4, 5, 6, 7`).
Frames transmitted: `0, 1, 2, 3, 4, 5`. **Frame 2 is lost in transit**.

#### 1. Maximum Window Sizes:
- **Go-Back-N (GBN)**: `W_S = 2^m - 1 = 2³ - 1 = 7`. (Here sender window = 4 or 5).
- **Selective Repeat (SR)**: `W_S = 2^(m - 1) = 2² = 4`.

---

#### 2. Go-Back-N (GBN) Timeline Simulation:
- **Window Size**: `W_S = 4`, `W_R = 1`.

```text
SENDER (W_S = 4)                                   RECEIVER (W_R = 1)
   |                                                  |
   | ----------- Frame 0 ---------------------------> | [Accepts Frame 0]
   | ----------- Frame 1 ---------------------------> | [Accepts Frame 1]
   | ----------- Frame 2 (LOST X)                     | (Never arrives!)
   | ----------- Frame 3 ---------------------------> | [DISCARDS 3: Expected 2!]
   |                                                  |
   | <---------- ACK 1 (Cumulative for 0 & 1) ------- |
   |                                                  |
   | ----------- Frame 4 ---------------------------> | [DISCARDS 4: Expected 2!]
   | ----------- Frame 5 ---------------------------> | [DISCARDS 5: Expected 2!]
   |                                                  |
   | [Timer for Frame 2 EXPIRES!]                     |
   |                                                  |
   | === GO-BACK-N RETRANSMISSION (Frames 2, 3, 4, 5) |
   | ----------- Frame 2 ---------------------------> | [Accepts Frame 2]
   | ----------- Frame 3 ---------------------------> | [Accepts Frame 3]
   | ----------- Frame 4 ---------------------------> | [Accepts Frame 4]
   | ----------- Frame 5 ---------------------------> | [Accepts Frame 5]
   | <---------- ACK 6 (Cumulative) ----------------- |
```
- *Key Behavior*: Receiver buffer is strictly 1. Even though Frames 3, 4, and 5 arrived intact, the receiver discarded them all because Frame 2 was missing. The sender had to go back and retransmit **all 4 frames**.

---

#### 3. Selective Repeat (SR) Timeline Simulation:
- **Window Size**: `W_S = 4`, `W_R = 4`.

```text
SENDER (W_S = 4)                                   RECEIVER (W_R = 4)
   |                                                  |
   | ----------- Frame 0 ---------------------------> | [Accepts & Delivers 0]
   | ----------- Frame 1 ---------------------------> | [Accepts & Delivers 1]
   | ----------- Frame 2 (LOST X)                     | (Never arrives!)
   | ----------- Frame 3 ---------------------------> | [BUFFERS Frame 3!]
   |                                                  |
   | <---------- ACK 0 ------------------------------ |
   | <---------- ACK 1 ------------------------------ |
   | <---------- NAK 2 (Requests Frame 2) ----------- |
   |                                                  |
   | [Sender receives NAK 2]                          |
   | === SELECTIVE RETRANSMISSION (ONLY Frame 2) ===  |
   | ----------- Frame 2 (Retransmit ONLY 2) -------> | [Accepts Frame 2]
   |                                                  | [Delivers Frames 2 & 3!]
   | <---------- ACK 3 ------------------------------ |
   |                                                  |
   | ----------- Frame 4 ---------------------------> | [Accepts & Delivers 4]
   | ----------- Frame 5 ---------------------------> | [Accepts & Delivers 5]
   | <---------- ACK 5 ------------------------------ |
```
- *Key Behavior*: The receiver maintains an internal buffer. It safely holds Frame 3 and sends a NAK 2. The sender retransmits **ONLY Frame 2**. Bandwidth is preserved!

---

### Q11 Solution: Sliding Window Design & Bandwidth-Delay Product
**Marking Scheme**:
- 1. Propagation time and transmission time: 2 Marks
- 2. Bandwidth-Delay Product (bits and frames): 3 Marks
- 3. Stop-and-Wait efficiency: 2 Marks
- 4. Minimum sequence bits for 100% efficiency (GBN vs SR): 3 Marks

**Answer**:
Given:
- Distance `d = 100 km = 10⁵ m`
- Bandwidth `B = 100 Mbps = 10⁸ bits/sec`
- Propagation speed `v = 2 × 10⁸ m/s`
- Frame size `L = 1000 Bytes = 8000 bits`

#### 1. Propagation & Transmission Time:
- **One-Way Propagation Delay**:
  ```
  T_prop = d / v = 10⁵ / (2 × 10⁸) = 0.5 × 10⁻³ sec = 0.5 ms
  ```
- **Transmission Time**:
  ```
  T_trans = L / B = 8000 / 10⁸ = 8 × 10⁻⁵ sec = 0.08 ms
  ```

#### 2. Bandwidth-Delay Product (BDP):
- **BDP in Bits**:
  ```
  BDP = Bandwidth · Round-Trip Time (RTT) = B · (2 · T_prop)
      = 10⁸ bits/sec · (2 · 0.5 × 10⁻³ sec) = 10⁸ · 10⁻³ = 100,000 bits
  ```
- **BDP in Frames (Pipelined Capacity)**:
  ```
  BDP_frames = BDP / Frame Size = 100,000 bits / 8000 bits = 12.5 frames
  ```

#### 3. Stop-and-Wait Channel Utilization:
- Parameter `a`:
  ```
  a = T_prop / T_trans = 0.5 ms / 0.08 ms = 6.25
  ```
- Efficiency `η`:
  ```
  η = 1 / (1 + 2a) = 1 / (1 + 2 · 6.25) = 1 / 13.5 ≈ 0.074 (7.4%)
  ```

#### 4. Minimum Sequence Number Bits for 100% Utilization:
To achieve 100% utilization (`η = 1.0`), the window size must cover the entire round-trip pipeline:
```
W_optimal ≥ 1 + 2a = 1 + 2(6.25) = 13.5 ⟹ W_optimal = 14 frames
```
- **For Go-Back-N ARQ**:
  `W_S ≤ 2^m - 1`
  `2^m - 1 ≥ 14 ⟹ 2^m ≥ 15`
  For `m = 3`: `2³ = 8 < 15`
  For `m = 4`: `2⁴ = 16 ≥ 15`
  ⟹ **`m = 4` bits required for Go-Back-N**.
- **For Selective Repeat ARQ**:
  `W_S ≤ 2^(m - 1)`
  `2^(m - 1) ≥ 14`
  For `m = 4`: `2³ = 8 < 14`
  For `m = 5`: `2⁴ = 16 ≥ 14`
  ⟹ **`m = 5` bits required for Selective Repeat**.

---

### Q12 Solution: HDLC Protocol Deep Dive
**Marking Scheme**:
- (a) HDLC frame format drawing and field explanations: 3 Marks
- (b.1) I-Frame structure, N(S), N(R), and piggybacking: 2 Marks
- (b.2) S-Frame commands (RR, RNR, REJ, SREJ): 3 Marks
- (b.3) U-Frame commands (SABM, DISC): 2 Marks

**Answer**:
**(a) HDLC Frame Format**:
```text
+----------+----------+----------+-----------------------+----------+----------+
|   FLAG   | ADDRESS  | CONTROL  |  INFORMATION (DATA)   |   FCS    |   FLAG   |
| (8 bits) | (8 bits) | (8 bits) |   (Variable Length)   | (16 bits)| (8 bits) |
| 01111110 |          |          |                       |   CRC    | 01111110 |
+----------+----------+----------+-----------------------+----------+----------+
```
- **Flag (`01111110`)**: Synchronizes sender and receiver and delineates frame boundaries using bit stuffing.
- **Address**: Identifies the secondary station in multipoint configurations.
- **Control**: Identifies the frame type (I, S, or U) and carries sequence numbers.
- **Information**: Contains user payload data from the Network Layer.
- **FCS**: 16-bit CRC checksum calculated over Address, Control, and Information fields.

**(b) Control Field Architecture for the Three Frame Types**:

1. **Information Frames (I-Frames)**:
   - Format: `[ 0 | N(S) (3 bits) | P/F (1 bit) | N(R) (3 bits) ]`
   - Begins with a leading `0`.
   - `N(S)`: Sequence number of the outgoing data frame being sent.
   - `N(R)`: Acknowledgment sequence number of the next frame the station expects to receive.
   - **Piggybacking**: When station A sends data to station B, it bundles its acknowledgment of B's previous data inside the `N(R)` field of its own outgoing I-frame, eliminating the overhead of dedicated ACK packets.

2. **Supervisory Frames (S-Frames)**:
   - Format: `[ 1 0 | Type (2 bits) | P/F (1 bit) | N(R) (3 bits) ]`
   - Begins with `1 0`. Carries no user information payload.
   - **Commands (determined by 2-bit Type field)**:
     - `Type 00 (RR - Receive Ready)`: Positive acknowledgment; confirms receipt of frames up to `N(R) - 1` and signals readiness to receive frame `N(R)`.
     - `Type 10 (RNR - Receive Not Ready)`: Acknowledges frames up to `N(R) - 1`, but temporarily halts sender transmission due to receiver buffer congestion.
     - `Type 01 (REJ - Reject)`: Negative acknowledgment for Go-Back-N ARQ; requests retransmission of all frames starting from `N(R)`.
     - `Type 11 (SREJ - Selective Reject)`: Negative acknowledgment for Selective Repeat ARQ; requests retransmission of *only* frame `N(R)`.

3. **Unnumbered Frames (U-Frames)**:
   - Format: `[ 1 1 | Modifier bits (5 bits) | P/F (1 bit) ]`
   - Begins with `1 1`. Used for link initialization, mode negotiation, and link teardown.
   - **Key Commands**:
     - `SABM (Set Asynchronous Balanced Mode)`: Initializes a full-duplex point-to-point connection between peer stations.
     - `DISC (Disconnect)`: Terminates the logical connection.
     - `UA (Unnumbered Acknowledgment)`: Confirms acceptance of SABM or DISC commands.
