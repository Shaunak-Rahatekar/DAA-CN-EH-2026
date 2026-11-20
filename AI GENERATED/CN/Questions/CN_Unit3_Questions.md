# CN Unit III: Medium Access Control (MAC Sublayer) — Question Bank

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Total Questions**: 12 (4 Short, 4 Medium, 4 Long/Numerical)  
*Note: Full worked solutions and marking schemes are provided in the separate file `CN_Unit3_Answers.md`.*

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Compare **Pure ALOHA** and **Slotted ALOHA** in terms of transmission rules, vulnerable periods, throughput formulas, and maximum achievable channel efficiency.

### Q2. [4 Marks]
Differentiate between **1-Persistent CSMA**, **Non-Persistent CSMA**, and **p-Persistent CSMA** in terms of channel sensing behavior and collision probability.

### Q3. [4 Marks]
In the **Binary Exponential Back-off (BEB)** algorithm used in Ethernet:
(a) How is the back-off delay calculated after the 3rd collision and after the 12th collision? [2M]  
(b) After how many collisions does the algorithm abort transmission, and why? [2M]

### Q4. [4 Marks]
Why can the collision detection mechanism of wired Ethernet (**CSMA/CD**) **NOT** be used directly in wireless networks (**IEEE 802.11**)? State the two fundamental physical reasons.

---

## Section B: Medium Analytical & Descriptive Questions (8 Marks Each)

### Q5. [8 Marks]
(a) Prove rigorously why the minimum frame transmission time in a CSMA/CD network must satisfy the condition:
```
T_trans ≥ 2 · T_prop
```
Draw a neat timeline diagram showing the worst-case collision scenario between two maximum-distance stations. [5M]  
(b) In a 10 Mbps Ethernet network, the one-way propagation delay is 25.6 μs. Calculate the theoretical minimum frame size in bits and in bytes. [3M]

### Q6. [8 Marks]
Draw the complete **IEEE 802.3 Ethernet Frame Format** and explain the function of each field:
1. Preamble and Start Frame Delimiter (SFD) [2M]
2. Destination and Source MAC Addresses [2M]
3. Type / Length field (explain how a receiver distinguishes between IEEE 802.3 and DIX Ethernet II) [2M]
4. Data payload and Padding requirement (explain why the frame must be at least 64 bytes) [2M]

### Q7. [8 Marks]
Explain the **Hidden Terminal Problem** and the **Exposed Terminal Problem** in wireless local area networks:
1. Draw a clear geometric node placement diagram illustrating both problems. [3M]
2. Explain how the **RTS / CTS (Request to Send / Clear to Send)** four-way handshake in **CSMA/CA** resolves the hidden terminal problem. [3M]
3. What is the role of the **NAV (Network Allocation Vector)**? [2M]

### Q8. [8 Marks]
Trace the evolution of Ethernet standards from legacy shared media to modern multi-gigabit speeds:
1. Compare **10BASE-T**, **100BASE-TX (Fast Ethernet)**, **1000BASE-T (Gigabit Ethernet)**, and **10GBASE-T** in terms of data rate, cabling, encoding, and duplex mode. [4M]
2. What technical dilemma occurred when scaling half-duplex CSMA/CD to Gigabit Ethernet? Explain the two mechanisms (**Carrier Extension** and **Frame Bursting**) introduced to resolve this issue. [4M]

---

## Section C: Long Numerical & Comprehensive Problems (10 Marks Each)

### Q9. [10 Marks] (ALOHA Mathematical Throughput Derivations)
(a) Using Poisson probability distribution for packet generation, derive the mathematical throughput equation for **Pure ALOHA**:
```
S = G · e^(-2G)
```
Prove that the maximum throughput is `S_max = 1 / (2e) ≈ 18.4%` and determine the exact traffic load `G` at which this maximum occurs. [5M]  
(b) Derive the mathematical throughput equation for **Slotted ALOHA**:
```
S = G · e^(-G)
```
Prove that the maximum throughput is `S_max = 1 / e ≈ 36.8%` at `G = 1.0`. Explain why the vulnerable period in Slotted ALOHA is half that of Pure ALOHA. [5M]

### Q10. [10 Marks] (CSMA/CD Network Design & Bandwidth Scaling Numerical)
A 2-km coaxial cable bus operates with a propagation speed of `2 × 10⁸ m/s`.
(a) If the network operates at **10 Mbps**:
    1. Calculate the one-way propagation delay `T_prop`. [1.5M]
    2. Calculate the minimum frame size `L_min` required for reliable collision detection. [2M]
(b) Suppose the network speed is upgraded to **100 Mbps (Fast Ethernet)** and then to **1 Gbps (Gigabit Ethernet)** while keeping the physical cable length fixed at 2 km:
    1. What would be the required minimum frame size at 100 Mbps and at 1 Gbps? [3M]
    2. Discuss why continuing to use CSMA/CD becomes technically impractical at Gigabit speeds, and explain how the networking industry resolved this limitation. [3.5M]

### Q11. [10 Marks] (Binary Exponential Back-off Simulation & Probability Analysis)
Two stations, Station A and Station B, simultaneously attempt to transmit over an idle 10 Mbps Ethernet bus and collide at time `t = 0`.
Both stations execute the standard IEEE 802.3 **Binary Exponential Back-off (BEB)** algorithm (Slot time = `51.2 μs`):
1. State the range of random integer `K` available to each station on:
   - The 1st retransmission attempt (`n = 1`) [1M]
   - The 2nd retransmission attempt (`n = 2`) [1M]
   - The 3rd retransmission attempt (`n = 3`) [1M]
2. Calculate the exact probability that Station A and Station B will **collide again** on their 1st retransmission attempt. [2.5M]
3. Calculate the probability that they will collide again on their 2nd retransmission attempt. [2.5M]
4. If Station A chooses `K = 1` and Station B chooses `K = 3` after the second collision, determine the exact time delay (in microseconds) that each station will wait before retransmitting. Which station wins the channel? [2M]

### Q12. [10 Marks] (IEEE 802.11 Wireless Architecture & Timing Sequence)
(a) Draw the complete **CSMA/CA timing diagram** showing the sequential transmission of:
    - DIFS period
    - RTS control frame
    - SIFS period
    - CTS control frame
    - SIFS period
    - DATA frame
    - SIFS period
    - ACK frame
    - NAV timer progression for surrounding stations. [5M]  
(b) Explain why **SIFS** is shorter than **DIFS**, and describe what would happen to network reliability if SIFS were longer than DIFS. [3M]  
(c) Why is an explicit Layer-2 **ACK frame** mandatory in IEEE 802.11 Wi-Fi, whereas standard wired Ethernet does not use Layer-2 acknowledgments? [2M]
