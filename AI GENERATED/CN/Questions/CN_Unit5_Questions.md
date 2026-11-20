# Computer Networks (CS3213) — Unit V: Transport Layer Exam Questions

**Target Exam**: End-Semester Written Examination  
**Pattern**: 4 Marks (Short Answer), 8 Marks (Analytical / Numerical), 10 Marks (Comprehensive Long Answer)

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Differentiate between TCP and UDP with respect to connection establishment, data stream paradigm, reliability, header overhead, and flow control. Mention two application examples for each.

### Q2. [4 Marks]
Explain the lifecycle of the Berkeley Sockets API on the server side (`socket`, `bind`, `listen`, `accept`, `recv`, `send`, `close`). Specifically explain what value the `accept()` system call returns and why it does not reuse the listening socket.

### Q3. [4 Marks]
Draw the timing sequence of the TCP 3-way handshake. Explain why a 2-way handshake is inadequate and vulnerable to half-open zombie connections in packet networks.

### Q4. [4 Marks]
Briefly explain the functions of the four timers maintained by a TCP entity:
1. Retransmission Timer (RTO)
2. Persistence Timer
3. Keep-Alive Timer
4. Time-Wait Timer (Explain why its duration is set to `2 × MSL`).

### Q5. [4 Marks]
What is Silly Window Syndrome (SWS)? State the catastrophic transmission efficiency it produces. Explain how **Nagle's Algorithm** at the sender and **Clark's Solution** at the receiver eliminate this syndrome.

### Q6. [4 Marks]
Explain Jacobson's algorithm for calculating the dynamic Retransmission Timeout (RTO) in TCP. State **Karn's algorithm rule** and explain why RTT samples must never be collected for retransmitted segments.

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. [8 Marks]
1. Draw the complete 32-bit word-aligned TCP Segment Header format, labeling all fields and control flags with their exact bit lengths. *(4 Marks)*
2. Explain the purpose and operation of:
   - Sequence Number and Acknowledgment Number
   - Window Size (`rwnd`)
   - Control flags: `SYN`, `FIN`, `RST`, `PSH`, and `URG`. *(4 Marks)*

### Q8. [8 Marks]
A TCP Reno connection has an initial slow start threshold `ssthresh = 16 MSS` and starts transmission at Round 1 with `cwnd = 1 MSS`.  
1. At transmission Round 8, the sender receives a **Triple Duplicate ACK**.
2. At transmission Round 12, a **Retransmission Timeout (RTO)** occurs.  
Construct a complete trace table showing the `cwnd` value (in MSS) and protocol operational phase (Slow Start, Congestion Avoidance, Fast Recovery) for all transmission rounds from 1 to 14. Contrast how **TCP Tahoe** and **TCP Reno** behave after the triple duplicate ACK in Round 8.

### Q9. [8 Marks]
1. Explain how TCP achieves end-to-end Flow Control using the Sliding Window protocol. Distinguish between the Receive Window (`rwnd`) and the Congestion Window (`cwnd`). *(4 Marks)*
2. A TCP sender currently has `cwnd = 8000 Bytes` and receives an acknowledgment segment carrying `rwnd = 5000 Bytes`. If the sender's state variables are `LastByteSent = 12,000` and `LastByteAcked = 9,000`, calculate:
   - Total unacknowledged data in flight
   - Current maximum allowed window
   - Effective usable send window (bytes the sender can transmit immediately without waiting). *(4 Marks)*

### Q10. [8 Marks]
1. Draw the complete timing sequence of TCP Connection Teardown using the 4-way handshake, labeling the states traversed by both the active closer and the passive closer (`ESTABLISHED`, `FIN_WAIT_1`, `FIN_WAIT_2`, `CLOSE_WAIT`, `LAST_ACK`, `TIME_WAIT`, `CLOSED`). *(5 Marks)*
2. Explain the concept of a **half-closed connection** (`CLOSE_WAIT` state). Why does TCP allow data transmission to continue after one party has sent its `FIN` segment? *(3 Marks)*

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. [10 Marks]
1. Discuss the architectural and practical limitations of the traditional `HTTP/2 over TLS over TCP` protocol stack that motivated the development of the **QUIC protocol** (consider kernel ossification, middlebox interference, handshake round trips, and transport-layer head-of-line blocking). *(5 Marks)*
2. Draw the comparative protocol stack diagram of traditional TCP/TLS vs QUIC/UDP. Explain in detail the three core innovations of QUIC:
   - **0-RTT and 1-RTT Connection Handshake** (built-in TLS 1.3)
   - **Elimination of Head-of-Line (HoL) Blocking across multi-stream multiplexing**
   - **Connection Migration using 64-bit Connection ID (CID)**. *(5 Marks)*

### Q12. [10 Marks]
1. A TCP connection dynamically measures Round-Trip Times using Jacobson's algorithm with weighting factors `α = 0.125` and `β = 0.25`.  
   Suppose the initial state is `Smoothed RTT (RTT_s) = 40 ms` and `Deviation (D) = 5 ms`.  
   Successive rounds yield the following sample RTT measurements:
   - Round 1: Sample `RTT_m = 48 ms`
   - Round 2: Sample `RTT_m = 32 ms`
   - Round 3: Sample `RTT_m = 60 ms`  
   Calculate the updated `RTT_s`, `Deviation (D)`, and Retransmission Timeout (`RTO = RTT_s + 4D`) at the end of each round. Present the step-by-step arithmetic in a neat calculation table. *(6 Marks)*
2. Explain the **Additive Increase Multiplicative Decrease (AIMD)** principle in TCP congestion control. Why does AIMD mathematically converge to both optimal throughput efficiency and fair resource allocation among competing flows, whereas Additive Increase Additive Decrease (AIAD) or Multiplicative Increase Multiplicative Decrease (MIMD) fail? *(4 Marks)*
