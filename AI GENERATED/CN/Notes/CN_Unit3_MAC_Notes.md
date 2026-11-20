# CN Unit III: Medium Access Control (MAC Sublayer) — Complete Exam Notes

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Sources Used**: `CN SYLLABUS.pdf`, `CN Endsem Revision (CS3213).pdf`, `IT2266_CN-ESE-QUESTION-PAPER.pdf`, Tanenbaum 5th Ed, Forouzan 5th Ed  
**Mathematical & Protocol Notation**: Universal Unicode text symbols (no raw LaTeX tags).

---

## 1. The Channel Allocation Problem: Static vs Dynamic

When multiple transmitting stations share a single broadcast communication channel, a systematic protocol is required to determine who gets to transmit next and prevent collisions.

### 1.1 Static Channel Allocation (FDM and TDM)
- **FDM (Frequency Division Multiplexing)**: The total channel bandwidth is divided into fixed frequency bands, one dedicated to each station.
- **TDM (Time Division Multiplexing)**: Time is divided into recurring fixed time slots, one assigned to each station.
- **The Critical Flaw for Computer Networks**:
  - Computer data traffic is inherently **bursty** (peak-to-average transmission ratio can be 1000:1).
  - If a station has no data to send, its allocated frequency band or time slot remains completely idle and wasted.
  - If many stations become active simultaneously, they suffer massive delays even while other channels sit empty.
  - By queueing theory, dividing a single channel of capacity `C` among `N` independent users increases the average packet delay by a factor of `N`: `T_FDM = N · T_shared`.

### 1.2 Dynamic Channel Allocation
In dynamic channel allocation, the channel is shared on an on-demand basis. Five key assumptions characterize multiple access systems:
1. **Station Model**: `N` independent stations generate frames for transmission.
2. **Single Shared Channel**: All stations communicate over a single shared physical medium.
3. **Collision Assumption**: If two frames are transmitted concurrently, they overlap in time and their signals interfere destructively. Both frames are garbled and ruined (a collision).
4. **Time Model**: Continuous time (transmission can begin at any instant, e.g., Pure ALOHA) or Slotted time (transmission begins only at discrete time slot boundaries, e.g., Slotted ALOHA).
5. **Carrier Sensing**: Stations can listen to the channel before transmitting (e.g., CSMA), or stations cannot sense the channel and transmit blindly (e.g., ALOHA).

---

## 2. Multiple Access Protocols: The ALOHA System

Developed in 1970 by Norman Abramson at the University of Hawaii for ground-based packet radio broadcasting.

### 2.1 Pure ALOHA
- **Basic Principle**: Stations transmit whenever they have a frame ready to send. There is no listening to the channel beforehand ("talk whenever you want").
- **Collision Feedback**: The sender listens for an acknowledgment or listens to its own transmission. If a collision occurs, the sender waits a random amount of time and retransmits.
- **Vulnerable Period**:
  Let `T_frame` be the transmission time of a standard frame.
  If station A begins transmitting a frame at time `t0`:
  - Any other station that began transmitting between `t0 - T_frame` and `t0` will collide with the beginning of A's frame.
  - Any other station that begins transmitting between `t0` and `t0 + T_frame` will collide with the end of A's frame.
  - Therefore, the **vulnerable period is twice the frame transmission time**:
    ```
    Vulnerable Period = 2 · T_frame
    ```
- **Throughput Derivation**:
  Let `G` be the total offered traffic load (average number of frame transmission attempts per frame time).
  By the Poisson distribution, the probability that `k` frames are generated during a time interval of duration `2 · T_frame` is:
  ```
  P[k] = ((2G)^k · e^(-2G)) / k!
  ```
  A frame transmission is successful if and only if **zero other frames** are generated during the vulnerable period (`k = 0`):
  ```
  P[0] = e^(-2G)
  ```
  The throughput `S` (rate of successful transmissions per frame time) is:
  ```
  S = G · P[0] = G · e^(-2G)
  ```
- **Maximum Throughput**:
  Differentiating `S` with respect to `G` and setting to zero (`dS/dG = 0`):
  `e^(-2G) - 2G · e^(-2G) = 0 ⟹ 1 - 2G = 0 ⟹ G = 0.5`.
  Substituting `G = 0.5`:
  ```
  S_max = 0.5 · e^(-1) = 1 / (2e) ≈ 0.184 (18.4%)
  ```
  *Conclusion*: In Pure ALOHA, the maximum possible channel utilization is only **18.4%**.

---

### 2.2 Slotted ALOHA
- **Basic Principle**: Time is divided into discrete intervals called **slots**, each exactly equal to the frame transmission time `T_frame`.
- Stations are synchronized by a master clock pulse. A station with a frame ready cannot transmit immediately; it must wait until the **beginning of the next time slot**.
- **Vulnerable Period**:
  Since frames can only start at slot boundaries, if another station wants to transmit, it must transmit in the exact same slot.
  ```
  Vulnerable Period = T_frame   (Cut in half compared to Pure ALOHA!)
  ```
- **Throughput Derivation**:
  The probability of zero other frame arrivals during a single slot time `T_frame` is `P[0] = e^(-G)`.
  The throughput `S` is:
  ```
  S = G · e^(-G)
  ```
- **Maximum Throughput**:
  Setting `dS/dG = 0`:
  `e^(-G) - G · e^(-G) = 0 ⟹ 1 - G = 0 ⟹ G = 1.0`.
  Substituting `G = 1.0`:
  ```
  S_max = 1.0 · e^(-1) = 1 / e ≈ 0.368 (36.8%)
  ```
  *Conclusion*: Slotted ALOHA **doubles the efficiency** of Pure ALOHA to **36.8%**, but requires global time synchronization.

---

## 3. Carrier Sense Multiple Access (CSMA) Protocols

ALOHA protocols suffer from high collision rates because stations transmit blindly without checking channel activity. CSMA protocols introduce **Carrier Sensing**:
> **"Listen before Talk"**: A station senses the physical transmission medium (carrier) before attempting to transmit.

### 3.1 1-Persistent CSMA
- **Algorithm**:
  1. Sense the channel.
  2. If the channel is **idle**, transmit immediately (with probability 1, hence "1-persistent").
  3. If the channel is **busy**, continue listening continuously. The instant the channel becomes idle, transmit immediately.
  4. If a collision occurs, wait a random amount of time and start over.
- **Weakness**: If two or more stations become ready while a third station is transmitting, both wait and sense the channel become idle at the exact same moment. Both transmit immediately, causing an **inevitable collision**.

### 3.2 Non-Persistent CSMA
- **Algorithm**:
  1. Sense the channel.
  2. If the channel is **idle**, transmit immediately.
  3. If the channel is **busy**, do NOT keep listening continuously. Instead, wait a **random back-off time** and sense the channel again.
- **Advantage & Tradeoff**: Dramatically reduces collisions because waiting stations draw different random delays and will not transmit at the same time. However, the channel may sit completely idle even when stations have data waiting, leading to longer packet delays.

### 3.3 p-Persistent CSMA (Used in Slotted Channels)
- **Algorithm**:
  1. When a station is ready and the channel is sensed idle:
     - Transmit with probability `p`.
     - Defer to the next time slot with probability `q = 1 - p`.
  2. In the next slot, if the channel is still idle, repeat the decision (transmit with probability `p` or defer).
  3. If the channel becomes busy during deferral, back off as if a collision occurred.
- **Tuning**: Parameter `p` is mathematically optimized. If `N` stations have frames to send, setting `N · p < 1` avoids collisions while keeping idle slots minimal.

---

## 4. CSMA with Collision Detection (CSMA/CD)

Used in legacy shared-medium Ethernet (IEEE 802.3).
> **"Listen before Talk, Keep Listening while Talking, Abort Immediately upon Collision"**.

### 4.1 The Need for Collision Detection
In standard CSMA, if two frames collide, both stations continue transmitting their entire frames to the bitter end, wasting valuable channel time and bandwidth. CSMA/CD adds real-time physical collision detection:
1. Station senses medium; if idle, starts transmitting.
2. Station continues to monitor the channel **while transmitting** (comparing outgoing signal voltage with line voltage).
3. If the line voltage exceeds a threshold (indicating destructive interference from another transmission), a **collision is detected**.
4. The station immediately **aborts frame transmission**, broadcasts a **32-bit JAM signal** to ensure all other stations detect the collision, and initiates exponential back-off.

---

### 4.2 Minimum Frame Size Derivation (The 2 · T_prop Rule)

#### The Worst-Case Collision Scenario:
Consider two stations, `A` and `B`, separated by maximum cable length with one-way propagation delay `T_prop`:
1. At time `t = 0`, Station A begins transmitting a frame.
2. The leading edge of A's signal propagates along the wire toward B, reaching B at time `t = T_prop - ε` (just an infinitesimal instant before reaching B).
3. At time `t = T_prop - ε`, Station B senses the wire. Since A's signal has not quite arrived, B senses the channel as idle and begins transmitting!
4. A collision occurs immediately near B at time `t = T_prop`.
5. The collision signal (runt waveform) must now propagate all the way back across the entire cable length to Station A, which takes another `T_prop` time.
6. Station A detects the collision at time:
   ```
   t = 2 · T_prop
   ```

#### The Golden Requirement:
To guarantee that Station A realizes its frame collided, **Station A must still be actively transmitting its frame when the collision signal arrives back at A**!
If A finishes transmitting before the collision signal returns, A will erroneously assume its frame was delivered successfully!

#### The Formula:
```
Frame Transmission Time  ≥  2 · One-Way Propagation Delay
       T_trans           ≥  2 · T_prop
```
Since `T_trans = L_min / Bandwidth`:
```
L_min / Bandwidth  ≥  2 · T_prop
L_min              ≥  2 · T_prop · Bandwidth
```

#### Standard Ethernet Numerical Values:
- In classic 10 Mbps Ethernet: Maximum cable network length is 2500 meters (with 4 repeaters).
- Round-trip propagation time: `2 · T_prop ≈ 46.4 μs + repeater delays ≈ 50 μs`.
- IEEE 802.3 defined the **Slot Time** = `51.2 μs` (corresponding to 512 bit times at 10 Mbps).
- **Minimum Frame Size**:
  ```
  L_min = 512 bits = 64 Bytes
  ```
  Any frame shorter than 64 bytes is called a **collision fragment** or **runt frame** and is automatically discarded by network hardware.

---

### 4.3 Binary Exponential Back-off (BEB) Algorithm
When a collision occurs, stations must wait before retrying to prevent immediate re-collision.

#### Step-by-Step Procedure:
1. Initialize collision counter `n = 0`.
2. Upon collision, transmit 32-bit Jam signal and increment `n = n + 1`.
3. If `n > 16`, abort transmission and report a **Network Congestion / Excessive Collision Error**.
4. Set back-off ceiling: `k = min(n, 10)`.
5. Pick an integer `K` uniformly at random from the discrete range:
   ```
   0 ≤ K ≤ 2^k - 1
   ```
6. The station waits a back-off delay:
   ```
   Wait Time = K · Slot Time   (where Slot Time = 51.2 μs in 10 Mbps Ethernet)
   ```
7. After waiting, the station attempts transmission again.

#### Range Progression:
- Collision 1 (`n=1`): `k=1`, `K ∈ {0, 1}`, wait 0 or 51.2 μs.
- Collision 2 (`n=2`): `k=2`, `K ∈ {0, 1, 2, 3}`, wait up to 153.6 μs.
- Collision 3 (`n=3`): `k=3`, `K ∈ {0..7}`.
- ...
- Collision 10 (`n=10`): `k=10`, `K ∈ {0..1023}`, wait up to 52.4 ms.
- Collisions 11 to 16: Range stays frozen at `[0, 1023]`.
- *Why Exponential?*: If few stations collide, the range is small to minimize idle delay. If many stations collide, the range expands exponentially to disperse competing traffic and resolve contention.

---

## 5. IEEE 802.3 Ethernet Standards & Frame Formats

### 5.1 Ethernet Frame Format (DIX Ethernet II vs IEEE 802.3)
```text
+----------+------+----------+----------+----------+-----------------------+----------+
| PREAMBLE | SFD  | DEST MAC | SRC MAC  | TYPE/LEN |     PAYLOAD (DATA)    |   FCS    |
| (7 Bytes)| (1B) | (6 Bytes)| (6 Bytes)| (2 Bytes)|   (46 to 1500 Bytes)  | (4 Bytes)|
+----------+------+----------+----------+----------+-----------------------+----------+
|<-- Physical Overhead -->|<---------------- 64 to 1518 Bytes ------------------------->|
```

1. **Preamble (7 Bytes)**: Pattern of alternating `10101010` bytes used to synchronize the receiver's physical clock with the incoming bit stream.
2. **SFD (Start Frame Delimiter - 1 Byte)**: Pattern `10101011`. The final two consecutive `11` bits signal that the destination MAC address begins immediately on the next bit.
3. **Destination MAC (6 Bytes)**: 48-bit hardware address of the intended receiving station, multicast group, or broadcast (`FF:FF:FF:FF:FF:FF`).
4. **Source MAC (6 Bytes)**: 48-bit hardware address of the transmitting network card.
5. **Type / Length Field (2 Bytes)**:
   - If value `≤ 1500` (decimal): Interpreted as **Length** (IEEE 802.3 standard; indicates number of bytes in data payload).
   - If value `≥ 1536` (`0x0600` hex): Interpreted as **Type** (DIX Ethernet II standard; identifies Network Layer protocol, e.g., `0x0800` for IPv4, `0x0806` for ARP).
6. **Data Payload (46 to 1500 Bytes)**:
   - Contains the Network Layer packet.
   - If payload is smaller than 46 bytes, **Padding bytes** are appended to bring the frame up to the mandatory **64-byte minimum frame size**.
7. **FCS (Frame Check Sequence - 4 Bytes)**: Contains a 32-bit CRC (CRC-32) computed over Dest MAC, Source MAC, Type/Length, and Data fields.

---

### 5.2 High-Speed Ethernet Evolution

| Standard | Speed | Media & Cable Type | Maximum Distance | Encoding Scheme | Duplex & CSMA/CD Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **10BASE-T** | 10 Mbps | Cat 3 / Cat 5 UTP | 100 m | Manchester | Half/Full duplex; CSMA/CD on shared hub |
| **100BASE-TX** (Fast Ethernet) | 100 Mbps | Cat 5 UTP (2 pairs) | 100 m | 4B/5B + MLT-3 | Full duplex standard; autonegotiation |
| **1000BASE-T** (Gigabit Ethernet) | 1000 Mbps | Cat 5e / Cat 6 UTP (4 pairs) | 100 m | 4D-PAM5 (5 voltage levels) | Strictly switched full-duplex; CSMA/CD obsolete |
| **10GBASE-T** (10-Gigabit) | 10 Gbps | Cat 6A / Cat 7 UTP | 100 m | 64B/66B + PAM-16 | Strictly full-duplex; NO CSMA/CD support |

> **IMPORTANT EXAM NOTE ON GIGABIT ETHERNET**:
> In half-duplex Gigabit Ethernet, maintaining the 64-byte minimum frame size would have shrunk the maximum cable length to an impractical 10 meters!
> IEEE solved this using two techniques:
> 1. **Carrier Extension**: Padding frames out to 512 bytes on the wire.
> 2. **Frame Bursting**: Transmitting multiple small frames in a single burst without releasing the channel.
> In practice, Gigabit Ethernet is deployed exclusively as **full-duplex switched Ethernet**, completely eliminating CSMA/CD and collisions.

---

## 6. Wireless LANs (IEEE 802.11) & CSMA/CA

### 6.1 Why CSMA/CD Cannot Be Used in Wireless Networks
Collision detection (CSMA/CD) works on wired copper cables because the signal voltage is relatively uniform across the wire. In wireless communication:
1. **Huge Signal Dynamic Range (Self-Interference)**:
   A station's own transmitter emits high radio power (e.g., +20 dBm). Incoming signals from other distant transmitting stations arrive severely attenuated by inverse-square law ($1/d^2$ or $1/d^4$, e.g., -80 dBm). The station's own transmission completely drowns out any colliding signal. Transceivers cannot transmit and listen at the same frequency simultaneously.
2. **The Hidden Terminal Problem**:
   Station A transmits to Station B. Station C is out of radio range of A, but in range of B. When C senses the medium, it hears silence (A is hidden from C). C begins transmitting to B, destroying A's frame at B!
3. **The Exposed Terminal Problem**:
   Station B is transmitting to Station A. Station C wants to transmit to Station D (where D is out of range of B). C senses B's transmission and wrongly concludes the channel is busy, unnecessarily postponing transmission to D even though no collision would occur at A or D.

---

### 6.2 CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)
IEEE 802.11 avoids collisions using a combination of **Inter-Frame Spaces (IFS)** and **Virtual Carrier Sensing (RTS/CTS)**.

#### 1. Inter-Frame Spaces (Priority Hierarchy):
Before transmitting, a station senses the channel. If idle, it must wait for a designated quiet period:
- **SIFS (Short Inter-Frame Space)**: Shortest waiting time; has highest priority. Used for immediate control frames: **ACK**, **CTS**, and polling responses.
- **PIFS (PCF Inter-Frame Space)**: Medium waiting time; used in centralized contention-free polling.
- **DIFS (Distributed Inter-Frame Space)**: Longest waiting time; used for standard asynchronous data frames. Stations wanting to send normal data must wait until the channel has been continuously idle for at least a full DIFS period.

#### 2. Virtual Carrier Sensing: The RTS/CTS Handshake:
To solve the hidden terminal problem, IEEE 802.11 uses small four-way handshake control frames:
1. Sender A waits for DIFS, then transmits a small **RTS (Request to Send)** frame to Access Point B. The RTS header contains a **Duration Field** specifying the total time required to transmit the pending data frame and receive its ACK.
2. Receiver B replies after SIFS with a **CTS (Clear to Send)** frame, broadcasting the identical duration value.
3. **Setting the NAV (Network Allocation Vector)**:
   All surrounding stations (including hidden station C) hear the CTS frame broadcast by B.
   Even though C cannot hear A, C reads the Duration field in the CTS and sets an internal countdown timer called the **NAV (Network Allocation Vector)**.
   Station C treats the channel as virtually busy and remains completely silent until the NAV timer reaches zero!
4. Sender A transmits its large **DATA Frame**.
5. Receiver B verifies CRC and returns an explicit **ACK Frame** after SIFS.
