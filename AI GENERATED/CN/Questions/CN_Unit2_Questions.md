# CN Unit II: Logical Link Control (Data Link Layer) — Question Bank

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Total Questions**: 12 (4 Short, 4 Medium, 4 Long/Numerical)  
*Note: Full worked solutions and marking schemes are provided in the separate file `CN_Unit2_Answers.md`.*

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Explain the **Bit Stuffing** procedure used in the HDLC protocol. Show how the following bit sequence is transformed by bit stuffing at the sender:
```
Original Bits:  0 1 1 1 1 1 1 0 1 1 1 1 1 0 0 1 1 1 1 1 0
```

### Q2. [4 Marks]
State the **Hamming rule inequality** relating the number of data bits `m` and redundant parity bits `r`. How many parity bits are required to encode an 8-bit ASCII character? State the bit positions occupied by the parity bits.

### Q3. [4 Marks]
In sliding window flow control protocols, what are the maximum allowable sender and receiver window sizes for:
(a) **Go-Back-N (GBN) ARQ** using an `m`-bit sequence number? [2M]  
(b) **Selective Repeat (SR) ARQ** using an `m`-bit sequence number? Explain why `W_S + W_R ≤ 2^m`. [2M]

### Q4. [4 Marks]
Differentiate between **PAP (Password Authentication Protocol)** and **CHAP (Challenge Handshake Authentication Protocol)** used in the Point-to-Point Protocol (PPP) in terms of handshake steps and security mechanism.

---

## Section B: Medium Analytical & Descriptive Questions (8 Marks Each)

### Q5. [8 Marks]
(a) Explain the three types of services that the Data Link Layer provides to the Network Layer: **Unacknowledged Connectionless**, **Acknowledged Connectionless**, and **Acknowledged Connection-Oriented**. State an appropriate application/protocol example for each. [4M]  
(b) Describe the **Byte Stuffing** framing method. Explain how escape bytes (`ESC`) are used when payload data contains either the `FLAG` byte or the `ESC` byte itself. [4M]

### Q6. [8 Marks]
A data bit sequence `1 0 0 1 1 0 1` is to be transmitted using a **Hamming Error-Correcting Code** with even parity:
1. Determine the number of parity bits required and the total length of the codeword. [2M]
2. Determine the bit values of all parity bits and write the complete transmitted codeword. [3M]
3. If during transmission, the 5th bit of the codeword is inverted (corrupted from 1 to 0), show mathematically how the receiver calculates the **Syndrome Word** and corrects the corrupted bit. [3M]

### Q7. [8 Marks]
(a) Derive the mathematical expression for the **efficiency (channel utilization) η** of the **Stop-and-Wait ARQ** protocol in terms of frame transmission time `T_trans` and one-way propagation time `T_prop`. [4M]  
(b) A 1-Gbps satellite link has a propagation delay of 270 ms. If data frames of size 1000 bytes (8000 bits) are transmitted using Stop-and-Wait ARQ, calculate:
    1. Transmission time `T_trans` [1M]
    2. Parameter `a = T_prop / T_trans` [1M]
    3. The channel utilization `η` and actual data throughput [2M]

### Q8. [8 Marks]
Explain the architecture of the **Point-to-Point Protocol (PPP)**:
1. Draw the complete PPP frame format and describe each field. [3M]
2. Explain the role of the **Link Control Protocol (LCP)** and the **Network Control Protocol (NCP / IPCP)**. [3M]
3. What is **PPPoE** and why is it widely used by modern broadband Internet service providers? [2M]

---

## Section C: Long Numerical & Comprehensive Problems (10 Marks Each)

### Q9. [10 Marks] (Cyclic Redundancy Check — Full Generation & Verification)
A message bit stream `D = 1 0 0 1 0 0` is to be transmitted using the standard CRC generator polynomial:
```
G(x) = x³ + x² + 1   (Binary: 1 1 0 1)
```
1. What is the degree `r` of the generator polynomial? How many zeros must be appended to the message? [2M]
2. Perform the step-by-step binary **modulo-2 division** to calculate the CRC remainder (checksum). Show all XOR division steps clearly. [4M]
3. Write down the complete transmitted codeword `T`. [1M]
4. Show the receiver's modulo-2 verification step:
   - Case A: The frame is received error-free. Prove the remainder is zero. [1.5M]
   - Case B: An error occurs during transmission, inverting the 3rd bit. Prove the remainder is non-zero and the error is detected. [1.5M]

### Q10. [10 Marks] (Go-Back-N vs Selective Repeat Timeline Simulation)
A sender uses a 3-bit sequence number (`m = 3`) to transmit frames numbered 0, 1, 2, 3, 4, 5 to a receiver over a high-latency link.
During transmission, **Frame 2 is lost due to channel noise**, but all other frames and ACKs travel safely.
1. Determine the maximum allowable sender window size `W_S` for:
   - Go-Back-N ARQ [1M]
   - Selective Repeat ARQ [1M]
2. Draw a neat, labeled **timeline interaction diagram** showing the sequence of frames sent, ACKs returned, timeouts, and retransmissions for:
   - **Go-Back-N ARQ** (Show how out-of-order frames 3, 4, 5 are handled by the receiver). [4M]
   - **Selective Repeat ARQ** (Show receiver buffering and selective retransmission). [4M]

### Q11. [10 Marks] (Sliding Window Optimal Design & Bandwidth-Delay Product)
A 100-km fiber optic link operates at a transmission rate of **100 Mbps**. The propagation speed of light in the fiber is `2 × 10⁸ m/s`.
Data frames are **1000 bytes (8000 bits)** in length, and acknowledgment frames are negligibly small.
1. Calculate the one-way propagation delay `T_prop` and the transmission time `T_trans`. [2M]
2. Calculate the **Bandwidth-Delay Product (BDP)** of the link in bits and in frames. [3M]
3. If the link uses **Stop-and-Wait ARQ**, calculate the channel utilization `η`. [2M]
4. What is the **minimum number of sequence number bits (m)** required to achieve **100% channel utilization** using:
   - Go-Back-N ARQ? [1.5M]
   - Selective Repeat ARQ? [1.5M]

### Q12. [10 Marks] (HDLC Protocol Deep Dive)
(a) Draw the complete **HDLC Frame Format** and explain the purpose of each field. [3M]  
(b) Explain the internal structure of the 8-bit **Control Field** for:
    1. **I-Frames (Information)**: Explain `N(S)` and `N(R)` and how **piggybacking** is achieved. [2M]
    2. **S-Frames (Supervisory)**: Explain the commands `RR`, `RNR`, `REJ`, and `SREJ`. [3M]
    3. **U-Frames (Unnumbered)**: Explain the commands `SABM` and `DISC`. [2M]
