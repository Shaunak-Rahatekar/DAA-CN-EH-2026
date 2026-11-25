# Computer Networks (CS3213 / IT2266 / CS3025)
# Complete Master Formula & High-Yield Numerical Problem Sheet

> **Scope**: All 6 Units of the Computer Networks Syllabus (VIT Pune / SPPU pattern).  
> **Structure**:  
> 1. **Part 1**: Unit-wise and Topic-wise Master Formula Quick Reference with exact units and variable definitions.  
> 2. **Part 2**: Complete Worked Numerical Bank -- providing a high-level exam-grade problem, step-by-step substitution, engineering intuition, and common examiner traps for every single formula.  
> **Formatting**: 100% clean, human-readable universal formatting with no unreadable symbols.

---

# PART 1: MASTER FORMULA QUICK REFERENCE (UNITS 1 TO 6)

---

## UNIT 1: FUNDAMENTALS & PHYSICAL LAYER

### 1.1 Delay Components in Packet-Switched Networks
* **Transmission Delay (T_trans)**:
  `T_trans = L / B`
  - `L` = Packet / Frame length (in bits)
  - `B` = Channel Bandwidth / Data Transmission Rate (in bits per second, bps)
  - Unit of `T_trans`: seconds (s) or milliseconds (ms).
  - *Physical Meaning*: Time required to push all bits of the frame onto the physical wire.

* **Propagation Delay (T_prop)**:
  `T_prop = d / v`
  - `d` = Distance between sender and receiver (in meters, m)
  - `v` = Propagation speed in transmission medium (in meters/sec, m/s; typically `2 x 10^8 m/s` in copper/fiber, `3 x 10^8 m/s` in free space / vacuum)
  - Unit of `T_prop`: seconds (s) or milliseconds (ms).
  - *Physical Meaning*: Time taken for the first bit to travel from sender to receiver.

* **Total End-to-End Latency**:
  `Total Delay = T_trans + T_prop + T_proc + T_queue`
  - `T_proc` = Node processing delay (header parsing, checksum verification, routing table lookup)
  - `T_queue` = Queuing delay at intermediate router buffers waiting for transmission.

### 1.2 Bandwidth-Delay Product (BDP)
`BDP = Bandwidth x RTT = B x (2 * T_prop)`
- `B` = Link capacity (bps)
- `RTT` = Round Trip Time (seconds)
- Unit of BDP: bits or bytes (`BDP / 8`).
- *Physical Meaning*: The volume of data bits that can be "in flight" inside the transmission pipe at any given instant. Represents the optimal sliding window size to keep the link 100% saturated.

### 1.3 Channel Capacities (Nyquist & Shannon Theorems)
* **Nyquist Bit Rate (For Ideal Noiseless Channels)**:
  `C_Nyquist = 2 * B * log2(M)`
  - `B` = Bandwidth of the channel (in Hertz, Hz)
  - `M` = Number of discrete signal levels (voltage levels); `M = 2^k` where `k` = bits per signal element.
  - Unit: bits per second (bps).

* **Shannon Capacity (For Real Noisy Channels)**:
  `C_Shannon = B * log2(1 + SNR)`
  - `B` = Channel Bandwidth (Hz)
  - `SNR` = Signal-to-Noise Power Ratio (linear ratio: `Power_signal / Power_noise`, dimensionless)
  - `SNR_dB = 10 * log10(SNR)` ==> `SNR = 10^(SNR_dB / 10)`
  - Unit: bits per second (bps).
  - *Note*: `C_Shannon` represents the absolute theoretical upper bound on error-free information rate.

---

## UNIT 2: DATA LINK LAYER & ERROR CONTROL

### 2.1 Cyclic Redundancy Check (CRC) Polynomial Division
* **Generator Polynomial**: `G(x)` of degree `r` has `(r + 1)` bits.
* **Message Bits**: `M` of length `k` bits.
* **Appended Message**: Append `r` zeros to `M` ==> `M' = M * 2^r` (length `k + r` bits).
* **Modulo-2 Division (XOR Subtraction)**:
  `Remainder (FCS) = (M * 2^r) mod_2 G(x)`
  - `FCS` length = strictly `r` bits.
* **Transmitted Codeword**: `T = M * 2^r + FCS` (length `k + r` bits).
* **Receiver Verification**:
  `T mod_2 G(x) == 0` ==> Frame accepted as error-free.
  `T mod_2 G(x) != 0` ==> Error detected; frame rejected.

### 2.2 Hamming Error-Correcting Code
* **Parity Bits Inequality (Hamming Rule)**:
  `2^r >= m + r + 1`
  - `m` = Number of original data bits
  - `r` = Number of redundant parity check bits
  - Total codeword length `n = m + r`.
* **Parity Bit Positions**:
  - Parity bits are placed strictly at bit indices that are powers of 2:
    `P_1 at Position 1 (2^0)`, `P_2 at Position 2 (2^1)`, `P_4 at Position 4 (2^2)`, `P_8 at Position 8 (2^3)`, etc.
* **Syndrome Calculation (Even Parity)**:
  `Syndrome S = C_r ... C_2 C_1 in binary`
  - If `S == 0`: No error in received codeword.
  - If `S != 0`: Single-bit error is located precisely at bit index `S`. Invert bit at index `S` to correct.

### 2.3 Flow Control & Sliding Window ARQ Protocols
* **Dimensionless Propagation Parameter (a)**:
  `a = T_prop / T_trans = (d / v) / (L / B)`

* **Stop-and-Wait ARQ Efficiency (Utilization eta)**:
  `eta = T_trans / (T_trans + 2 * T_prop + T_proc + T_ack)`
  If `T_proc` and `T_ack` are negligible:
  `eta = 1 / (1 + 2a)`
  `Throughput = eta * Bandwidth = B / (1 + 2a)`

* **Optimal Window Size for 100% Channel Utilization**:
  `W_optimal = 1 + 2a`

* **Go-Back-N (GBN) ARQ**:
  - Sender Window Size: `W_s = 2^k - 1` (where `k` = bits in Sequence Number field)
  - Receiver Window Size: `W_r = 1`
  - Channel Efficiency:
    `eta = min(1.0, W_s / (1 + 2a))`

* **Selective Repeat (SR) ARQ**:
  - Sender Window Size: `W_s = 2^(k - 1)`
  - Receiver Window Size: `W_r = 2^(k - 1)`
  - Channel Efficiency:
    `eta = min(1.0, W_s / (1 + 2a))`

---

## UNIT 3: MEDIUM ACCESS CONTROL (MAC) SUBLAYER

### 3.1 ALOHA Protocols Throughput
* **Pure ALOHA**:
  - Vulnerable Period: `2 * T_frame`
  - Throughput Equation: `S = G * e^(-2G)`
  - Maximum Throughput: `S_max = 1 / (2e) approx. 0.184 = 18.4%` at offered traffic load `G = 0.5`.

* **Slotted ALOHA**:
  - Vulnerable Period: `1 * T_frame`
  - Throughput Equation: `S = G * e^(-G)`
  - Maximum Throughput: `S_max = 1 / e approx. 0.368 = 36.8%` at offered traffic load `G = 1.0` (Double Pure ALOHA!).

### 3.2 CSMA/CD (Ethernet IEEE 802.3)
* **Collision Detection Condition**:
  `T_trans >= 2 * T_prop`
  `L_min / B >= 2 * (d / v)`
  `L_min = 2 * T_prop * Bandwidth`
  - Standard Ethernet (10 Mbps, 2.5 km bus, `T_prop = 25.6 us`):
    `L_min = 2 * (25.6 us) * 10 Mbps = 51.2 us * 10 Mbps = 512 bits = 64 Bytes`.

* **Network Diameter (D_max) Inverse Law**:
  `D_max <= (L_min * v) / (2 * B) proportional to (1 / B)`
  - As network bandwidth `B` increases, maximum allowable network diameter `D_max` shrinks inversely.

* **Binary Exponential Back-Off (BEB) Algorithm**:
  - After `i`-th collision, station waits:
    `Wait Time = K * Slot_Time`
    where `Slot_Time = 51.2 us` (for 10 Mbps Ethernet).
  - `K` is an integer chosen randomly from:
    `K in [0, 2^k - 1]  where  k = min(i, 10)`
  - Maximum attempts = 16 (abort and declare link failure after 16 collisions).

---

## UNIT 4: NETWORK LAYER & ADDRESSING

### 4.1 IPv4 Header Calculations
* **Header Length (IHL)**:
  `Header Bytes = IHL * 4`
  - Minimum IHL = 5 (`5 * 4 = 20 Bytes` with no options).
  - Maximum IHL = 15 (`15 * 4 = 60 Bytes` with 40 Bytes of options).
* **Payload Length**:
  `Payload Data Bytes = Total Length - (IHL * 4)`

### 4.2 IPv4 Datagram Fragmentation
* **Maximum Fragment Payload Size**:
  `Max Payload per Fragment = floor((MTU - Header_Size) / 8) * 8`
  *(Must strictly be a multiple of 8 bytes!)*
* **Fragment Offset**:
  `Fragment Offset Field Value = (Starting Byte Position of Data) / 8`
* **More Fragments Flag (MF)**:
  - `MF = 1` for all intermediate fragments.
  - `MF = 0` for the last fragment.

### 4.3 Subnetting & CIDR Address Math
* For prefix `/n`:
  - Host bits: `h = 32 - n`
  - Subnet bits borrowed: `s = n - Default_Class_Prefix`
  - Total IP Addresses in block: `2^h`
  - **Usable Host Addresses**: `2^h - 2` (subtracting Network ID where host bits=0, and Directed Broadcast where host bits=1).
  - Subnet Mask: First `n` bits are 1s, remaining `32 - n` bits are 0s.
  - Block size (step size in relevant octet): `256 - Mask_Octet = 2^h`.

### 4.4 Distance Vector (Bellman-Ford) Update Formula
`D_x(y) = min_{v in neighbors(x)} { c(x, v) + D_v(y) }`
- `c(x, v)` = direct cost of link from node `x` to neighbor `v`.
- `D_v(y)` = distance from neighbor `v` to destination `y` (from `v`'s vector).

### 4.5 Traffic Shaping (Token Bucket Algorithm)
* **Maximum Burst Duration (S)**:
  `S = b / (M - r)`
  - `b` = Token bucket capacity (in bits or bytes)
  - `M` = Maximum peak transmission rate (bps)
  - `r` = Token arrival / replenishment rate (bps)
* **Total Burst Data Volume (V)**:
  `V = S * M = (b * M) / (M - r)`

---

## UNIT 5: TRANSPORT LAYER & CONGESTION CONTROL

### 5.1 TCP Retransmission Timeout (RTO) - Jacobson's Algorithm
* **Smoothed RTT (SRTT)**:
  `SRTT_new = (1 - alpha) * SRTT_old + alpha * RTT_measured`  *(typically alpha = 0.125 = 1/8)*
* **RTT Variation (RTTVAR)**:
  `RTTVAR_new = (1 - beta) * RTTVAR_old + beta * |SRTT_new - RTT_measured|`  *(typically beta = 0.25 = 1/4)*
* **Timeout Formula**:
  `RTO = SRTT_new + 4 * RTTVAR_new`
* **Karn's Algorithm**:
  - Never update RTO calculation on retransmitted packets.
  - On timeout, double RTO: `RTO_new = 2 * RTO_old` (Exponential Timer Backoff).

### 5.2 TCP Window & Throughput
* **Usable Send Window**:
  `Usable Window = min(cwnd, rwnd) - (LastByteSent - LastByteAcked)`
  - `cwnd` = Congestion Window maintained by sender.
  - `rwnd` = Advertised Receive Window sent by receiver.
* **TCP Average Throughput (Mathis Formula)**:
  `Throughput <= (1.22 * MSS) / (RTT * sqrt(p))`
  where `p` = packet loss probability.

---

## UNIT 6: APPLICATION LAYER

### 6.1 Base64 MIME Encoding Expansion
* **Conversion**: 3 binary bytes (24 bits) are mapped to 4 printable ASCII characters (6 bits each, values 0-63).
* **Encoding Overhead**: `+33.33%`
  `Encoded Size in Bytes = ceil(Raw_Bytes / 3) * 4`
* **Padding Rules**:
  - If `Raw_Bytes mod 3 == 1`: 2 bytes of padding `==`.
  - If `Raw_Bytes mod 3 == 2`: 1 byte of padding `=`.

### 6.2 HTTP Transfer Latencies
* **Non-Persistent HTTP (HTTP/1.0)**:
  `Total Time = Sum_{i=1}^N (2 * RTT + T_trans,i)`
* **Persistent HTTP with Pipelining (HTTP/1.1)**:
  `Total Time = 2 * RTT + Sum_{i=1}^N T_trans,i`

---
---

# PART 2: COMPLETE WORKED NUMERICAL BANK
### (One High-Level Exam Problem Per Formula with Step-by-Step Solutions)

---

## PROBLEM 1: Transmission Delay vs Propagation Delay [Unit 1]
**Exam Problem Statement:**
> A 100 Megabyte file is to be transmitted from a host in Pune to a host in Tokyo across a 10,000 km optical fiber link with a transmission bandwidth of 100 Mbps. The propagation speed of light in optical fiber is `2 x 10^8 m/s`.
> 1. Calculate the Transmission Delay (`T_trans`).
> 2. Calculate the one-way Propagation Delay (`T_prop`).
> 3. Which delay dominates, and what does this imply for interactive real-time applications?

**Step-by-Step Solution:**
1. **Given Data:**
   - File Size `L = 100 MB = 100 x 10^6 Bytes = 800 x 10^6 bits` (Decimal networking standard)
   - Bandwidth `B = 100 Mbps = 100 x 10^6 bps = 10^8 bps`
   - Distance `d = 10,000 km = 10^7 meters`
   - Propagation speed `v = 2 x 10^8 m/s`
2. **Transmission Delay:**
   `T_trans = L / B = (800 x 10^6 bits) / (100 x 10^6 bps) = 8.0 seconds`.
3. **Propagation Delay:**
   `T_prop = d / v = (10^7 m) / (2 x 10^8 m/s) = 0.05 seconds = 50 ms`.
4. **Dominance & Analysis:**
   - Here, `T_trans (8000 ms) >> T_prop (50 ms)`. Transmission delay dominates bulk file transfer by a factor of 160:1!
   - For interactive real-time applications (e.g. VoIP or gaming), packets are very small (e.g. 100 Bytes ==> `T_trans = 8 us`), making `T_prop (50 ms)` the absolute dominating bottleneck that cannot be reduced by purchasing more bandwidth.
* **Examiner Trap**: Forgetting to multiply Megabytes by 8 to convert to bits before dividing by Mbps!

---

## PROBLEM 2: Bandwidth-Delay Product (BDP) & Optimal Window [Unit 1 & 2]
**Exam Problem Statement:**
> A satellite communication channel operates at a data rate of 50 Mbps with a round-trip propagation time (RTT) of 480 ms. 
> 1. Calculate the Bandwidth-Delay Product (BDP) in bits and in Megabytes.
> 2. If standard TCP without window scaling has a maximum advertised window (`rwnd`) of 65,535 Bytes, calculate the maximum link utilization that this TCP connection can achieve.

**Step-by-Step Solution:**
1. **Given Data:**
   - `B = 50 Mbps = 50 x 10^6 bps`
   - `RTT = 480 ms = 0.48 seconds`
2. **Bandwidth-Delay Product:**
   `BDP = B x RTT = (50 x 10^6 bps) x (0.48 s) = 24,000,000 bits = 24 Mbits`.
   In Bytes: `BDP = 24,000,000 / 8 = 3,000,000 Bytes = 3 Megabytes (3 MB)`.
3. **Maximum Utilization without Window Scale Option:**
   - Max window allowed by unscaled 16-bit TCP window field: `W_max = 65,535 Bytes`.
   - Max Throughput achievable:
     `Throughput = W_max / RTT = (65,535 x 8 bits) / 0.48 s = 524,280 / 0.48 = 1,092,250 bps = 1.092 Mbps`.
   - Link Utilization:
     `eta = (1.092 Mbps / 50 Mbps) x 100% = 2.18%`!
4. **Engineering Insight:**
   - Over 97.8% of the expensive satellite link is wasted! This proves why **RFC 1323 TCP Window Scale Option** (which allows windows up to 1 GB) is mandatory for Long Fat Networks (LFNs).

---

## PROBLEM 3: Shannon vs Nyquist Capacity [Unit 1]
**Exam Problem Statement:**
> A standard telephone channel has a bandwidth of 3 kHz and a Signal-to-Noise Ratio (SNR) of 30 dB.
> 1. Calculate the theoretical maximum Shannon Channel Capacity.
> 2. If we wish to transmit data at this Shannon capacity over a noiseless channel of the same 3 kHz bandwidth, how many discrete signal levels (`M`) are required according to Nyquist's theorem?

**Step-by-Step Solution:**
1. **Convert SNR from dB to Linear Ratio:**
   `SNR_dB = 10 * log10(SNR) = 30 dB`
   `log10(SNR) = 3 ==> SNR = 10^3 = 1000`.
2. **Shannon Capacity:**
   `C_Shannon = B * log2(1 + SNR) = 3000 * log2(1 + 1000) = 3000 * log2(1001)`.
   Since `2^10 = 1024`, `log2(1001) approx. 9.9672`.
   `C_Shannon = 3000 * 9.9672 = 29,901.6 bps approx. 30 kbps` (the famous 28.8k / 33.6k dial-up modem limit!).
3. **Nyquist Equivalent Signal Levels:**
   Set `C_Nyquist = C_Shannon`:
   `2 * B * log2(M) = 29,902`
   `2 * 3000 * log2(M) = 29,902 ==> 6000 * log2(M) = 29,902`
   `log2(M) = 29,902 / 6000 = 4.9837`
   `M = 2^4.9837 approx. 31.64`
   Round up to next power of 2: **M = 32 discrete voltage levels** (5 bits per baud).

---

## PROBLEM 4: Cyclic Redundancy Check (CRC-4) Modulo-2 Division [Unit 2]
*(From May 2019 VIT ESE Q1b)*
**Exam Problem Statement:**
> A bit string `1101011011` is to be transmitted using CRC error detection with the generator polynomial `G(x) = x^4 + x + 1`.
> 1. Find the transmitted codeword frame `T`.
> 2. Show that the receiver detects no error if the frame arrives uncorrupted.
> 3. Show that an error in the 4th bit from the left is detected by the receiver.

**Step-by-Step Solution:**
1. **Representations:**
   - `M = 1101011011` (10 bits)
   - `G(x) = 1*x^4 + 0*x^3 + 0*x^2 + 1*x^1 + 1*x^0 ==> 10011` (5 bits, degree `r = 4`).
2. **Sender Modulo-2 Division:**
   - Append `r = 4` zeros: `M' = 11010110110000` (14 bits).
   - Divide by `10011` using XOR subtraction (no carries/borrows):
```
              1100100101  (Quotient)
      ------------------
10011 ) 11010110110000
        10011
        -----
         10011
         10011
         -----
          000010110
              10011
              -----
               010100
                10011
                -----
                 01110  <-- Remainder FCS (4 bits)
```
   - Remainder `FCS = 1110`.
   - Transmitted Codeword `T = 11010110111110` (14 bits).
3. **Receiver Verification (No Error):**
   - Divide `11010110111110` by `10011`.
   - Remainder evaluates to strictly `0000`. Frame accepted!
4. **Error Detection (4th bit inverted: `1101...` --> `1100...`):**
   - Corrupted Frame `T' = 11000110111110`.
   - Divide `T'` by `10011`:
     Remainder evaluates to `0110 != 0000`. Non-zero remainder flags error; frame discarded.

---

## PROBLEM 5: Hamming Code Error Correction (12-Bit Syndrome Test) [Unit 2]
*(From Nov 2024 VIT ESE Q2b-OR)*
**Exam Problem Statement:**
> A receiver receives the 12-bit codeword `011100101110`. Assuming even parity Hamming code was used, test if the codeword is correct. If incorrect, determine the bit position in error, the corrected codeword, and extract the original data bits.

**Step-by-Step Solution:**
1. **Codeword Layout (Positions 1 to 12 from left to right):**

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Role** | P1 | P2 | D1 | P4 | D2 | D3 | D4 | P8 | D5 | D6 | D7 | D8 |
| **Received Bit** | **0** | **1** | **1** | **1** | **0** | **0** | **1** | **0** | **1** | **1** | **1** | **0** |

2. **Calculate Parity Check Bits (Even Parity ==> XOR sum should equal 0):**
   - `C_1` (checks bits with binary position ending in 1: 1, 3, 5, 7, 9, 11):
     `C_1 = b_1 XOR b_3 XOR b_5 XOR b_7 XOR b_9 XOR b_11`
     `C_1 = 0 XOR 1 XOR 0 XOR 1 XOR 1 XOR 1 = 0` (Even number of 1s ==> No error in bit 0).
   - `C_2` (checks bits with 2nd bit = 1: 2, 3, 6, 7, 10, 11):
     `C_2 = b_2 XOR b_3 XOR b_6 XOR b_7 XOR b_10 XOR b_11`
     `C_2 = 1 XOR 1 XOR 0 XOR 1 XOR 1 XOR 1 = 1` (Odd number of 1s ==> **Error detected!**).
   - `C_4` (checks bits with 3rd bit = 1: 4, 5, 6, 7, 12):
     `C_4 = b_4 XOR b_5 XOR b_6 XOR b_7 XOR b_12`
     `C_4 = 1 XOR 0 XOR 0 XOR 1 XOR 0 = 0` (Even number of 1s ==> No error in bit 2).
   - `C_8` (checks bits with 4th bit = 1: 8, 9, 10, 11, 12):
     `C_8 = b_8 XOR b_9 XOR b_10 XOR b_11 XOR b_12`
     `C_8 = 0 XOR 1 XOR 1 XOR 1 XOR 0 = 1` (Odd number of 1s ==> **Error detected!**).
3. **Syndrome Evaluation:**
   `Syndrome S = C_8 C_4 C_2 C_1 = 1010 in binary = 8 + 0 + 2 + 0 = 10`.
   - **Conclusion**: Single-bit error is located at **Position 10**!
4. **Correction:**
   - Received bit at position 10 was `1`. Invert to `0`.
   - **Corrected Codeword**: `0 1 1 1 0 0 1 0 1 0 1 0`.
5. **Extract Original Data Bits (Positions 3, 5, 6, 7, 9, 10, 11, 12):**
   - `D_1 (Pos 3) = 1`
   - `D_2 (Pos 5) = 0`
   - `D_3 (Pos 6) = 0`
   - `D_4 (Pos 7) = 1`
   - `D_5 (Pos 9) = 1`
   - `D_6 (Pos 10) = 0` (Corrected!)
   - `D_7 (Pos 11) = 1`
   - `D_8 (Pos 12) = 0`
   - **Original Data = `10011010` (Hex: `0x9A`)**.

---

## PROBLEM 6: Stop-and-Wait Frame Size Determination [Unit 2]
*(From Nov 2024 VIT ESE Q1a)*
**Exam Problem Statement:**
> In a network using Stop-and-Wait protocol, the link bandwidth is 4 Kbps and one-way propagation delay is 20 ms. If the required channel utilization / efficiency is 50%, find the required frame size in bytes.

**Step-by-Step Solution:**
1. **Given:**
   - `B = 4 Kbps = 4000 bps`
   - `T_prop = 20 ms = 0.020 s`
   - `Efficiency (eta) = 50% = 0.5`
2. **Efficiency Equation:**
   `eta = 1 / (1 + 2a) = 0.5`
   `1 + 2a = 1 / 0.5 = 2 ==> 2a = 1 ==> a = 0.5`.
3. **Find Transmission Time:**
   `a = T_prop / T_trans ==> T_trans = T_prop / a = 20 ms / 0.5 = 40 ms = 0.040 s`.
4. **Find Frame Size (L):**
   `T_trans = L / B ==> L = T_trans * B = 0.040 s * 4000 bps = 160 bits`.
   `L in Bytes = 160 / 8 = 20 Bytes`.
* **Result**: Required frame size is strictly **20 Bytes (160 bits)**.

---

## PROBLEM 7: Satellite Link Stop-and-Wait Efficiency with Overhead [Unit 2]
*(From Nov 2024 VIT ESE Q1c)*
**Exam Problem Statement:**
> A satellite transmission line connecting two ground stations uses Stop-and-Wait protocol with the following parameters:
> - Data Transmission Rate = 64 kbps
> - Frame Size = 2048 Bytes
> - One-way Propagation Delay = 180 ms
> - Acknowledgement Size = 10 Bytes
> - Processing Delay of one station = 25 ms
> Calculate: (1) Total cycle time, (2) Channel utilization, (3) Effective throughput.

**Step-by-Step Solution:**
1. **Calculations of Individual Delays:**
   - Data frame transmission: `T_t,frame = (2048 * 8 bits) / 64,000 bps = 16,384 / 64,000 = 0.256 s = 256 ms`.
   - Propagation delay: `T_prop = 180 ms`.
   - Receiver processing delay: `T_proc,rx = 25 ms`.
   - ACK frame transmission: `T_t,ack = (10 * 8 bits) / 64,000 bps = 80 / 64,000 = 0.00125 s = 1.25 ms`.
   - Return propagation delay: `T_prop = 180 ms`.
   - Sender processing delay: `T_proc,tx = 25 ms`.
2. **Total Round-Trip Cycle Time:**
   `T_total = 256 + 180 + 25 + 1.25 + 180 + 25 = 667.25 ms = 0.66725 s`.
3. **Channel Efficiency (eta):**
   `eta = T_t,frame / T_total = 256 ms / 667.25 ms = 0.38366 approx. 38.37%`.
4. **Effective Throughput:**
   `Throughput = eta * Bandwidth = 0.3837 * 64 kbps = 24.56 kbps`.

---

## PROBLEM 8: Sliding Window Window Sizes (GBN vs SR) [Unit 2]
*(From Nov 2024 VIT ESE Q2a)*
**Exam Problem Statement:**
> For a communications link, transmission time `T_trans = 1 ms` and propagation delay `T_prop = 99.5 ms`. A 7-bit sequence number header field (`N = 7`) is predefined.
> 1. Find the optimal sender window size for 100% efficiency.
> 2. With the predefined 7-bit header, find the sender window size and efficiency for Go-Back-N (GBN).
> 3. With the predefined 7-bit header, find the sender window size and efficiency for Selective Repeat (SR).

**Step-by-Step Solution:**
1. **Calculate Parameter a & Optimal Window:**
   `a = T_prop / T_trans = 99.5 ms / 1 ms = 99.5`.
   `W_optimal = 1 + 2a = 1 + 2(99.5) = 1 + 199 = 200 frames`.
2. **Go-Back-N (GBN) Analysis with N = 7 bits:**
   - Max GBN Sender Window: `W_s = 2^N - 1 = 2^7 - 1 = 128 - 1 = 127 frames`.
   - Channel Efficiency:
     `eta_GBN = W_s / (1 + 2a) = 127 / 200 = 0.635 = 63.5%`.
3. **Selective Repeat (SR) Analysis with N = 7 bits:**
   - Max SR Sender Window: `W_s = 2^(N - 1) = 2^(7 - 1) = 2^6 = 64 frames`.
   - Channel Efficiency:
     `eta_SR = W_s / (1 + 2a) = 64 / 200 = 0.320 = 32.0%`.
4. **Bits Required for 100% Saturation (`W = 200`):**
   - For GBN: `2^k - 1 >= 200 ==> 2^k >= 201 ==> k = 8 bits` (`2^8 = 256`).
   - For SR: `2^(k - 1) >= 200 ==> 2^k >= 400 ==> k = 9 bits` (`2^9 = 512`).

---

## PROBLEM 9: Pure vs Slotted ALOHA Maximum Throughput [Unit 3]
**Exam Problem Statement:**
> A shared wireless channel has a bandwidth of 56 kbps. Stations transmit fixed-size frames of 1000 bits. 
> 1. Find the frame transmission time (`T_frame`).
> 2. What is the maximum throughput in frames/sec and bps under Pure ALOHA?
> 3. What is the maximum throughput under Slotted ALOHA?

**Step-by-Step Solution:**
1. **Frame Transmission Time:**
   `T_frame = L / B = 1000 bits / 56,000 bps = 0.017857 seconds = 17.86 ms`.
   Raw link frame capacity: `1 / T_frame = 56,000 / 1000 = 56 frames/second`.
2. **Pure ALOHA Maximums:**
   - Max Efficiency `S_max = 1 / (2e) = 0.184 = 18.4%` (at `G = 0.5`).
   - Max Throughput in frames/sec: `56 * 0.184 = 10.3 frames/sec`.
   - Max Throughput in bps: `0.184 * 56,000 bps = 10,304 bps = 10.3 kbps`.
3. **Slotted ALOHA Maximums:**
   - Max Efficiency `S_max = 1 / e = 0.368 = 36.8%` (at `G = 1.0`).
   - Max Throughput in frames/sec: `56 * 0.368 = 20.6 frames/sec`.
   - Max Throughput in bps: `0.368 * 56,000 bps = 20,608 bps = 20.6 kbps`.
* **Key Takeaway**: Slotted ALOHA halves the vulnerable period from `2 * T_frame` to `1 * T_frame`, exactly doubling maximum throughput!

---

## PROBLEM 10: CSMA/CD Minimum Frame Length & Network Diameter [Unit 3]
*(From May 2019 VIT ESE Q1c & Q2c)*
**Exam Problem Statement:**
> A 1 Gbps Gigabit Ethernet network operates with a minimum frame size of 512 bits. The propagation velocity in the cable is `2 x 10^8 m/s`.
> 1. Calculate the maximum allowable cable length (network diameter) for collision detection to function correctly.
> 2. Explain why Gigabit Ethernet required either Carrier Extension or switched full-duplex operation.

**Step-by-Step Solution:**
1. **Governing Law:**
   `T_trans >= 2 * T_prop ==> L_min / B >= 2 * (d_max / v)`
   `d_max <= (L_min * v) / (2 * B)`
2. **Calculate d_max:**
   `L_min = 512 bits`
   `B = 1 Gbps = 10^9 bps`
   `v = 2 x 10^8 m/s`
   `d_max = (512 * 2 x 10^8) / (2 * 10^9) = (1024 x 10^8) / (2 x 10^9) = 1024 / 20 = 51.2 meters`.
3. **Engineering Justification:**
   - At 1 Gbps, a 512-bit frame limits the network diameter to merely **51.2 meters**, which is too short even to wire a single floor of an office building!
   - To increase network span to 200 meters without changing the 64-byte minimum frame seen by IP, the IEEE 802.3z standard introduced **Carrier Extension** (padding slots to 512 Bytes = 4096 bits) and **Frame Bursting**, and ultimately replaced shared hubs with full-duplex Ethernet switches (eliminating collisions entirely).

---

## PROBLEM 11: Binary Exponential Back-Off (BEB) Probabilities [Unit 3]
*(From Nov 2024 VIT ESE Q3b)*
**Exam Problem Statement:**
> Two stations A and B on a 10 Mbps CSMA/CD network attempt transmission and collide. 
> 1. What is the probability that they collide a second consecutive time?
> 2. What is the probability that they collide on the third consecutive attempt?
> 3. What is the maximum back-off time station A could wait after the 12th collision?

**Step-by-Step Solution:**
1. **First Collision (i = 1):**
   `k = min(1, 10) = 1 ==> K in [0, 2^1 - 1] = {0, 1}` (2 choices).
   Total combinations for `(K_A, K_B) = 2 x 2 = 4`.
   Collision occurs if `K_A == K_B`: `{(0,0), (1,1)}` (2 outcomes).
   `Probability of 2nd collision = 2 / 4 = 1/2 = 50%`.
2. **Second Collision (i = 2):**
   `k = min(2, 10) = 2 ==> K in [0, 2^2 - 1] = {0, 1, 2, 3}` (4 choices).
   Total combinations = `4 x 4 = 16`.
   Collision occurs if `K_A == K_B`: 4 outcomes.
   `Probability of 3rd collision = 4 / 16 = 1/4 = 25%`.
3. **12th Collision (i = 12):**
   `k = min(12, 10) = 10` (Truncated at 10!).
   Max `K = 2^10 - 1 = 1023`.
   `Slot_Time = 51.2 us`.
   `Max Wait Time = 1023 * 51.2 us = 52,377.6 us approx. 52.38 ms`.

---

## PROBLEM 12: IPv4 MTU Datagram Fragmentation [Unit 4]
**Exam Problem Statement:**
> An IPv4 datagram with a 20-byte header and 3980 bytes of data (Total Length = 4000 Bytes, Identification = `0xABCD`, `DF = 0`, `MF = 0`) arrives at a router whose outgoing link has an MTU of 1500 Bytes.
> Show the fragmentation table listing: Fragment Number, Total Length, Data Length, Flag MF, and Fragment Offset for all fragments.

**Step-by-Step Solution:**
1. **Determine Maximum Data per Fragment:**
   - Link MTU = 1500 Bytes.
   - Header = 20 Bytes.
   - Space for data = `1500 - 20 = 1480 Bytes`.
   - Verify multiple of 8: `1480 / 8 = 185` (exact integer multiple!).
2. **Divide Data (3980 Bytes):**
   - Fragment 1: Takes `1480 Bytes` (Bytes 0 to 1479). Remaining data = `3980 - 1480 = 2500 Bytes`.
   - Fragment 2: Takes `1480 Bytes` (Bytes 1480 to 2959). Remaining data = `2500 - 1480 = 1020 Bytes`.
   - Fragment 3: Takes remaining `1020 Bytes` (Bytes 2960 to 3979).
3. **Construct Fragmentation Table:**

| Frag # | Total Length | Header | Data Payload | Flag DF | Flag MF | Fragment Offset |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | **1500 B** | 20 B | 1480 B | 0 | **1** | `0 / 8 =` **0** |
| **2** | **1500 B** | 20 B | 1480 B | 0 | **1** | `1480 / 8 =` **185** |
| **3** | **1040 B** | 20 B | 1020 B | 0 | **0** | `2960 / 8 =` **370** |

* **Verification**: Total original data = `1480 + 1480 + 1020 = 3980 Bytes`. All offsets strictly in units of 8 bytes.

---

## PROBLEM 13: Class C Subnetting with Host 1 Addresses [Unit 4]
*(From Nov 2024 VIT ESE Q3a)*
**Exam Problem Statement:**
> Subnet the Class C network `195.1.1.0` so that you have at least 10 subnets, each supporting up to 12 hosts.
> 1. Determine the subnet mask and CIDR prefix.
> 2. Determine the block size (step size).
> 3. State the IP address assigned to Host 1 on Subnet 0, Subnet 1, Subnet 2, Subnet 3, and Subnet 10.

**Step-by-Step Solution:**
1. **Bit Budget Analysis:**
   - Class C has 8 host bits available in the 4th octet (`/24`).
   - Need >= 10 subnets: `2^s >= 10 ==> s = 4 subnet bits` (`2^4 = 16 subnets`).
   - Need >= 12 hosts/subnet: `2^h - 2 >= 12 ==> 2^h >= 14 ==> h = 4 host bits` (`2^4 - 2 = 14 usable hosts`).
   - Check: `s + h = 4 + 4 = 8 bits`. Matches 8 bits perfectly!
2. **Prefix & Mask:**
   - New prefix: `24 + 4 = /28`.
   - Subnet Mask: `11111111.11111111.11111111.11110000 = 255.255.255.240`.
   - Block Size = `2^h = 2^4 = 16`.
3. **Subnetting Table & Host 1 Calculation (`Host 1 = Net ID + 1`):**

| Subnet # | Network ID (Net ID) | Host 1 IP Address | Usable Range | Directed Broadcast |
| :---: | :---: | :---: | :---: | :---: |
| **Subnet 0** | `195.1.1.0/28` | **`195.1.1.1`** | `.1` to `.14` | `195.1.1.15` |
| **Subnet 1** | `195.1.1.16/28` | **`195.1.1.17`** | `.17` to `.30` | `195.1.1.31` |
| **Subnet 2** | `195.1.1.32/28` | **`195.1.1.33`** | `.33` to `.46` | `195.1.1.47` |
| **Subnet 3** | `195.1.1.48/28` | **`195.1.1.49`** | `.49` to `.62` | `195.1.1.63` |
| **Subnet 10** | `195.1.1.160/28` | **`195.1.1.161`** | `.161` to `.174` | `195.1.1.175` |

*(Formula for Subnet k: `Net ID = 195.1.1.(16 * k)`, `Host 1 = 195.1.1.(16 * k + 1)`)*.

---

## PROBLEM 14: CIDR Block Analysis [Unit 4]
*(From May 2019 VIT ESE Q3b)*
**Exam Problem Statement:**
> An organization is assigned the CIDR address block `167.199.130.83/25`. Find:
> 1. The subnet netmask.
> 2. The total IP block size.
> 3. The first IP address (Network ID).
> 4. The last IP address (Directed Broadcast Address).
> 5. The usable host IP range.

**Step-by-Step Solution:**
1. **Subnet Mask:**
   - Prefix `/25` has 25 leading ones:
     `11111111.11111111.11111111.10000000 = 255.255.255.128`.
2. **Block Size:**
   - Host bits: `h = 32 - 25 = 7 bits`.
   - Total IP addresses = `2^h = 2^7 = 128 addresses`.
3. **First IP Address (Network ID):**
   - 4th octet value is `83`.
   - Block boundaries with block size 128 in 4th octet: `0, 128`.
   - Since `0 <= 83 < 128`, the base network address is:
     **First IP = `167.199.130.0`**.
4. **Last IP Address (Directed Broadcast):**
   - Last IP = `Base + Block Size - 1 = 0 + 128 - 1 = 127`.
   - **Last IP = `167.199.130.127`**.
5. **Usable Host Range:**
   - First usable = `Net ID + 1 = 167.199.130.1`.
   - Last usable = `Broadcast - 1 = 167.199.130.126`.
   - Total usable hosts = `128 - 2 = 126 hosts`.

---

## PROBLEM 15: Distance Vector Routing Table Update [Unit 4]
*(From May 2019 VIT ESE Q3a)*
**Exam Problem Statement:**
> In a subnet using Distance Vector Routing, router C has three neighbors: B, D, and E. The measured link delays from C to these neighbors are:
> - Delay to B = 6
> - Delay to D = 3
> - Delay to E = 5
> The incoming vectors received at C are:
> - From B: `(5, 0, 8, 12, 6, 2)` for destinations `(A, B, C, D, E, F)`
> - From D: `(16, 12, 6, 0, 9, 10)`
> - From E: `(7, 6, 3, 9, 0, 4)`
> Calculate C's new routing table.

**Step-by-Step Solution:**
1. **Bellman-Ford Formula for destination X:**
   `D_C(X) = min[ c(C, B) + D_B(X),  c(C, D) + D_D(X),  c(C, E) + D_E(X) ]`
2. **Evaluations for Each Destination:**
   - **To A**: `min(6 + 5,  3 + 16,  5 + 7) = min(11, 19, 12) = 11` (via line B).
   - **To B**: `min(6 + 0,  3 + 12,  5 + 6) = min(6, 15, 11) = 6` (via line B).
   - **To C**: Distance to self is strictly `0`.
   - **To D**: `min(6 + 12,  3 + 0,  5 + 9) = min(18, 3, 14) = 3` (via line D).
   - **To E**: `min(6 + 6,  3 + 9,  5 + 0) = min(12, 12, 5) = 5` (via line E).
   - **To F**: `min(6 + 2,  3 + 10,  5 + 4) = min(8, 13, 9) = 8` (via line B).
3. **C's New Routing Table:**

| Destination | Minimum Delay Cost | Next Hop / Outgoing Line |
| :---: | :---: | :---: |
| **A** | **11** | **B** |
| **B** | **6** | **B** |
| **C** | **0** | **-** |
| **D** | **3** | **D** |
| **E** | **5** | **E** |
| **F** | **8** | **B** |

---

## PROBLEM 16: Negative Weight Cycle Detection [Unit 4]
*(From Nov 2024 VIT ESE Q4b)*
**Exam Problem Statement:**
> Given a directed graph with edges: `0 --> 1 (weight +1)`, `1 --> 2 (weight +3)`, `2 --> 3 (weight +2)`, and `3 --> 1 (weight -6)`.
> 1. Determine whether a negative weight cycle exists.
> 2. Explain how the Bellman-Ford algorithm mathematically detects this condition.

**Step-by-Step Solution:**
1. **Direct Cycle Sum:**
   - Cycle is formed by vertices `1 --> 2 --> 3 --> 1`.
   - Sum of weights:
     `Weight_cycle = w(1, 2) + w(2, 3) + w(3, 1) = (+3) + (+2) + (-6) = 5 - 6 = -1`.
   - Since `Weight_cycle = -1 < 0`, **YES, A NEGATIVE WEIGHT CYCLE EXISTS**.
2. **Bellman-Ford Detection Mechanism:**
   - In a graph with `V` vertices without negative cycles, every shortest simple path has at most `V - 1` edges.
   - Bellman-Ford relaxes all edges `V - 1` times.
   - On the `V`-th relaxation step, if `dist[v] > dist[u] + w(u, v)` holds true for any edge `(u, v)`, it proves that traversing the cycle further reduces the path cost toward negative infinity (`-infinity`). The algorithm flags `Negative Weight Cycle Detected` and aborts.

---

## PROBLEM 17: Token Bucket Traffic Shaping Burst Duration [Unit 4]
**Exam Problem Statement:**
> A computer network uses a token bucket to shape traffic. The token bucket capacity is `b = 2 Megabytes`. Tokens arrive at a rate of `r = 10 MB/sec`. The network link has a maximum transmission capacity of `M = 50 MB/sec`.
> 1. Calculate the maximum burst duration (`S`) during which the computer can transmit at the peak rate of 50 MB/sec.
> 2. Calculate the total data volume transmitted during this burst.

**Step-by-Step Solution:**
1. **Given Data:**
   - Bucket capacity `b = 2 MB`
   - Token arrival rate `r = 10 MB/s`
   - Peak rate `M = 50 MB/s`
2. **Burst Duration Formula:**
   During a burst of duration `S`, total tokens consumed equals initial tokens plus tokens arriving:
   `Consumed = S * M`
   `Available = b + S * r`
   `S * M = b + S * r ==> S * (M - r) = b ==> S = b / (M - r)`.
3. **Calculate S:**
   `S = (2 MB) / (50 MB/s - 10 MB/s) = 2 / 40 = 0.05 seconds = 50 ms`.
4. **Total Data Volume Transmitted:**
   `Volume V = S * M = 0.05 s * 50 MB/s = 2.5 Megabytes (2.5 MB)`.

---

## PROBLEM 18: TCP Jacobson's RTO Calculation [Unit 5]
**Exam Problem Statement:**
> An established TCP connection has an estimated `SRTT = 100 ms` and `RTTVAR = 20 ms`.
> 1. What is the current Retransmission Timeout (RTO)?
> 2. A new acknowledgment arrives with a measured round-trip time `RTT_m = 140 ms`. Using standard Jacobson parameters (`alpha = 0.125`, `beta = 0.25`), calculate the updated `SRTT`, `RTTVAR`, and new `RTO`.

**Step-by-Step Solution:**
1. **Current RTO:**
   `RTO_initial = SRTT + 4 * RTTVAR = 100 + 4 * (20) = 100 + 80 = 180 ms`.
2. **Update with New Measurement `RTT_m = 140 ms`:**
   - **Update SRTT**:
     `SRTT_new = (1 - alpha) * SRTT_old + alpha * RTT_m`
     `SRTT_new = (1 - 0.125) * 100 + 0.125 * 140 = 0.875 * 100 + 17.5 = 87.5 + 17.5 = 105 ms`.
   - **Update RTTVAR**:
     `RTTVAR_new = (1 - beta) * RTTVAR_old + beta * |SRTT_old - RTT_m|`
     `|100 - 140| = 40 ms`.
     `RTTVAR_new = (1 - 0.25) * 20 + 0.25 * (40) = 0.75 * 20 + 10 = 15 + 10 = 25 ms`.
   - **Calculate New RTO**:
     `RTO_new = SRTT_new + 4 * RTTVAR_new = 105 + 4 * (25) = 105 + 100 = 205 ms`.

---

## PROBLEM 19: TCP AIMD Congestion Window Trace [Unit 5]
**Exam Problem Statement:**
> A TCP Reno sender has an `MSS = 1 KB`. The initial `ssthresh` is set to `16 KB`. The sender starts in Slow Start with `cwnd = 1 KB`.
> Trace the value of `cwnd` across transmission rounds (RTTs) up to Round 10, assuming a packet loss due to **triple duplicate ACKs** occurs during Round 7.

**Step-by-Step Solution:**
1. **Trace Table:**

| Round (RTT) | State | cwnd at Start | Action Taken |
| :---: | :---: | :---: | :---: |
| **1** | Slow Start | **1 KB** | cwnd doubles (receives 1 ACK) |
| **2** | Slow Start | **2 KB** | cwnd doubles |
| **3** | Slow Start | **4 KB** | cwnd doubles |
| **4** | Slow Start | **8 KB** | cwnd doubles |
| **5** | Slow Start | **16 KB** | Reaches `ssthresh (16 KB)`! Switches to Congestion Avoidance |
| **6** | Congestion Avoidance | **17 KB** | cwnd grows linearly (+1 KB) |
| **7** | Congestion Avoidance | **18 KB** | **3 Dup ACKs Detected!** Loss event |
| **8** | Fast Recovery (Reno) | **9 KB** | `ssthresh = cwnd/2 = 9 KB`; `cwnd = ssthresh = 9 KB` |
| **9** | Congestion Avoidance | **10 KB** | cwnd grows linearly (+1 KB) |
| **10** | Congestion Avoidance | **11 KB** | cwnd grows linearly (+1 KB) |

* **Contrast with TCP Tahoe**: In TCP Tahoe, Round 8 would drop `cwnd = 1 KB` and restart Slow Start. TCP Reno stays in Congestion Avoidance at `9 KB`!

---

## PROBLEM 20: Base64 MIME Encoding Expansion [Unit 6]
**Exam Problem Statement:**
> An email attachment is an image of exact size 150,000 Bytes.
> 1. Calculate the size of the image after Base64 encoding for MIME transmission.
> 2. How many padding characters are appended?
> 3. If an email server has a 20 MB message size limit, what is the maximum raw attachment size that can be safely sent?

**Step-by-Step Solution:**
1. **Calculate Encoded Size:**
   - Base64 maps 3 raw bytes to 4 ASCII characters:
     `Encoded Size = ceil(150,000 / 3) * 4 = 50,000 * 4 = 200,000 Bytes`.
   - Size overhead = `200,000 - 150,000 = +50,000 Bytes (+33.33%)`.
2. **Padding Characters:**
   - `150,000 mod 3 = 0`.
   - Since the byte count is an exact multiple of 3, **0 padding characters** (`=`) are needed!
3. **Max Raw Attachment for 20 MB Limit:**
   `Max Encoded = 20 MB = 20 x 10^6 Bytes`.
   `Max Raw = (3 / 4) * 20 MB = 15 Megabytes (15 MB)`.

---

## PROBLEM 21: Non-Persistent vs Persistent HTTP Download Latency [Unit 6]
**Exam Problem Statement:**
> A web browser requests an HTML base page that references 9 embedded images stored on the same server. The one-way propagation delay between client and server is `RTT / 2 = 15 ms` (`RTT = 30 ms`). The transmission time of the HTML page is 10 ms, and each image requires 5 ms transmission time.
> 1. Calculate the total elapsed time using Non-Persistent HTTP (HTTP/1.0, serial).
> 2. Calculate the total elapsed time using Persistent HTTP with Pipelining (HTTP/1.1).

**Step-by-Step Solution:**
1. **Non-Persistent HTTP (HTTP/1.0):**
   - Each object requires a separate TCP connection:
     `Time per object = 1 RTT (TCP handshake) + 1 RTT (HTTP request/response) + T_trans = 2 * RTT + T_trans`.
   - HTML base object: `2 * (30 ms) + 10 ms = 60 + 10 = 70 ms`.
   - 9 Images: `9 * [2 * (30 ms) + 5 ms] = 9 * (65 ms) = 585 ms`.
   - `Total Time = 70 ms + 585 ms = 655 ms`.
2. **Persistent HTTP with Pipelining (HTTP/1.1):**
   - Single TCP connection opened for base HTML: `2 * RTT + T_trans,HTML = 60 + 10 = 70 ms`.
   - Once HTML is parsed, client pipelines all 9 image requests together over the existing open connection:
     `Time for all images = 1 RTT + Sum of transmission times = 30 ms + (9 * 5 ms) = 30 + 45 = 75 ms`.
   - `Total Time = 70 ms + 75 ms = 145 ms`.
* **Conclusion**: Persistent HTTP with Pipelining is **4.5 times faster** (`145 ms vs 655 ms`)!

---

## PROBLEM 22: DHCP Lease Renewal Timers [Unit 6]
**Exam Problem Statement:**
> A laptop connects to the VIT campus Wi-Fi network at 9:00:00 AM and is assigned an IP address by a DHCP server with a lease duration of 8 hours (28,800 seconds).
> State the exact wall-clock time and protocol action for:
> 1. Timer T1 (Renewal Timer)
> 2. Timer T2 (Rebinding Timer)
> 3. Lease Expiration

**Step-by-Step Solution:**
1. **Timer T1 (50% of lease):**
   - `Duration = 0.50 * 8 hours = 4 hours`.
   - **Time: 1:00:00 PM**.
   - **Action**: Client sends a unicast `DHCPREQUEST` packet directly to the original DHCP server to extend the lease. If acknowledged (`DHCPACK`), timers reset.
2. **Timer T2 (87.5% of lease):**
   - `Duration = 0.875 * 8 hours = 7 hours`.
   - **Time: 4:00:00 PM**.
   - **Action**: If original server did not reply to T1, client broadcasts a `DHCPREQUEST` to **any** active DHCP server on the local subnet to renew the IP.
3. **Lease Expiration (100% of lease):**
   - `Duration = 8 hours`.
   - **Time: 5:00:00 PM**.
   - **Action**: Lease expires. Client must immediately cease using the IP address, unbind the network adapter, and restart the full 4-step DORA process (`DHCPDISCOVER`).

---
---

## Summary Checklist of 10 Critical Exam Pitfalls:
1. **BDP Unit Trap**: `BDP` is in bits (`Bandwidth * RTT`). Divide by 8 to get Bytes!
2. **Subnetting Host Count Trap**: Always subtract 2 for usable hosts (`2^h - 2`).
3. **Fragment Offset Unit Trap**: Always divide byte offset by 8 (`Offset = Byte Position / 8`).
4. **Hamming Code Index Trap**: Parity bits are placed strictly at bit indices 1, 2, 4, 8... (not 0, 1, 2...).
5. **CRC Zero Padding Trap**: The number of zeros appended is equal to the **degree** `r` of `G(x)`, which is `(Number of bits in G) - 1`.
6. **CSMA/CD Factor of 2 Trap**: The transmission time must be at least twice the one-way propagation delay (`T_trans >= 2 * T_prop`), because the collision signal must travel all the way back.
7. **Pure vs Slotted ALOHA Factor Trap**: Pure ALOHA has `e^(-2G)` and max `18.4%`; Slotted ALOHA has `e^(-G)` and max `36.8%`.
8. **TCP Tahoe vs Reno Fast Recovery Trap**: On 3 duplicate ACKs, TCP Reno does NOT drop `cwnd = 1`. It sets `cwnd = ssthresh` and stays in Congestion Avoidance.
9. **Base64 Overhead Trap**: Base64 increases file size by `+33.33%` (not 25%).
10. **Active vs Passive FTP Trap**: In Active FTP, the server initiates the data connection from Port 20 to the client's high-numbered port.
