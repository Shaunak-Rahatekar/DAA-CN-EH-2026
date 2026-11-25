# Computer Networks (CS3025 / IT2266) — Actual ESE Papers Solutions Pack
## Bansilal Ramnath Agarwal Charitable Trust's Vishwakarma Institute of Technology (VIT Pune)

> **Source Documents Transcribed & Fully Solved:**
> 1. **Paper 1**: ESE IT2266 (Computer Networks) — **Tuesday, 12-Nov-2024 (Max Marks: 60)**
> 2. **Paper 2**: ESE CS3025 (Computer Networks) — **Friday, 3rd May 2019 (Max Marks: 100)**
> 
> **Purpose**: Exact reference solutions, numerical derivations, step-by-step algorithms, and examiner marking criteria for tomorrow's Computer Networks written examination.

---

# PART I: ESE NOVEMBER 2024 (IT2266 — 60 MARKS)

---

### Q.1a [2 Marks]
**Question:**
> Data Rate: $4\text{ Kbps}$. Propagation delay $= 20\text{ ms}$. Efficiency/Utilization $= 50\% = 0.5$. Find frame size?

**Step-by-Step Solution:**
1. **Given Data:**
   - Data Transmission Rate ($R$) $= 4\text{ Kbps} = 4,000\text{ bps}$
   - One-way Propagation Delay ($T_{\text{prop}}$) $= 20\text{ ms} = 0.020\text{ s}$
   - Channel Utilization / Efficiency ($\eta$) $= 50\% = 0.5$

2. **Governing Formula for Stop-and-Wait Protocol:**
   $$\eta = \frac{T_{\text{trans}}}{T_{\text{trans}} + 2 \cdot T_{\text{prop}}} = \frac{1}{1 + 2a} \quad \text{where } a = \frac{T_{\text{prop}}}{T_{\text{trans}}}$$

3. **Calculate parameter $a$ and $T_{\text{trans}}$:**
   $$\frac{1}{1 + 2a} = 0.5 \implies 1 + 2a = 2 \implies 2a = 1 \implies a = 0.5$$
   $$T_{\text{trans}} = \frac{T_{\text{prop}}}{a} = \frac{20\text{ ms}}{0.5} = 40\text{ ms} = 0.040\text{ seconds}$$

4. **Calculate Frame Size ($L$):**
   $$T_{\text{trans}} = \frac{L}{R} \implies L = T_{\text{trans}} \times R$$
   $$L = 0.040\text{ s} \times 4,000\text{ bps} = 160\text{ bits}$$
   $$\text{Frame Size in Bytes} = \frac{160\text{ bits}}{8\text{ bits/byte}} = \mathbf{20\text{ Bytes}}$$

*(If using binary $1\text{ Kbps} = 1024\text{ bps}$: $L = 0.040 \times 4096 = 163.84\text{ bits} \approx 20.48\text{ Bytes}$. Standard metric: **20 Bytes / 160 bits**).*

---

### Q.1b [4 Marks]
**Question:**
> Explain OSI Model with neat diagram. State functionality, protocols and standard of each layer.

**Step-by-Step Solution:**
1. **OSI Architecture Diagram:**
```
  [Application Layer]    <--- Layer 7 (User Interface & Network Services)
           │
  [Presentation Layer]   <--- Layer 6 (Syntax, Encryption, Data Compression)
           │
  [ Session Layer ]      <--- Layer 5 (Dialog Management, Checkpoints, Tokens)
           │
  [ Transport Layer ]    <--- Layer 4 (End-to-End Reliability, Flow/Congestion Control)
           │
  [  Network Layer  ]    <--- Layer 3 (Logical Addressing, Routing, Packet Forwarding)
           │
  [ Data Link Layer ]    <--- Layer 2 (Framing, Physical MAC Addressing, Error Detection)
           │
  [ Physical Layer  ]    <--- Layer 1 (Raw Bit Transmission, Voltage, Connectors)
```

2. **Layer-by-Layer Functionality, Protocols & Standards:**

| Layer | Primary Functionality | Representative Protocols | Standards / Specs |
| :--- | :--- | :--- | :--- |
| **7. Application** | Provides network services directly to end-user software applications. | HTTP, DNS, SMTP, FTP, DHCP | RFC 2616, RFC 1035 |
| **6. Presentation** | Data representation, translation (ASCII/EBCDIC), encryption/decryption, compression. | TLS/SSL, JPEG, MPEG, MIDI | X.209 (ASN.1), RFC 8446 |
| **5. Session** | Establishes, manages, synchronizes (checkpoints), and terminates application sessions. | NetBIOS, RPC, PPTP, SCP | ISO/IEC 8326 |
| **4. Transport** | True end-to-end process-to-process communication, segmentation, flow/congestion control. | TCP, UDP, SCTP, QUIC | RFC 793, RFC 768 |
| **3. Network** | Host-to-host packet routing across heterogeneous networks, logical IP addressing. | IPv4, IPv6, ICMP, OSPF, BGP | RFC 791, RFC 8200 |
| **2. Data Link** | Node-to-node hop-by-hop framing, MAC addressing, error detection (CRC), flow control. | Ethernet (MAC), PPP, HDLC, ARP | IEEE 802.3, RFC 1661 |
| **1. Physical** | Mechanical, electrical, and functional interface for transmitting raw unstructured bitstreams over physical media. | NRZ, Manchester, DSL, 1000Base-T | EIA/TIA-232, IEEE 802.3u |

---

### OR Q.1b [4 Marks]
**Question:**
> Calculate Transmission Delay to transmit $10\text{ GB}$ data over the channel having bandwidth $20\text{ Kbps}$?

**Step-by-Step Solution:**
1. **Given Data:**
   - Data Size ($L$) $= 10\text{ GB}$
   - Bandwidth ($R$) $= 20\text{ Kbps} = 20 \times 10^3\text{ bps} = 20,000\text{ bps}$

2. **Unit Conversions:**
   - **Decimal Standard ($1\text{ GB} = 10^9\text{ Bytes}$)**:
     $$L = 10 \times 10^9\text{ Bytes} \times 8\text{ bits/byte} = 8 \times 10^{10}\text{ bits} = 80,000,000,000\text{ bits}$$
   - **Binary Standard ($1\text{ GiB} = 2^{30}\text{ Bytes}$)**:
     $$L = 10 \times 1,073,741,824\text{ Bytes} \times 8\text{ bits/byte} = 85,899,345,920\text{ bits}$$

3. **Transmission Delay Calculation ($T_{\text{trans}} = \frac{L}{R}$):**
   - **Under Decimal Standard:**
     $$T_{\text{trans}} = \frac{80 \times 10^9\text{ bits}}{20 \times 10^3\text{ bps}} = \mathbf{4,000,000\text{ seconds}}$$
     $$\text{In Hours} = \frac{4,000,000}{3,600} \approx \mathbf{1,111.11\text{ Hours}} \approx \mathbf{46.3\text{ Days}}$$
   - **Under Binary Standard:**
     $$T_{\text{trans}} = \frac{85,899,345,920\text{ bits}}{20,000\text{ bps}} = \mathbf{4,294,967.3\text{ seconds}} \approx \mathbf{1,193.05\text{ Hours}} \approx \mathbf{49.71\text{ Days}}$$

---

### Q.1c [6 Marks]
**Question:**
> A point-to-point satellite transmission line connecting two computers uses a stop and wait -protocol and has the following properties:
> - Data Transmission Rate (DTR) $= 64\text{ kbps}$
> - Frame Size $= 2048\text{ byte}$
> - One Way Propagation Delay $= 180\text{ ms}$
> - Acknowledgement Size $= 10\text{ bytes}$
> - Processing Delay of one computer $= 25\text{ ms}$

**Step-by-Step Solution:**
1. **Parameters & Conversions:**
   - $\text{DTR } (R) = 64\text{ kbps} = 64,000\text{ bits/second}$
   - $\text{Frame Size } (L_f) = 2048\text{ bytes} = 2048 \times 8 = 16,384\text{ bits}$
   - $\text{Ack Size } (L_{\text{ack}}) = 10\text{ bytes} = 10 \times 8 = 80\text{ bits}$
   - $T_{\text{prop}} = 180\text{ ms} = 0.180\text{ s}$
   - $T_{\text{proc}} = 25\text{ ms} = 0.025\text{ s}$ (per computer)

2. **Component Delays:**
   - **Data Frame Transmission Delay ($T_{t,f}$)**:
     $$T_{t,f} = \frac{16,384\text{ bits}}{64,000\text{ bps}} = 0.256\text{ s} = \mathbf{256\text{ ms}}$$
   - **Forward Propagation Delay**: $T_{\text{prop}} = \mathbf{180\text{ ms}}$
   - **Receiver Processing Delay**: $T_{\text{proc, rx}} = \mathbf{25\text{ ms}}$
   - **ACK Transmission Delay ($T_{t,\text{ack}}$)**:
     $$T_{t,\text{ack}} = \frac{80\text{ bits}}{64,000\text{ bps}} = 0.00125\text{ s} = \mathbf{1.25\text{ ms}}$$
   - **Reverse Propagation Delay**: $T_{\text{prop}} = \mathbf{180\text{ ms}}$
   - **Sender Processing Delay**: $T_{\text{proc, tx}} = \mathbf{25\text{ ms}}$

3. **Total Round-Trip Cycle Time ($T_{\text{total}}$):**
   $$T_{\text{total}} = T_{t,f} + T_{\text{prop}} + T_{\text{proc, rx}} + T_{t,\text{ack}} + T_{\text{prop}} + T_{\text{proc, tx}}$$
   $$T_{\text{total}} = 256\text{ ms} + 180\text{ ms} + 25\text{ ms} + 1.25\text{ ms} + 180\text{ ms} + 25\text{ ms} = \mathbf{667.25\text{ ms}} = 0.66725\text{ s}$$
   *(Note: If processing delay is counted only once at receiver: $T_{\text{total}} = 642.25\text{ ms}$).*

4. **Line Utilization / Efficiency ($\eta$):**
   $$\eta = \frac{T_{t,f}}{T_{\text{total}}} = \frac{256\text{ ms}}{667.25\text{ ms}} \approx \mathbf{38.37\%} \quad (\text{or } 39.86\% \text{ with single } T_{\text{proc}})$$

5. **Effective Throughput:**
   $$\text{Throughput} = \eta \times \text{DTR} = 0.3837 \times 64\text{ kbps} \approx \mathbf{24.56\text{ kbps}}$$

---

### Q.2a [6 Marks]
**Question:**
> Calculate the following with given data as Transmission time $= 1\text{ ms}$, propagation delay $= 99.5\text{ ms}$ and predefined sequence Number Header $N = 7$.
> 1. Sender window size
> 2. Sequence numbers required?
> 3. No of bits in Sequence No field.
> 4. Efficiency of channel with calculated sliding window size.

**Step-by-Step Solution:**
1. **Given Data:**
   - Transmission Time ($T_t$) $= 1\text{ ms}$
   - Propagation Delay ($T_p$) $= 99.5\text{ ms}$
   - Header Sequence Number Field bits ($N$) $= 7\text{ bits}$

2. **Parameter $a$ and Theoretical Optimal Window:**
   $$a = \frac{T_p}{T_t} = \frac{99.5\text{ ms}}{1\text{ ms}} = 99.5$$
   $$W_{\text{optimal}} = 1 + 2a = 1 + 2(99.5) = 1 + 199 = \mathbf{200\text{ frames}}$$

3. **Sub-questions:**
   - **1. Sender Window Size ($W_s$):**
     - Based on the predefined header $N = 7$ bits:
       - In **Go-Back-N (GBN)**: $W_s = 2^N - 1 = 2^7 - 1 = 128 - 1 = \mathbf{127\text{ frames}}$.
       - In **Selective Repeat (SR)**: $W_s = 2^{N-1} = 2^{7-1} = 2^6 = \mathbf{64\text{ frames}}$.
       - *(If designed for 100% capacity irrespective of $N$: $W_s = W_{\text{optimal}} = \mathbf{200\text{ frames}}$)*.
   - **2. Sequence Numbers Required:**
     - To achieve full 100% link utilization with window $W = 200$:
       - In **GBN**: Total sequence numbers required $\ge W_s + 1 = 200 + 1 = \mathbf{201}$.
       - In **SR**: Total sequence numbers required $\ge 2 \times W_s = 2 \times 200 = \mathbf{400}$.
   - **3. Number of Bits in Sequence Number Field ($k$):**
     - To support 100% utilization:
       - For GBN: $2^k \ge 201 \implies k = \lceil \log_2 201 \rceil = \mathbf{8\text{ bits}}$ ($2^8 = 256$).
       - For SR: $2^k \ge 400 \implies k = \lceil \log_2 400 \rceil = \mathbf{9\text{ bits}}$ ($2^9 = 512$).
     - For the given predefined header: The field currently has $N = \mathbf{7\text{ bits}}$.
   - **4. Efficiency of Channel with Calculated Sliding Window Size:**
     $$\eta = \frac{W_s}{1 + 2a} = \frac{W_s}{200}$$
     - **With GBN window ($W_s = 127$)**:
       $$\eta = \frac{127}{200} = \mathbf{63.5\%}$$
     - **With Selective Repeat window ($W_s = 64$)**:
       $$\eta = \frac{64}{200} = \mathbf{32.0\%}$$
     - **With Full Pipe window ($W_s = 200$)**:
       $$\eta = \frac{200}{200} = \mathbf{100\%}$$

---

### Q.2b [6 Marks]
**Question:**
> Write a pseudo code to implement Sliding window protocol in detail, test code with example.

**Step-by-Step Solution:**
```c
// High-Level Sender Pseudocode for 1-bit Sliding Window Protocol (Stop-and-Wait)
void Sender(void) {
    seq_nr next_frame_to_send = 0;  // 0 or 1
    frame s;
    packet buffer;

    while (true) {
        wait_for_event(&event);
        if (event == network_layer_ready) {
            from_network_layer(&buffer);
            s.data = buffer;
            s.seq = next_frame_to_send;
            to_physical_layer(&s);
            start_timer(s.seq);

            while (true) {
                wait_for_event(&event);
                if (event == frame_arrival) {
                    from_physical_layer(&r);
                    if (r.ack == next_frame_to_send) {
                        stop_timer(r.ack);
                        next_frame_to_send = 1 - next_frame_to_send; // Toggle 0 <-> 1
                        break; // Ready for next packet
                    }
                } else if (event == timeout) {
                    to_physical_layer(&s); // Retransmit upon timeout
                    start_timer(s.seq);
                }
            }
        }
    }
}
```
**Test Example Trace:**
1. Sender sends `Frame 0`. Timer started.
2. Receiver gets `Frame 0`, validates checksum, sends `ACK 0`.
3. Sender receives `ACK 0`, stops timer, toggles `next_frame = 1`.
4. Sender transmits `Frame 1`. Packet lost in transit.
5. Sender timer expires (`timeout`).
6. Sender retransmits `Frame 1`. Receiver gets `Frame 1`, accepts and returns `ACK 1`. Successful delivery confirmed.

---

### OR Q.2b [6 Marks]
**Question:**
> Test if `011100101110` code word is correct, assuming they were created using an even parity Hamming Code. If one is incorrect, indicate what the correct code word should have been. Also, indicate what the original data was.

**Step-by-Step Solution:**
1. **Identify Bit Positions (Left-to-Right from 1 to 12):**

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Role** | $P_1$ | $P_2$ | $D_1$ | $P_4$ | $D_2$ | $D_3$ | $D_4$ | $P_8$ | $D_5$ | $D_6$ | $D_7$ | $D_8$ |
| **Bit Value** | **0** | **1** | **1** | **1** | **0** | **0** | **1** | **0** | **1** | **1** | **1** | **0** |

2. **Parity Check Calculations (Even Parity $\implies$ XOR Sum should be 0):**
   - **$C_1$ (checks positions with bit 0 set in binary: 1, 3, 5, 7, 9, 11)**:
     $$b_1 \oplus b_3 \oplus b_5 \oplus b_7 \oplus b_9 \oplus b_{11} = 0 \oplus 1 \oplus 0 \oplus 1 \oplus 1 \oplus 1 = \mathbf{0} \quad (\text{No error in bit 0})$$
   - **$C_2$ (checks positions with bit 1 set in binary: 2, 3, 6, 7, 10, 11)**:
     $$b_2 \oplus b_3 \oplus b_6 \oplus b_7 \oplus b_{10} \oplus b_{11} = 1 \oplus 1 \oplus 0 \oplus 1 \oplus 1 \oplus 1 = \mathbf{1} \quad (\mathbf{Error detected!})$$
   - **$C_4$ (checks positions with bit 2 set in binary: 4, 5, 6, 7, 12)**:
     $$b_4 \oplus b_5 \oplus b_6 \oplus b_7 \oplus b_{12} = 1 \oplus 0 \oplus 0 \oplus 1 \oplus 0 = \mathbf{0} \quad (\text{No error in bit 2})$$
   - **$C_8$ (checks positions with bit 3 set in binary: 8, 9, 10, 11, 12)**:
     $$b_8 \oplus b_9 \oplus b_{10} \oplus b_{11} \oplus b_{12} = 0 \oplus 1 \oplus 1 \oplus 1 \oplus 0 = \mathbf{1} \quad (\mathbf{Error detected!})$$

3. **Syndrome Evaluation:**
   $$\text{Syndrome } S = C_8 C_4 C_2 C_1 = 1010_2 = 8 + 0 + 2 + 0 = \mathbf{10}$$
   **Conclusion:** The code word is **INCORRECT**. The single-bit error is located at **Position 10**!

4. **Determine the Corrected Code Word:**
   - The bit at position 10 is currently `1`.
   - Invert position 10: $1 \to \mathbf{0}$.
   - Corrected Code Word:
     $$\mathbf{0 \ 1 \ 1 \ 1 \ 0 \ 0 \ 1 \ 0 \ 1 \ 0 \ 1 \ 0}$$

5. **Extract the Original Data Bits ($D_1$ to $D_8$ from positions 3, 5, 6, 7, 9, 10, 11, 12):**
   - $D_1 (\text{Pos } 3) = \mathbf{1}$
   - $D_2 (\text{Pos } 5) = \mathbf{0}$
   - $D_3 (\text{Pos } 6) = \mathbf{0}$
   - $D_4 (\text{Pos } 7) = \mathbf{1}$
   - $D_5 (\text{Pos } 9) = \mathbf{1}$
   - $D_6 (\text{Pos } 10) = \mathbf{0}$ *(corrected from 1)*
   - $D_7 (\text{Pos } 11) = \mathbf{1}$
   - $D_8 (\text{Pos } 12) = \mathbf{0}$
   - **Original Data:** $\mathbf{1 0 0 1 1 0 1 0_2}$ (Hexadecimal: `0x9A`).

---

### Q.3a [6 Marks]
**Question:**
> Subnet the Class C IP Address `195.1.1.0` so that you have 10 subnets each with a maximum 12 hosts on each subnet. List the Address on host 1 on subnet 0, 1, 2, 3, 10.

**Step-by-Step Solution:**
1. **Analyze Requirements:**
   - Base Address: `195.1.1.0` (Class C default prefix: `/24`, default mask `255.255.255.0`).
   - Total host bits available $= 8$ bits.
   - Required Subnets: At least 10 $\implies 2^s \ge 10 \implies s = 4$ subnet bits ($2^4 = 16 \ge 10$).
   - Required Hosts per subnet: At least 12 hosts $\implies 2^h - 2 \ge 12 \implies 2^h \ge 14 \implies h = 4$ host bits ($2^4 - 2 = 14 \ge 12$).
   - Total bits: $s + h = 4 + 4 = 8$ bits. Exactly matches the 8 host bits of Class C!

2. **Subnet Mask & Block Size:**
   - New Prefix: $24 + 4 = \mathbf{/28}$.
   - Subnet Mask: `11111111.11111111.11111111.11110000` = $\mathbf{255.255.255.240}$.
   - Block Size (Step Size) $= 2^h = 2^4 = \mathbf{16}$.

3. **Subnet Table & Host 1 Address (Host 1 $=$ Network ID $+ 1$):**

| Subnet Number | Network ID | Host 1 Address | Usable Host Range | Directed Broadcast |
| :---: | :---: | :---: | :---: | :---: |
| **Subnet 0** | `195.1.1.0/28` | **`195.1.1.1`** | `195.1.1.1` to `.14` | `195.1.1.15` |
| **Subnet 1** | `195.1.1.16/28` | **`195.1.1.17`** | `195.1.1.17` to `.30` | `195.1.1.31` |
| **Subnet 2** | `195.1.1.32/28` | **`195.1.1.33`** | `195.1.1.33` to `.46` | `195.1.1.47` |
| **Subnet 3** | `195.1.1.48/28` | **`195.1.1.49`** | `195.1.1.49` to `.62` | `195.1.1.63` |
| **Subnet 10** | `195.1.1.160/28` | **`195.1.1.161`** | `195.1.1.161` to `.174` | `195.1.1.175` |

*(Formula for Subnet $k$: $\text{NetID} = 195.1.1.(k \times 16)$; $\text{Host 1} = 195.1.1.(k \times 16 + 1)$)*.

---

### OR Q.3a [6 Marks]
**Question:**
> An organization is granted the block `130.34.12.64/26`. The organization needs 4 subnets and each with an equal number of hosts. Design a subnet and find no of address, first and last address in each subnet.

**Step-by-Step Solution:**
1. **Given Data:**
   - Assigned block: `130.34.12.64/26`.
   - Number of subnets required $= 4$.
   - Bits to borrow: $2^s = 4 \implies s = 2$ bits.

2. **Calculations:**
   - New Prefix: $26 + 2 = \mathbf{/28}$.
   - Subnet Mask: $\mathbf{255.255.255.240}$.
   - Host bits remaining: $h = 32 - 28 = 4$ bits.
   - Total addresses per subnet: $2^4 = \mathbf{16\text{ addresses}}$.
   - Usable hosts per subnet: $16 - 2 = \mathbf{14\text{ usable hosts}}$.
   - Block increment: $16$.

3. **Subnet Design Table:**

| Subnet | Network ID | No. of Addresses | First Usable Host | Last Usable Host | Broadcast Address |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **Subnet 0** | `130.34.12.64/28` | 16 | `130.34.12.65` | `130.34.12.78` | `130.34.12.79` |
| **Subnet 1** | `130.34.12.80/28` | 16 | `130.34.12.81` | `130.34.12.94` | `130.34.12.95` |
| **Subnet 2** | `130.34.12.96/28` | 16 | `130.34.12.97` | `130.34.12.110` | `130.34.12.111` |
| **Subnet 3** | `130.34.12.112/28` | 16 | `130.34.12.113` | `130.34.12.126` | `130.34.12.127` |

*(Note: First address in subnet is the Network ID; last address is the Broadcast address)*.

---

### Q.3b [6 Marks]
**Question:**
> Let us assume that A and B start transmitting data and collision occurs. Now, A and B know that there is a collision and they stop transmitting. When should A and B again start transmission? (Demonstrate and explain it with neat diagram.)

**Step-by-Step Solution:**
1. **Collision Handling Mechanism (CSMA/CD):**
   - When A and B detect collision, they immediately cease transmitting their remaining payload bits.
   - Both stations broadcast a **32-to-48 bit Jamming Signal** onto the channel to guarantee all other stations detect the collision.
   - Both stations initialize their collision counter $i$ ($i \gets i + 1$).
2. **Binary Exponential Back-Off (BEB) Algorithm:**
   - After the $i$-th collision, each station selects a random integer $K$ uniformly from the discrete set:
     $$K \in [0, 2^k - 1] \quad \text{where } k = \min(i, 10)$$
   - Each station waits for backoff duration:
     $$\text{Wait Time} = K \times \text{Slot Time} \quad (\text{Slot Time} = 2 T_{\text{prop}} = 51.2\,\mu\text{s})$$
3. **Timeline Demonstration for Collision #1 ($i = 1$):**
   - $k = 1 \implies K \in [0, 2^1 - 1] = \{0, 1\}$.
   - Station A randomly picks $K_A \in \{0, 1\}$; Station B randomly picks $K_B \in \{0, 1\}$.
   - If $K_A = 0$ and $K_B = 1$: Station A senses the medium and retransmits immediately (after 96-bit Interframe Gap). Station B waits for $51.2\,\mu\text{s}$, detects Station A's carrier, and defers. **Collision resolved!**
   - If $K_A = K_B$ (e.g. both pick 0 or both pick 1), they collide a 2nd time ($i = 2$). Range expands to $K \in \{0, 1, 2, 3\}$. The probability of collision drops to $1/4 = 25\%$.
   - Max attempts $= 16$. If $i = 16$, transmission is aborted with network failure.

---

### Q.4a [4 Marks]
**Question:**
> Compute the shortest path between node `a` to `z` by using Distance Vector Routing Protocol.
> *(Graph: a-b:4, a-c:2, b-c:1, b-d:5, c-d:8, c-e:10, d-e:2, d-z:6, e-z:3)*

**Step-by-Step Solution:**
1. **Graph Specification:**
   - Nodes: $\{a, b, c, d, e, z\}$
   - Link Costs: $(a, b)=4, (a, c)=2, (b, c)=1, (b, d)=5, (c, d)=8, (c, e)=10, (d, e)=2, (d, z)=6, (e, z)=3$.

2. **Distance Vector (Bellman-Ford) Update Formula:**
   $$D_x(y) = \min_{v \in \text{neighbors}(x)} \big\{ c(x, v) + D_v(y) \big\}$$

3. **Step-by-Step Iterations from Source Node `a`:**
   - **Iteration 0 (Direct Neighbors of `a`)**:
     $D(a)=0, D(b)=4, D(c)=2, D(d)=\infty, D(e)=\infty, D(z)=\infty$.
   - **Iteration 1 (Vectors from `b` and `c` received)**:
     - Distance to $b$: $\min(4, c(a, c) + c(c, b) = 2 + 1 = 3) \implies \mathbf{3}$ (via $c$).
     - Distance to $d$: $\min(\infty, D(c)+c(c, d)=2+8=10, D(b)+c(b, d)=3+5=8) \implies \mathbf{8}$ (path: $a \to c \to b \to d$).
     - Distance to $e$: $\min(\infty, D(c)+c(c, e)=2+10=12) \implies \mathbf{12}$ (via $c$).
   - **Iteration 2 (Vector from `d` received)**:
     - Distance to $e$: $\min(12, D(d) + c(d, e) = 8 + 2 = 10) \implies \mathbf{10}$ (path: $a \to c \to b \to d \to e$).
     - Distance to $z$: $\min(\infty, D(d) + c(d, z) = 8 + 6 = 14) \implies \mathbf{14}$ (via $d$).
   - **Iteration 3 (Vector from `e` received)**:
     - Distance to $z$: $\min(14, D(e) + c(e, z) = 10 + 3 = 13) \implies \mathbf{13}$ (path: $a \to c \to b \to d \to e \to z$).

4. **Result:**
   - **Shortest Path from `a` to `z`**:
     $$\mathbf{a \longrightarrow c \longrightarrow b \longrightarrow d \longrightarrow e \longrightarrow z}$$
   - **Total Path Cost**:
     $$2 + 1 + 5 + 2 + 3 = \mathbf{13}$$
   - **Routing Table Entry at Node `a`**: Destination: `z` | Cost: `13` | Next Hop: `c`.

---

### Q.4b [4 Marks]
**Question:**
> Detect if there is a negative weight cycle in following graph?
> *(Graph: 0 -> 1 with weight 1, 1 -> 2 with weight 3, 2 -> 3 with weight 2, 3 -> 1 with weight -6)*

**Step-by-Step Solution:**
1. **Identify the Cycle:**
   - The graph contains a directed cycle formed by nodes: $1 \longrightarrow 2 \longrightarrow 3 \longrightarrow 1$.
2. **Calculate Total Cycle Weight:**
   $$\text{Weight}(1, 2) = +3$$
   $$\text{Weight}(2, 3) = +2$$
   $$\text{Weight}(3, 1) = -6$$
   $$\text{Cycle Weight} = (+3) + (+2) + (-6) = 5 - 6 = \mathbf{-1}$$
3. **Conclusion:**
   - Since the sum of edge weights around the cycle is **negative ($-1 < 0$)**, **YES, A NEGATIVE WEIGHT CYCLE EXISTS**.
4. **Algorithmic Detection via Bellman-Ford:**
   - Run Bellman-Ford for $|V| - 1 = 4 - 1 = 3$ iterations.
   - Run a 4th relaxation step over all edges.
   - For edge $(3, 1)$: $dist[1] > dist[3] + (-6)$. Because the cycle reduces distance by 1 indefinitely on every traversal, the distance to nodes 1, 2, and 3 converges to $-\infty$. Bellman-Ford flags `Negative Weight Cycle Detected`.

---

### OR Q.4b [4 Marks]
**Question:**
> Suppose two programs use TCP to establish a connection, communicate, terminate the connection, and then open a new connection. Further suppose a FIN message sent to shutdown the first connection is duplicated and delayed until the second connection has been established. If a copy of the old FIN is delivered, will TCP terminate the new connection?

**Step-by-Step Solution:**
1. **Direct Answer:**
   - **NO, TCP will NOT terminate the new connection.**
2. **Technical Justification:**
   - **Sequence Number Validation:** Every TCP segment carries a 32-bit sequence number. Modern TCP implementations generate Initial Sequence Numbers (ISNs) using a clock-based pseudo-random generator (RFC 6528). When the delayed duplicate `FIN` arrives, its sequence number belongs to the *old* connection's sequence space and falls strictly outside the receive window (`rcv_wnd`) of the new connection. TCP discards out-of-window control packets silently.
   - **TIME_WAIT State Protection ($2 \times \text{MSL}$):** When the first connection closed, the actively closing endpoint entered `TIME_WAIT` for $2 \times \text{MSL}$ (Maximum Segment Lifetime, typically 120 seconds). This enforced quarantine ensures that any wandering duplicate segments (including FIN) expire and are drained by network router TTL decrement before a new connection on the same socket pair `(IP_A, Port_A, IP_B, Port_B)` can be established.
   - **Ephemeral Port Reallocation:** The operating system typically binds a fresh, different ephemeral port number for the second connection, causing the packet tuple to mismatch entirely.

---

### Q.5a [4 Marks]
**Question:**
> Choose correct transport layer protocol to transfer character array from host A to host B and write pseudo code for host A to host B interprocess file transfer.

**Step-by-Step Solution:**
1. **Protocol Selection:** **TCP (Transmission Control Protocol - `SOCK_STREAM`)**.
   - *Reason:* Text data / character arrays require **100% loss-free, reliable, in-order byte stream delivery**. Missing or swapped characters corrupt text semantics. TCP provides checksums, sequencing, cumulative ACKs, and retransmissions.
2. **Interprocess Socket Pseudocode (Client Host A $\to$ Server Host B):**
```python
# HOST B (Server Receiver)
server_socket = socket(AF_INET, SOCK_STREAM)
server_socket.bind(("0.0.0.0", 8080))
server_socket.listen(1)
conn, addr = server_socket.accept()
while True:
  data = conn.recv(1024)
  if not data:
    break
  file.write(data)
conn.close()

# HOST A (Client Sender)
client_socket = socket(AF_INET, SOCK_STREAM)
client_socket.connect(("Host_B_IP", 8080))
char_array = "Important text character array payload"
client_socket.sendall(char_array.encode("utf-8"))
client_socket.close()
```

---

### Q.5b [4 Marks]
**Question:**
> Choose correct transport layer protocol to transfer Audio file from host A to host B and write pseudo code for host A to host B interprocess file transfer.

**Step-by-Step Solution:**
1. **Protocol Selection:**
   - **For Real-Time Audio Streaming (VoIP / Live Audio):** **UDP (User Datagram Protocol - `SOCK_DGRAM`)** with RTP.
     - *Reason:* Real-time audio prioritizes low latency and minimal jitter over 100% reliability. Human hearing tolerates 1-2% packet loss, but TCP retransmission delays cause jarring audio stutter and freezing.
   - *(Note: For downloading a static, stored MP3 audio file, TCP is chosen to prevent file corruption).*
2. **Interprocess UDP Socket Pseudocode:**
```python
# HOST B (Audio Stream Listener)
sock = socket(AF_INET, SOCK_DGRAM)
sock.bind(("0.0.0.0", 5005))
while True:
  audio_chunk, addr = sock.recvfrom(2048)
  audio_player.play(audio_chunk)

# HOST A (Audio Stream Sender)
sock = socket(AF_INET, SOCK_DGRAM)
server_addr = ("Host_B_IP", 5005)
for audio_chunk in read_mic_or_audio_stream():
  sock.sendto(audio_chunk, server_addr)
```

---

### OR Q.5c [4 Marks]
**Question:**
> How IP addresses are assigned by using DHCP Services?

**Step-by-Step Solution:**
1. **The 4-Step DORA Protocol Flow:**
   - **D — DHCP DISCOVER:** Client broadcasts `0.0.0.0:68 ➔ 255.255.255.255:67` with its MAC address and transaction ID (`xid`) to locate DHCP servers.
   - **O — DHCP OFFER:** DHCP server reserves an unallocated IP from its address pool and broadcasts/unicasts an offer containing: Offered IP, Subnet Mask, Default Gateway, DNS Servers, and Lease Duration.
   - **R — DHCP REQUEST:** Client broadcasts acceptance of the chosen server's offer, implicitly informing other DHCP servers to release their reserved offers.
   - **A — DHCP ACK:** Chosen server commits the IP lease to its database and sends an acknowledgment packet to client. Client configures its network interface.
2. **Lease Timers:**
   - **T1 Renewal Timer ($50\%$ of lease)**: Client sends unicast `DHCPREQUEST` to lease granter.
   - **T2 Rebinding Timer ($87.5\%$ of lease)**: If T1 fails, client broadcasts `DHCPREQUEST` to any available DHCP server.
   - **Expiration ($100\%$)**: Lease terminates; IP returned to pool.

---

### Q.6a [4 Marks]
**Question:**
> Resolve `www.vit.edu` to IP address by using DNS protocol.

**Step-by-Step Solution:**
```
[Client Host] ──(1) Query www.vit.edu──► [Local DNS Server (LDNS)]
                                                │
    ┌────────────────(2) Query "." ─────────────┤
    ▼                                           │
[Root DNS Server] ──(3) Referral to ".edu" ────►│
                                                │
    ┌────────────────(4) Query ".edu" ──────────┤
    ▼                                           │
[TLD DNS Server]  ──(5) Referral to vit.edu ───►│
                                                │
    ┌────────────────(6) Query "vit.edu" ───────┤
    ▼                                           │
[Authoritative]   ──(7) A: 103.14.120.5 ───────►│
                                                │
[Client Host] ◄──(8) Returns IP 103.14.120.5 ───┘
```
1. **Resolution Steps:**
   - **Step 1:** Client checks browser cache, OS cache, and hosts file. On cache miss, sends **Recursive Query** to campus Local DNS Server (LDNS).
   - **Step 2–3:** LDNS sends **Iterative Query** to Root DNS Server (`.`). Root server returns NS referral for the `.edu` Top-Level Domain (TLD).
   - **Step 4–5:** LDNS queries `.edu` TLD Server. TLD returns NS referral pointing to authoritative name servers for domain `vit.edu`.
   - **Step 6–7:** LDNS queries Authoritative DNS Server for `vit.edu`. The authoritative server returns the **`A` Resource Record**: `www.vit.edu ➔ 103.14.120.5` with a TTL value.
   - **Step 8:** LDNS caches the mapping and returns the resolved IPv4 address to the client.

---

### Q.6b [3 Marks]
**Question:**
> Elaborate persistent and Non persistent HTTP connection.

**Step-by-Step Solution:**

| Feature | Non-Persistent HTTP (HTTP/1.0) | Persistent HTTP (HTTP/1.1 & HTTP/2) |
| :--- | :--- | :--- |
| **Connection Lifespan** | Opens a separate TCP connection for **each individual web object** (HTML, CSS, image). Closes immediately after transfer. | Keeps a **single TCP connection open** across multiple object requests and responses. |
| **Round Trip Time (RTT)** | Requires $2 \times \text{RTT} + \text{Transfer Time}$ per object (1 RTT for TCP handshake + 1 RTT for HTTP request/data). | Requires $2 \times \text{RTT}$ only for initial handshake; subsequent objects require only **$1 \times \text{RTT}$**. |
| **Server Overhead** | High CPU and memory overhead; server exhausts file descriptors / sockets under load. | Low overhead; maintains far fewer concurrent TCP sockets. |
| **Pipelining** | Not supported. | Supported in HTTP/1.1 (send multiple requests without waiting for intermediate ACKs). HTTP/2 adds full multiplexing. |

---
---

# PART II: ESE MAY 2019 (CS3025 — 100 MARKS)

---

### Q.1a [9 Marks]
**Question:**
> What is need of sliding window protocol? How does it increase the bandwidth utilization?

**Step-by-Step Solution:**
1. **Need for Sliding Window Protocol:**
   - In Stop-and-Wait, the sender sends 1 frame and sits idle waiting for the round-trip propagation and transmission of the ACK.
   - For links with high **Bandwidth-Delay Product (BDP)** (e.g., satellite links, fiber optics), propagation time is much greater than transmission time ($a = T_p / T_t \gg 1$), resulting in abysmal utilization:
     $$\eta_{\text{Stop-and-Wait}} = \frac{1}{1 + 2a} \approx 1\% \text{ to } 5\%$$
   - This wastes 95%+ of link capacity.
2. **How Sliding Window Increases Utilization:**
   - **Pipelining:** Sliding Window allows the sender to transmit up to $W$ outstanding unacknowledged frames before pausing.
   - **Mathematical Proof of Utilization:**
     $$\eta_{\text{Sliding Window}} = \frac{W \cdot T_{\text{trans}}}{T_{\text{trans}} + 2 T_{\text{prop}}} = \frac{W}{1 + 2a}$$
   - By choosing window size $W \ge 1 + 2a$, link utilization reaches **100%**:
     $$\eta = \min\left(1.0, \, \frac{W}{1 + 2a}\right)$$
   - Both Go-Back-N and Selective Repeat keep the physical pipe continuously filled with data bits while ACKs travel back in the reverse direction.

---

### Q.1b [9 Marks]
**Question:**
> Design the error detection system to calculate the CRC/FCS/Checksum using modulo-2 division method for generator polynomial of $x^4 + x + 1$ and message $1101011011$? Send the frame to the destination and check the correctness of frame.

**Step-by-Step Solution:**
1. **Polynomial to Binary Representation:**
   - Generator: $G(x) = x^4 + x^1 + x^0 \implies \mathbf{10011}$ (5 bits, degree $r = 4$).
   - Message: $M = \mathbf{1101011011}$ (10 bits).
2. **Sender Modulo-2 Division:**
   - Append $r = 4$ zeros to $M$: $M' = \mathbf{11010110110000}$ (14 bits).
   - Divide $M'$ by $G = 10011$ using XOR subtraction:
```
         1100100101   (Quotient)
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
                 01110  <--- Remainder (4 bits FCS = 1110)
```
   - Frame Check Sequence (FCS) $= \mathbf{1110}$.
3. **Transmitted Frame ($T$):**
   $$T = M + \text{FCS} = \mathbf{11010110111110}$$
4. **Receiver Verification (Error-Free Case):**
   - Receiver divides received frame $T = 11010110111110$ by $G = 10011$.
   - The remainder evaluated equals $\mathbf{0000}$.
   - **Conclusion:** Remainder $= 0 \implies$ Frame accepted as error-free!
5. **Receiver Error Detection (Corrupted Case):**
   - Introduce 1-bit error (invert bit 4 from left: $110\mathbf{0}0110111110$).
   - Divide by $10011$. Remainder evaluates to a non-zero pattern ($\ne 0000$).
   - **Conclusion:** Frame rejected!

---

### Q.1c [5 Marks]
**Question:**
> Why the minimum frame size of 10Base5 Ethernet Network is 64 bytes and maximum frame size is 1518 bytes excluding synchronization and preamble bytes?

**Step-by-Step Solution:**
1. **Minimum Frame Size ($64\text{ Bytes} = 512\text{ bits}$):**
   - **Physical Law:** Collision detection requires:
     $$T_{\text{trans}} \ge 2 \cdot T_{\text{prop}} \implies L_{\min} \ge 2 \cdot T_{\text{prop}} \cdot \text{Bandwidth}$$
   - In 10Base5 thick Ethernet, maximum segment span with 4 repeaters is $2500\text{ meters}$. Round-trip propagation time $2 T_{\text{prop}} \approx 51.2\,\mu\text{s}$.
   - At $10\text{ Mbps}$: $L_{\min} = 51.2\,\mu\text{s} \times 10\text{ Mbps} = \mathbf{512\text{ bits}} = \mathbf{64\text{ Bytes}}$.
   - If frame is $< 64$ bytes, transmission finishes before collision notification arrives; sender assumes successful delivery and drops frame from buffer.
2. **Maximum Frame Size ($1518\text{ Bytes}$):**
   - Payload maximum MTU $= 1500\text{ Bytes}$.
   - Header overhead: Destination MAC (6B) + Source MAC (6B) + EtherType (2B) + FCS (4B) $= 18\text{ Bytes}$.
   - Total $= 1500 + 18 = \mathbf{1518\text{ Bytes}}$.
   - **Reasons:**
     1. Prevents a single greedy host from monopolizing the shared medium.
     2. Limits receiver buffer memory allocation requirements in legacy hardware.
     3. Keeps bit error probability per frame low (larger frames have exponential error probability).

---

### Q.1d [4 Marks]
**Question:**
> Show that the persistent and non persistent CSMA methods improves the performance over ALOHA methods.

**Step-by-Step Solution:**
1. **Vulnerability in ALOHA:**
   - In Pure ALOHA, stations transmit blindly at will without checking if the medium is in use. Vulnerable time $= 2 \times T_{\text{frame}}$; max efficiency is only **18.4%**.
2. **The CSMA Breakthrough ("Listen Before Talk"):**
   - CSMA introduces **Carrier Sensing**. A station samples medium carrier energy before injecting bits.
   - **1-Persistent CSMA:** If medium is busy, station waits and continuously listens; transmits immediately when medium becomes idle. Max efficiency $\approx \mathbf{53\%}$.
   - **Non-Persistent CSMA:** If medium is busy, station backs off for a random time before sensing again. Avoids synchronized collisions when a busy transmission ends. Max efficiency $\approx \mathbf{85\%}$.
3. **Performance Conclusion:**
   - By eliminating collisions with already ongoing active frame transmissions, CSMA increases channel capacity from $18.4\%$ to up to $85\%$.

---

### Q.2a [8 Marks]
**Question:**
> How CSMA/CA protocol is used to avoid collisions for air as a transmission medium among the number of clients?

**Step-by-Step Solution:**
1. **Why CSMA/CD Fails in Wireless (Air):**
   - RF signal attenuation follows inverse-square law ($1/d^2$ to $1/d^4$). A station's own transmitter drowns out distant incoming signals, making collision detection during transmission physically impossible.
   - Presence of **Hidden Terminal** and **Exposed Terminal** problems.
2. **CSMA/CA Collision Avoidance Mechanisms:**
   - **1. Interframe Spaces (IFS):**
     - SIFS (Shortest IFS): Highest priority, used for ACKs, CTS.
     - DIFS (Distributed IFS): Minimum wait time for new frame transmission after sensing idle channel.
   - **2. Contention Window & Random Backoff:**
     - Station waits for DIFS, then decrements a random backoff counter only while medium remains idle.
   - **3. Virtual Carrier Sensing with RTS/CTS Exchange:**
     - Sender broadcasts **RTS (Request to Send)** containing frame duration.
     - Receiver replies with **CTS (Clear to Send)**.
     - All hidden nodes overhearing CTS set their **NAV (Network Allocation Vector)** timer and remain silent during transmission.
   - **4. Explicit Link-Layer ACK:** Receiver confirms packet with SIFS ACK.

---

### Q.2b [8 Marks]
**Question:**
> Consider a link with Data rate of $1\text{ Mbps}$ and one way latency of $40\text{ ms}$. Assume a packet size of $1000\text{ bytes}$. Calculate the sending window size in packets over this link.

**Step-by-Step Solution:**
1. **Given Data:**
   - Data Rate ($R$) $= 1\text{ Mbps} = 10^6\text{ bps}$
   - One-way Latency / Propagation Delay ($T_{\text{prop}}$) $= 40\text{ ms} = 0.040\text{ s}$
   - Packet Size ($L$) $= 1000\text{ bytes} = 8,000\text{ bits}$
2. **Calculate Transmission Delay ($T_{\text{trans}}$):**
   $$T_{\text{trans}} = \frac{L}{R} = \frac{8,000\text{ bits}}{10^6\text{ bps}} = 0.008\text{ s} = \mathbf{8\text{ ms}}$$
3. **Calculate Parameter $a$:**
   $$a = \frac{T_{\text{prop}}}{T_{\text{trans}}} = \frac{40\text{ ms}}{8\text{ ms}} = \mathbf{5}$$
4. **Calculate Sending Window Size for 100% Efficiency:**
   $$W_{\text{optimal}} = 1 + 2a = 1 + 2(5) = \mathbf{11\text{ packets}}$$
   *(Alternatively, using Bandwidth-Delay Product: $\text{BDP} = R \times \text{RTT} = 10^6 \times 0.080\text{ s} = 80,000\text{ bits} = 10\text{ packets}$. Total window to fill pipe $= 10 + 1 = \mathbf{11\text{ packets}}$)*.

---

### Q.2c [5 Marks]
**Question:**
> Justify the following statement: "Network diameter reduces as the network bandwidth increases in wired LAN"

**Step-by-Step Solution:**
1. **Mathematical Derivation:**
   - In CSMA/CD wired networks, collision detection requires:
     $$T_{\text{trans}} \ge 2 \cdot T_{\text{prop}} \implies \frac{L_{\min}}{B} \ge 2 \cdot \frac{D_{\max}}{v}$$
   - Solving for network diameter ($D_{\max}$):
     $$D_{\max} \le \frac{L_{\min} \cdot v}{2 \cdot B}$$
     where $L_{\min}$ is minimum frame size, $v$ is signal propagation velocity in copper ($\approx 2 \times 10^8\text{ m/s}$), and $B$ is bandwidth.
2. **Inverse Relationship:**
   - Network diameter $D_{\max}$ is **strictly inversely proportional to network bandwidth ($B$)**:
     $$D_{\max} \propto \frac{1}{B}$$
3. **Empirical Evidence in Ethernet History:**
   - Standard Ethernet ($10\text{ Mbps}$): $D_{\max} \approx \mathbf{2500\text{ meters}}$.
   - Fast Ethernet ($100\text{ Mbps}$): Bandwidth increased by $10\times \implies$ diameter shrank by $10\times$ to $\mathbf{205\text{ meters}}$!
   - Gigabit Ethernet ($1000\text{ Mbps}$): Diameter would shrink to $20\text{ meters}$ (unusable), which forced the introduction of **Carrier Extension** and full-duplex switched Ethernet.
   - **Statement is fully justified!**

---

### Q.2d [3 Marks]
**Question:**
> Why PPP is more popular than HDLC protocol?

**Step-by-Step Solution:**
1. **Multi-Protocol Encapsulation:** PPP supports multiple network layer protocols simultaneously over one link (IPv4, IPv6, IPX, AppleTalk) via its **NCP (Network Control Protocol)**. HDLC is primarily bit-oriented and single-protocol.
2. **Built-in Authentication:** PPP has native security authentication protocols (**PAP** and **CHAP**). HDLC lacks native authentication.
3. **Dynamic IP Configuration:** PPP includes IPCP (RFC 1332) to negotiate and assign IP addresses dynamically on dial-up/point-to-point links.
4. **Vendor Interoperability:** PPP is an open, standardized IETF RFC (RFC 1661) supported across all vendors. HDLC implementations by vendors (e.g. Cisco HDLC) are proprietary and mutually incompatible.

---

### Q.3a [8 Marks]
**Question:**
> Consider the subnet of following figure. Distance vector routing is used and the following vectors have just come in to router C: from B( 5,0,8,12,6,2); from D(16,12,6,0,9,10); and from E(7,6,3,9,0,4). The measured delays to B, D, and E are 6, 3, and 5 respectively. What is C's new routing table?

**Step-by-Step Solution:**
1. **Given Data:**
   - Neighbors of Router C: $B$, $D$, and $E$.
   - Measured link delays from C:
     - $c(C, B) = 6$
     - $c(C, D) = 3$
     - $c(C, E) = 5$
   - Destination nodes order: $(A, B, C, D, E, F)$
   - Incoming vectors:
     - Vector from $B$: $(5, 0, 8, 12, 6, 2)$
     - Vector from $D$: $(16, 12, 6, 0, 9, 10)$
     - Vector from $E$: $(7, 6, 3, 9, 0, 4)$
2. **Bellman-Ford Distance Computation at Router C:**
   $$D_C(X) = \min \big[ c(C, B) + D_B(X), \; c(C, D) + D_D(X), \; c(C, E) + D_E(X) \big]$$
   - **To A:** $\min(6+5, \, 3+16, \, 5+7) = \min(11, 19, 12) = \mathbf{11} \quad (\text{via } B)$
   - **To B:** $\min(6+0, \, 3+12, \, 5+6) = \min(6, 15, 11) = \mathbf{6} \quad (\text{via } B)$
   - **To C:** Distance to itself is $\mathbf{0}$.
   - **To D:** $\min(6+12, \, 3+0, \, 5+9) = \min(18, 3, 14) = \mathbf{3} \quad (\text{via } D)$
   - **To E:** $\min(6+6, \, 3+9, \, 5+0) = \min(12, 12, 5) = \mathbf{5} \quad (\text{via } E)$
   - **To F:** $\min(6+2, \, 3+10, \, 5+4) = \min(8, 13, 9) = \mathbf{8} \quad (\text{via } B)$
3. **C's New Routing Table:**

| Destination | Minimum Delay / Cost | Outgoing Line / Next Hop |
| :---: | :---: | :---: |
| **A** | **11** | **B** |
| **B** | **6** | **B** |
| **C** | **0** | **—** |
| **D** | **3** | **D** |
| **E** | **5** | **E** |
| **F** | **8** | **B** |

---

### Q.3b [8 Marks]
**Question:**
> One of the CIDR IP address allocated to the organization is `167.199.130.83/25`. Find the IP block size, netmask, first IP address and last IP address for the block.

**Step-by-Step Solution:**
1. **Given Data:** `167.199.130.83/25`
   - Prefix length $= /25$
2. **Calculations:**
   - **1. Netmask:**
     - First 25 bits are 1s, remaining 7 bits are 0s:
       `11111111.11111111.11111111.10000000`
     - Netmask $= \mathbf{255.255.255.128}$
   - **2. IP Block Size:**
     - Host bits $h = 32 - 25 = 7$ bits.
     - Block Size $= 2^h = 2^7 = \mathbf{128\text{ addresses}}$.
   - **3. First IP Address (Network ID):**
     - 4th octet value $= 83$.
     - Block boundaries in multiples of 128: $0, 128$.
     - Since $0 \le 83 < 128$, the block starts at $0$.
     - First IP Address $= \mathbf{167.199.130.0}$
   - **4. Last IP Address (Directed Broadcast Address):**
     - Last address in block $= \text{Base} + \text{Block Size} - 1 = 0 + 128 - 1 = 127$.
     - Last IP Address $= \mathbf{167.199.130.127}$
   - *(Usable Host Range: `167.199.130.1` to `167.199.130.126`, Total 126 usable hosts)*.

---

### Q.4a [8 Marks]
**Question:**
> If a host on internet in VIT Campus wants to communicate with a host in IIT for live lectures. If both are expected to have reliable communication over the internet. Before sending first data packet few other packets needs to be send from either the side. Can you remember the name of these packet exchanges? Illustrate this with neat diagram.

**Step-by-Step Solution:**
1. **Name of Packet Exchanges:**
   - **TCP 3-Way Handshake Connection Establishment** (`SYN`, `SYN+ACK`, `ACK`).
2. **Why Handshake is Compulsory for Reliable Live Lecture:**
   - TCP must synchronize Initial Sequence Numbers (ISNs) to guarantee in-order delivery of video chunks and avoid duplicate packet corruption.
   - Negotiates Maximum Segment Size (MSS) and window scaling parameters.
3. **Neat Ladder Diagram:**
```
  VIT Host (Client)                                IIT Host (Server)
         │                                                │
         │  1. [SYN] Seq = x                              │
         ├───────────────────────────────────────────────►│ (Allocates TCB Buffer)
         │                                                │
         │  2. [SYN + ACK] Seq = y, Ack = x + 1           │
         │◄───────────────────────────────────────────────┤
         │                                                │
         │  3. [ACK] Seq = x + 1, Ack = y + 1             │
         ├───────────────────────────────────────────────►│
   (ESTABLISHED)                                    (ESTABLISHED)
         │                                                │
         │══════════ Reliable Live Lecture Stream ═══════►│
```
4. **Packet Description:**
   - **Segment 1 (SYN)**: Client sends `SYN=1, Seq=x` to request connection.
   - **Segment 2 (SYN+ACK)**: Server responds with `SYN=1, ACK=1, Seq=y, Ack=x+1`, acknowledging $x$ and announcing server's sequence $y$.
   - **Segment 3 (ACK)**: Client returns `ACK=1, Seq=x+1, Ack=y+1`. Connection is established.

---

### Q.6b [5 Marks]
**Question:**
> Justify the following statement: "RIP protocol is not suitable for wide area network routing"

**Step-by-Step Solution:**
1. **Hop Count Metric Limit ($\le 15$ hops):**
   - Routing Information Protocol (RIP) uses hop count as its sole metric. A hop count of **16 denotes infinity (unreachable)**. WAN networks span dozens of hops across continents; RIP cannot reach destinations beyond 15 hops.
2. **Bandwidth/Latency Blindness:**
   - RIP treats a $10\text{ Gbps}$ fiber link and a $64\text{ Kbps}$ satellite link as identical "1 hop", routing traffic over slow, congested paths.
3. **Slow Convergence & Count-to-Infinity:**
   - RIP uses Bellman-Ford, requiring multiple iteration intervals (up to minutes) to detect dead links, creating routing loops and blackholes.
4. **Excessive Periodic Overhead:**
   - Floods its entire routing table every 30 seconds via broadcast/multicast, consuming valuable WAN bandwidth.
   - **Conclusion:** Modern WANs require link-state (OSPF, IS-IS) or path-vector (BGP) protocols.

---
### Q.6c [4 Marks]
**Question:**
> How crash recovery is done at the transport layer in the Internet?

**Step-by-Step Solution:**
1. **The Core Dilemma:**
   - When a server crashes and reboots during an active connection, all in-memory Transmission Control Blocks (TCB) are lost.
   - When a packet arrives from the client with an unrecognized connection tuple, the rebooted server responds with a **`RST` (Reset)** flag segment.
2. **Crash Recovery Approaches & Limitations:**
   - **Client Recovery:** Upon receiving `RST`, client realizes server crashed. It may attempt reconnection.
   - **Application-Layer Coordination:** Transport layer alone cannot guarantee recovery if application operations are non-idempotent (e.g., banking fund transfers). The application must maintain persistent commit logs (e.g., Write-Ahead Logging) and check transaction state after transport re-establishment.
