# CN Unit III: Medium Access Control (MAC Sublayer) — Complete Solutions & Marking Scheme

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Protocol Notation**: Universal Unicode text symbols.

---

## Section A Solutions (4 Marks Each)

### Q1 Solution: Pure ALOHA vs Slotted ALOHA
**Marking Scheme**:
- Comparison across all 4 parameters: 4 Marks (1M per row)

**Answer**:

| Parameter | Pure ALOHA | Slotted ALOHA |
| :--- | :--- | :--- |
| **Transmission Timing** | Stations transmit **immediately** whenever a frame is ready (continuous time). | Stations transmit **only at the beginning of discrete time slots**. |
| **Vulnerable Period** | `2 · T_frame` (from `t0 - T_frame` to `t0 + T_frame`). | `1 · T_frame` (exactly one time slot). |
| **Throughput Equation** | `S = G · e^(-2G)` | `S = G · e^(-G)` |
| **Maximum Efficiency** | **18.4%** (`1 / (2e)`) at offered load `G = 0.5`. | **36.8%** (`1 / e`) at offered load `G = 1.0` (exactly double!). |

---

### Q2 Solution: CSMA Persistence Variations
**Marking Scheme**:
- 1-Persistent CSMA: 1.5 Marks
- Non-Persistent CSMA: 1.5 Marks
- p-Persistent CSMA: 1 Mark

**Answer**:
1. **1-Persistent CSMA**:
   - Senses channel. If idle, transmits immediately with probability 1. If busy, listens continuously until the channel becomes idle, then transmits immediately.
   - *Limitation*: High collision probability because multiple waiting stations detect idle simultaneously and collide.
2. **Non-Persistent CSMA**:
   - Senses channel. If idle, transmits immediately. If busy, does NOT keep listening; waits a **random back-off delay** before sensing again.
   - *Advantage*: Dramatically reduces collisions. *Drawback*: Channel sits idle while stations are in back-off.
3. **p-Persistent CSMA**:
   - Used in slotted channels. Senses channel. If idle, transmits with probability `p`, or defers to the next slot with probability `1 - p`. In the next slot, repeats the decision. If channel becomes busy during deferral, backs off.
   - Balances collision avoidance with low channel idle time.

---

### Q3 Solution: Binary Exponential Back-off Calculations
**Marking Scheme**:
- (a) Calculation for 3rd collision and 12th collision: 2 Marks
- (b) Abort threshold and justification: 2 Marks

**Answer**:
**(a) Range of Random Integer K**:
After `n` collisions, `k = min(n, 10)` and `K` is chosen uniformly from `[0, 2^k - 1]`.
- **After 3rd Collision (`n = 3`)**:
  `k = min(3, 10) = 3`.
  Range: `0 ≤ K ≤ 2³ - 1 = 7` ⟹ `K ∈ {0, 1, 2, 3, 4, 5, 6, 7}`.
  Back-off Delay = `K × 51.2 μs` (maximum wait = `7 × 51.2 = 358.4 μs`).
- **After 12th Collision (`n = 12`)**:
  `k = min(12, 10) = 10` (capped at 10!).
  Range: `0 ≤ K ≤ 2¹⁰ - 1 = 1023` ⟹ `K ∈ {0, 1, ..., 1023}`.
  Back-off Delay = `K × 51.2 μs` (maximum wait = `1023 × 51.2 = 52.38 ms`).

**(b) Abort Threshold**:
- The algorithm attempts retransmission at most **16 times** (`n = 16`).
- If collision occurs on the 16th attempt, the station **aborts transmission** and reports an "Excessive Collisions / Network Congestion Error" to the upper layers.
- *Reason*: 16 consecutive collisions indicates persistent network congestion or a damaged cable (short circuit / babbling node). Retrying indefinitely would deadlock the station's outgoing queue.

---

### Q4 Solution: Why CSMA/CD Fails in Wireless Networks
**Marking Scheme**:
- Reason 1: Self-Interference / Signal attenuation: 2 Marks
- Reason 2: Hidden Terminal Problem: 2 Marks

**Answer**:
1. **Self-Interference (Large Dynamic Signal Range)**:
   - On a wired copper cable, transmitted and received signal voltages are comparable.
   - In wireless media, radio signals attenuate drastically over distance according to the inverse-square law ($1/d^2$ or $1/d^4$). A station's own transmitter outputs significant radio power (e.g., +20 dBm), while an incoming signal from a colliding station arrives at minuscule power (e.g., -80 dBm).
   - The station's own high-power transmission completely drowns out any weak incoming collision echo. Transceivers cannot transmit and detect collisions simultaneously on the same channel.
2. **The Hidden Terminal Problem**:
   - Collision detection requires that all colliding stations hear each other.
   - In wireless topologies, two stations (A and C) may both be within range of an Access Point (B), but out of range of each other.
   - Neither station can detect that the other is transmitting (A is "hidden" from C). Therefore, collision detection at the transmitter cannot function.

---

## Section B Solutions (8 Marks Each)

### Q5 Solution: CSMA/CD Minimum Frame Condition Proof
**Marking Scheme**:
- (a) Proof and timeline diagram showing worst-case collision: 5 Marks
- (b) Numerical calculation for 10 Mbps Ethernet: 3 Marks

**Answer**:
**(a) Proof that T_trans ≥ 2 · T_prop**:
Consider two stations, A and B, at opposite ends of a bus with one-way propagation time `T_prop`:
1. At `t = 0`, Station A senses the medium idle and begins transmitting its frame.
2. The leading edge of A's signal travels toward B, arriving at `t = T_prop - ε`.
3. An infinitesimal instant before A's signal arrives (`t = T_prop - ε`), Station B senses the wire. Sensing it idle, B begins transmitting!
4. A collision occurs immediately near Station B at `t = T_prop`.
5. The collision signal (runt waveform) must now travel back across the entire cable length to reach Station A. This takes another `T_prop` time.
6. Station A detects the collision at time:
   ```
   t_detect = T_prop + T_prop = 2 · T_prop
   ```
7. To reliably detect this collision, Station A must **still be actively transmitting its frame** at time `t = 2 · T_prop`. If A finished transmitting earlier, it would release its transmission buffer and wrongly conclude that the frame was successfully delivered.
8. Therefore, the frame transmission duration `T_trans` must be at least the round-trip propagation time:
   ```
   T_trans ≥ 2 · T_prop
   ```
   Since `T_trans = L_min / Bandwidth`:
   ```
   L_min ≥ 2 · T_prop · Bandwidth
   ```

**(b) Numerical Calculation for 10 Mbps Ethernet**:
Given:
- Bandwidth `B = 10 Mbps = 10 × 10⁶ bits/sec`
- One-way propagation delay `T_prop = 25.6 μs = 25.6 × 10⁻⁶ sec`

1. **Minimum Frame Size in Bits**:
   ```
   L_min = 2 · T_prop · B = 2 · (25.6 × 10⁻⁶ sec) · (10 × 10⁶ bits/sec)
         = 51.2 × 10 = 512 bits
   ```
2. **Minimum Frame Size in Bytes**:
   ```
   L_min_bytes = 512 bits / 8 = 64 Bytes
   ```
   *(Matches the exact standard IEEE 802.3 minimum Ethernet frame size!).*

---

### Q6 Solution: IEEE 802.3 Ethernet Frame Format
**Marking Scheme**:
- 1. Preamble and SFD explanation: 2 Marks
- 2. Dest and Source MAC explanation: 2 Marks
- 3. Type/Length field distinction: 2 Marks
- 4. Data payload and 64-byte minimum frame padding: 2 Marks

**Answer**:
```text
+----------+------+----------+----------+----------+-----------------------+----------+
| PREAMBLE | SFD  | DEST MAC | SRC MAC  | TYPE/LEN |     PAYLOAD (DATA)    |   FCS    |
| (7 Bytes)| (1B) | (6 Bytes)| (6 Bytes)| (2 Bytes)|   (46 to 1500 Bytes)  | (4 Bytes)|
+----------+------+----------+----------+----------+-----------------------+----------+
```

1. **Preamble (7 Bytes) & Start Frame Delimiter (SFD - 1 Byte)**:
   - **Preamble**: 7 bytes of alternating `10101010` bits used to synchronize the receiver's physical bit clock.
   - **SFD**: 1 byte with pattern `10101011`. The final two `11` bits signal the immediate start of the destination MAC address.
2. **Destination & Source MAC Addresses (6 Bytes each)**:
   - 48-bit hardware addresses. Destination can be unicast, multicast, or broadcast (`FF:FF:FF:FF:FF:FF`). Source is always the transmitting NIC's unicast address.
3. **Type / Length Field (2 Bytes)**:
   - **IEEE 802.3 Convention**: If value `≤ 1500` (decimal), it specifies the **Length** of the data field in bytes.
   - **DIX Ethernet II Convention**: If value `≥ 1536` (`0x0600` hex), it specifies the **EtherType** (Network layer protocol: `0x0800` for IPv4, `0x0806` for ARP, `0x86DD` for IPv6).
   - Receivers differentiate automatically because no Ethernet payload exceeds 1500 bytes.
4. **Data Payload (46 to 1500 Bytes) & Padding**:
   - Contains the Network layer packet.
   - The minimum frame size from Dest MAC to FCS is strictly **64 bytes**.
   - Subtracting 18 bytes of headers/trailer (6 + 6 + 2 + 4 = 18 B), the minimum payload is:
     ```
     Minimum Payload = 64 - 18 = 46 Bytes
     ```
   - If user data is less than 46 bytes, **Padding bytes** are appended to bring the frame up to 64 bytes to guarantee collision detection!

---

### Q7 Solution: Hidden & Exposed Terminal Problems & CSMA/CA RTS/CTS
**Marking Scheme**:
- 1. Diagram and explanation of Hidden and Exposed Terminal Problems: 3 Marks
- 2. RTS/CTS 4-way handshake resolution: 3 Marks
- 3. Role of NAV (Network Allocation Vector): 2 Marks

**Answer**:
**(1) Problem Descriptions**:
```text
Hidden Terminal:   [Station A] <---- radio range ----> [AP B] <---- radio range ----> [Station C]
                                                    (A and C cannot hear each other!)
Exposed Terminal:  [Station A] <--- [Station B]         [Station C] ---> [Station D]
                                (C hears B sending to A, falsely assumes it cannot send to D)
```
- **Hidden Terminal Problem**: Station A and Station C are both in range of B, but hidden from each other. When A transmits to B, C senses the channel idle and also transmits to B. The two transmissions collide destructively at B!
- **Exposed Terminal Problem**: Station C wants to send to D. It senses the channel and hears Station B transmitting to A. C needlessly defers transmission, even though C transmitting to D would not interfere with B's reception at A.

**(2) Resolution via RTS / CTS Handshake in CSMA/CA**:
1. Station A transmits a short **RTS (Request to Send)** frame to B containing a **Duration Field** covering the entire upcoming exchange.
2. Access Point B responds with a **CTS (Clear to Send)** frame, echoing the duration value.
3. Because CTS is transmitted by B, **Station C hears the CTS frame**!
4. Station C learns that B will be busy receiving for the specified duration and refrains from transmitting, successfully preventing the collision at B.

**(3) Role of the NAV (Network Allocation Vector)**:
- The NAV is an internal software countdown timer maintained by every 802.11 station.
- When any station overhears an RTS or CTS frame not addressed to itself, it reads the Duration field and sets its NAV timer to that duration.
- The station performs **Virtual Carrier Sensing**: as long as its NAV is non-zero, it treats the channel as 100% busy and stays quiet, even if physical carrier sensing detects silence!

---

### Q8 Solution: Evolution of Ethernet Standards
**Marking Scheme**:
- 1. Structured comparison of 10M, 100M, 1000M, 10G Ethernet: 4 Marks
- 2. Gigabit half-duplex dilemma (Carrier Extension & Frame Bursting): 4 Marks

**Answer**:
**(1) Standards Comparison Table**:

| Ethernet Standard | Data Rate | Physical Cable | Encoding Scheme | Duplex & CSMA/CD Status |
| :--- | :--- | :--- | :--- | :--- |
| **10BASE-T** | 10 Mbps | Cat 3 / Cat 5 UTP | Manchester | Half / Full Duplex; CSMA/CD on shared hubs |
| **100BASE-TX (Fast)** | 100 Mbps | Cat 5 UTP (2 pairs)| 4B/5B + MLT-3 | Full duplex standard; CSMA/CD optional |
| **1000BASE-T (Gigabit)**| 1000 Mbps | Cat 5e / 6 (4 pairs)| 4D-PAM5 (5 voltage levels)| Strictly full-duplex switched; CSMA/CD obsolete |
| **10GBASE-T (10-Gig)** | 10 Gbps | Cat 6A / Cat 7 UTP| 64B/66B + PAM-16 | Strictly full-duplex; **NO CSMA/CD support** |

**(2) Gigabit Ethernet Half-Duplex Dilemma & Solutions**:
- **The Dilemma**:
  To maintain CSMA/CD at 1000 Mbps while keeping the 64-byte minimum frame size:
  ```
  T_trans = 512 bits / 10⁹ bps = 0.512 μs
  Maximum Distance = (T_trans · c) / 2 = (0.512 × 10⁻⁶ · 2 × 10⁸) / 2 ≈ 51 meters
  ```
  With repeater delays, the maximum network cable diameter would shrink to an impractical **10 meters**!
- **Two Mechanisms Introduced to Fix This**:
  1. **Carrier Extension**: For half-duplex Gigabit Ethernet, the minimum carrier slot time was artificially expanded from 64 bytes to **512 bytes**. If a frame is smaller than 512 bytes, non-data extension symbols are appended on the wire to extend cable reach back to 100 meters.
  2. **Frame Bursting**: To avoid wasting bandwidth by extending every small frame to 512 bytes, a station that captures the channel is permitted to transmit a burst of multiple small frames back-to-back without releasing the channel.
- **Industry Resolution**: In practice, half-duplex Gigabit was never widely deployed. The industry moved entirely to **switched full-duplex Gigabit Ethernet**, where collisions are physically impossible and CSMA/CD was discarded.

---

## Section C Solutions (10 Marks Each)

### Q9 Solution: ALOHA Mathematical Throughput Derivations
**Marking Scheme**:
- (a) Pure ALOHA derivation, maximum throughput proof, and G value: 5 Marks
- (b) Slotted ALOHA derivation, maximum throughput proof, and vulnerable time comparison: 5 Marks

**Answer**:
**(a) Pure ALOHA Mathematical Derivation**:
Let `T` be the frame transmission time, and `G` be the total offered load (average number of frame arrival attempts per frame time `T`).
1. **Traffic Arrival Model**:
   Frame arrivals follow a Poisson distribution. The probability of generating `k` frames in a time interval of duration `t` is:
   ```
   P[k] = ((G · t / T)^k · e^(-G · t / T)) / k!
   ```
2. **Vulnerable Period**:
   In Pure ALOHA, a frame beginning transmission at time `t0` will collide if any other frame begins transmission in the window `[t0 - T, t0 + T]`.
   The duration of this vulnerable interval is `t_vuln = 2T`.
3. **Probability of Successful Transmission**:
   A transmission is successful if zero other frames are generated during `2T` (`k = 0, t = 2T`):
   ```
   P[0] = e^(-G · 2T / T) = e^(-2G)
   ```
4. **Throughput Equation `S`**:
   Throughput `S` is the rate of successful transmissions:
   ```
   S = G · P[0] = G · e^(-2G)
   ```
5. **Finding Maximum Throughput**:
   Take the derivative with respect to `G` and set to zero:
   ```
   dS/dG = e^(-2G) + G · (-2) · e^(-2G) = e^(-2G) · (1 - 2G) = 0
   ```
   Since `e^(-2G) ≠ 0`, `1 - 2G = 0 ⟹ G = 0.5`.
   Substituting `G = 0.5` into `S`:
   ```
   S_max = 0.5 · e^(-2 · 0.5) = 0.5 · e^(-1) = 1 / (2e) ≈ 0.1839 (18.4%)
   ```

**(b) Slotted ALOHA Mathematical Derivation**:
1. **Vulnerable Period**:
   In Slotted ALOHA, time is discretized into slots of length `T`. Transmissions start only at slot boundaries.
   A collision occurs only if two or more frames arrive during the **same single slot** preceding the transmission boundary.
   Therefore, the vulnerable period is reduced to `t_vuln = T` (half of Pure ALOHA).
2. **Probability of Success**:
   Zero other arrivals in duration `T` (`k = 0, t = T`):
   ```
   P[0] = e^(-G)
   ```
3. **Throughput Equation `S`**:
   ```
   S = G · P[0] = G · e^(-G)
   ```
4. **Finding Maximum Throughput**:
   ```
   dS/dG = e^(-G) - G · e^(-G) = e^(-G) · (1 - G) = 0 ⟹ G = 1.0
   ```
   Substituting `G = 1.0`:
   ```
   S_max = 1.0 · e^(-1) = 1 / e ≈ 0.3679 (36.8%)
   ```
   *Conclusion*: Restricting transmission starts to slot boundaries eliminates partial overlaps, cutting the vulnerable time in half and **doubling channel throughput** from 18.4% to 36.8%!

---

### Q10 Solution: CSMA/CD Network Design & Bandwidth Scaling
**Marking Scheme**:
- (a.1) One-way propagation delay: 1.5 Marks
- (a.2) Minimum frame size at 10 Mbps: 2 Marks
- (b.1) Frame sizes at 100 Mbps and 1 Gbps: 3 Marks
- (b.2) Practicality discussion and industry solution: 3.5 Marks

**Answer**:
Given:
- Cable length `d = 2 km = 2000 m`
- Propagation speed `v = 2 × 10⁸ m/s`

#### (a) 10 Mbps Operational Calculations:
1. **One-Way Propagation Delay `T_prop`**:
   ```
   T_prop = d / v = 2000 / (2 × 10⁸) = 10⁻⁵ sec = 10 μs
   ```
2. **Minimum Frame Size `L_min` at 10 Mbps**:
   Condition: `T_trans ≥ 2 · T_prop = 2 · 10 μs = 20 μs`.
   ```
   L_min = 2 · T_prop · Bandwidth
         = 2 · (10 × 10⁻⁶ sec) · (10 × 10⁶ bits/sec) = 200 bits = 25 Bytes
   ```
   *(Standard 64 bytes easily covers this requirement).*

#### (b) Bandwidth Scaling to 100 Mbps and 1 Gbps:
1. **Required Minimum Frame Sizes (keeping length = 2 km, T_prop = 10 μs)**:
   - **At 100 Mbps (Fast Ethernet)**:
     ```
     L_min = 2 · T_prop · Bandwidth = 2 · (10 μs) · 100 Mbps = 2000 bits = 250 Bytes
     ```
   - **At 1 Gbps (Gigabit Ethernet)**:
     ```
     L_min = 2 · T_prop · Bandwidth = 2 · (10 μs) · 1000 Mbps = 20,000 bits = 2500 Bytes
     ```
2. **Why Continuing CSMA/CD is Impractical & Industry Solution**:
   - At 1 Gbps, a minimum frame size of **2500 Bytes** would exceed the maximum standard MTU payload (1500 bytes)!
   - If a 64-byte TCP ACK packet were transmitted, it would have to be padded with over 2400 bytes of dummy data, causing massive bandwidth waste (efficiency < 3%).
   - Alternatively, keeping `L_min = 64 Bytes` would require shrinking the maximum cable length to just 20 meters!
   - **The Industry Solution**:
     The networking industry abandoned shared broadcast media and CSMA/CD entirely. Modern networks deploy **Switched Full-Duplex Ethernet**. Each station has a dedicated point-to-point link to a Layer-2 switch port. Stations transmit and receive simultaneously on separate physical wire pairs, completely eliminating packet collisions.

---

### Q11 Solution: Binary Exponential Back-off Simulation & Probability
**Marking Scheme**:
- 1. Range of K for collisions 1, 2, 3: 3 Marks
- 2. Collision probability on 1st retransmission: 2.5 Marks
- 3. Collision probability on 2nd retransmission: 2.5 Marks
- 4. Delay calculation and winning station: 2 Marks

**Answer**:
Given: 10 Mbps Ethernet, Slot Time = `51.2 μs`.

#### 1. Range of Integer K:
Formula: `K ∈ [0, 2^k - 1]` where `k = min(n, 10)`.
- **1st Retransmission (`n = 1`)**: `k = 1 ⟹ K ∈ {0, 1}` (2 possible choices).
- **2nd Retransmission (`n = 2`)**: `k = 2 ⟹ K ∈ {0, 1, 2, 3}` (4 possible choices).
- **3rd Retransmission (`n = 3`)**: `k = 3 ⟹ K ∈ {0, 1, 2, 3, 4, 5, 6, 7}` (8 possible choices).

#### 2. Probability of Collision on 1st Retransmission:
- Station A chooses `K_A ∈ {0, 1}` with equal probability `1/2`.
- Station B chooses `K_B ∈ {0, 1}` with equal probability `1/2`.
- Total possible pairs `(K_A, K_B)`: `2 × 2 = 4` equally likely outcomes.
- Collisions occur when `K_A = K_B`: Outcomes `(0, 0)` and `(1, 1)` (2 outcomes).
  ```
  P(Collision on 1st Retry) = 2 / 4 = 1/2 = 0.5 (50%)
  ```

#### 3. Probability of Collision on 2nd Retransmission:
- Each station independently chooses from `{0, 1, 2, 3}` (4 choices).
- Total pairs `(K_A, K_B)`: `4 × 4 = 16` outcomes.
- Collisions occur when `K_A = K_B`: `(0, 0), (1, 1), (2, 2), (3, 3)` (4 outcomes).
  ```
  P(Collision on 2nd Retry) = 4 / 16 = 1/4 = 0.25 (25%)
  ```

#### 4. Delay Calculation for K_A = 1 and K_B = 3:
- **Station A Wait Delay**:
  ```
  Delay_A = K_A · Slot Time = 1 · 51.2 μs = 51.2 μs
  ```
- **Station B Wait Delay**:
  ```
  Delay_B = K_B · Slot Time = 3 · 51.2 μs = 153.6 μs
  ```
- **Winning Station**:
  Station A finishes waiting after 51.2 μs and begins transmitting. When Station B's timer expires at 153.6 μs, B senses the carrier generated by A, finds the channel busy, and defers transmission.
  **Station A successfully captures the channel!**

---

### Q12 Solution: IEEE 802.11 CSMA/CA Architecture & Inter-Frame Spaces
**Marking Scheme**:
- (a) Complete CSMA/CA timing diagram with DIFS, RTS, SIFS, CTS, DATA, ACK, NAV: 5 Marks
- (b) Why SIFS < DIFS explanation: 3 Marks
- (c) Why Layer-2 ACK is mandatory in Wi-Fi vs Ethernet: 2 Marks

**Answer**:
**(a) CSMA/CA Timing Diagram**:
```text
Sender A:     [DIFS] [ RTS ]             [SIFS] [   DATA FRAME   ]
                |      |                   |          |
                v      |                   v          |
AP B:                  |   [SIFS] [ CTS ]             |   [SIFS] [ ACK ]
                       |     |      |                 |     |      |
                       +-----+      +-----------------+     +------+
                                    |
Hidden C:                           | <======== NAV (Timer) =======> |
                                      (Station C stays silent until NAV=0)
```

**(b) Why SIFS is Shorter than DIFS**:
- **Priority Hierarchy**: In CSMA/CA, a shorter waiting time confers higher medium access priority.
  `SIFS < PIFS < DIFS`.
- **Reason**:
  - `SIFS` (Short Inter-Frame Space) is used for ongoing dialog frames: **CTS**, **DATA**, and **ACK**.
  - Once a four-way handshake begins, no third-party station should be allowed to interrupt the ongoing transaction.
  - Because `SIFS < DIFS`, the receiver can send a CTS or ACK *before* any competing station waiting for a full `DIFS` period can finish its countdown and seize the channel!
- *What if SIFS were longer than DIFS?*: Competing stations waiting for DIFS would sense the channel idle during the pause, conclude the channel was free, and start transmitting new frames, constantly colliding with and destroying CTS, DATA, and ACK frames!

**(c) Why Layer-2 ACK is Mandatory in Wi-Fi vs Wired Ethernet**:
- **In Wired Ethernet**: Cable error rates are extremely low ($10^{-10}$), and transmitters use CSMA/CD to immediately know if a frame collided. If no collision occurred during the first 512 bit times, the frame arrived successfully. Layer-2 ACKs would waste bandwidth.
- **In Wireless Wi-Fi**:
  1. Transmitters cannot detect collisions over the air (no collision detection).
  2. Wireless channels have high error rates ($10^{-3}$ to $10^{-5}$) due to multipath fading, interference, and obstacles.
  3. The sender has **no other physical way of knowing** whether its frame arrived intact or was destroyed without an explicit Layer-2 ACK frame returned by the receiver.
