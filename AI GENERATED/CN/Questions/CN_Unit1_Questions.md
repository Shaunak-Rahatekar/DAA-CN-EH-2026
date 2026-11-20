# CN Unit I: Networking Fundamentals & Physical Layer — Question Bank

**Course Code**: CS3213 | **Subject**: Computer Networks  
**Total Questions**: 12 (4 Short, 4 Medium, 4 Long/Numerical)  
*Note: Full worked solutions and marking schemes are provided in the separate file `CN_Unit1_Answers.md`.*

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Differentiate between **Simplex**, **Half-Duplex**, and **Full-Duplex** transmission modes. Provide a real-world communication example for each.

### Q2. [4 Marks]
Explain the differences between **Physical (MAC) Addresses**, **Logical (IP) Addresses**, and **Port Addresses** in terms of operating layer, address size, and delivery scope.

### Q3. [4 Marks]
What is the difference between a **Collision Domain** and a **Broadcast Domain**? How many collision domains and broadcast domains are created by an 8-port Hub, an 8-port Switch, and an 8-port Router?

### Q4. [4 Marks]
What is **Software-Defined Networking (SDN)**? State the main function of the **Control Plane** and the **Data Plane**, and name the standard protocol used between them.

---

## Section B: Medium Analytical & Descriptive Questions (8 Marks Each)

### Q5. [8 Marks]
Compare the **OSI 7-Layer Reference Model** and the **TCP/IP Protocol Suite**:
1. Draw a clean diagram showing the layer mapping between both models. [3M]
2. Explain how the presentation and session layer functionalities of OSI are handled in TCP/IP. [2M]
3. Provide a structured comparison table covering: number of layers, service vs protocol distinction, network layer connection mode, and market adoption. [3M]

### Q6. [8 Marks]
Compare the five fundamental network topologies: **Mesh**, **Star**, **Bus**, **Ring**, and **Hierarchical (Tree)**.
Present your answer as a comprehensive comparison table covering:
1. Physical link count formula for `n` nodes
2. Number of I/O ports required per node
3. Installation cost and cabling complexity
4. Fault tolerance and single point of failure

### Q7. [8 Marks]
Explain the concepts of **DC Component**, **Self-Synchronization (Clock Recovery)**, and **Baseline Wander** in digital line encoding. Why are signals with zero DC component and high transition density preferred in telecommunications?

### Q8. [8 Marks]
Briefly explain the following legacy communication technologies and discuss why they have been replaced in modern enterprise networks:
(a) **ATM (Asynchronous Transfer Mode)** [2M]  
(b) **ISDN (Integrated Services Digital Network)** [2M]  
(c) **Frame Relay** [2M]  
(d) **SNA (Systems Network Architecture)** [2M]

---

## Section C: Long Numerical & Comprehensive Problems (10 Marks Each)

### Q9. [10 Marks] (Line Encoding Waveforms)
Given the digital binary data stream:
```
Bit Stream:  0  1  0  0  1  1  1  0
```
Draw the physical voltage waveforms for each of the following line encoding schemes:
1. **Unipolar NRZ** [2M]
2. **Polar NRZ-L** (Level) [2M]
3. **Polar NRZ-I** (Invert on 1) [2M]
4. **Bipolar AMI** (Alternate Mark Inversion) [2M]
5. **Manchester Encoding** (IEEE 802.3 Standard) [2M]  
*(Clearly mark bit boundaries, clock mid-points, voltage levels +V, 0V, -V, and transition directions).*

### Q10. [10 Marks] (Topologies & Device Network Design)
A newly established branch office of an engineering college needs to connect 24 department laboratory computers and 6 faculty workstations across two floors:
(a) Calculate the exact number of physical cables and I/O ports per computer if a **Full Mesh Topology** is deployed for all 30 computers. Discuss whether this design is practical. [3M]  
(b) If the network is redesigned using a **Star Topology** with Layer-2 switches on each floor connected to a central building router:
    1. How many links are required? [2M]
    2. Determine the total number of **Collision Domains** and **Broadcast Domains** in the entire building network. [3M]
    3. What happens if one department switch fails versus if the central router fails? [2M]

### Q11. [10 Marks] (OSI Protocol Data Units & Encapsulation)
Trace the journey of a user requesting a web page (`http://www.vit.edu`) from the application layer down to the physical wire:
1. Identify the specific **Protocol Data Unit (PDU)** name at each of the 7 OSI layers. [3M]
2. Explain the process of **Header Encapsulation** as data travels down the protocol stack from Layer 7 to Layer 1, detailing the specific addressing information added at Layers 4, 3, and 2. [4M]
3. Explain the reverse process of **Decapsulation** at the receiving server. [3M]

### Q12. [10 Marks] (Manchester vs Differential Manchester & Transmission Metrics)
(a) Draw and compare the waveforms for the bit sequence `1 0 1 1 0 0 1` using:
    1. **Manchester Encoding** (IEEE 802.3: 0 = High-to-Low, 1 = Low-to-High). [3M]
    2. **Differential Manchester Encoding** (Transition at beginning for 0, no transition for 1). [3M]  
(b) If a local area network transmits data at a bit rate of **10 Mbps** using Manchester encoding:
    1. What is the required **Baud Rate (Signal Rate)**? Explain the relationship between bit rate and baud rate. [2M]
    2. Explain why Manchester encoding cannot be used directly for Gigabit Ethernet (1000 Mbps), and name the encoding scheme adopted by modern high-speed Ethernet standards. [2M]
