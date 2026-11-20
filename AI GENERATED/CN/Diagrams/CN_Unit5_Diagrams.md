# Computer Networks (CS3213) — Unit V: Transport Layer Diagrams

This document contains high-resolution reference diagrams, exam visual breakdowns, and hand-drawable ASCII blueprints for fast reproduction during the 2-hour written examination.

---

## Diagram 1: TCP vs UDP Segment Header Formats

![TCP vs UDP Header Formats](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit5_Diagram1_TCP_vs_UDP_Headers.png)

### Hand-Drawn Exam Redraw Guide (ASCII Blueprint)
When asked to *"Draw and explain the TCP segment header format and compare it with UDP"* (typically 6 to 8 Marks):

#### TCP Header (20 to 60 Bytes, 32-bit aligned):
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |  Row 0 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |  Row 1 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |  Row 2 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|DataOff| Reserv|C|E|U|A|P|R|S|F|          Window Size          |  Row 3 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |        Urgent Pointer         |  Row 4 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options + Padding (0 - 40 Bytes)           |  Row 5 (0-40 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### UDP Header (Fixed 8 Bytes):
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |  Row 0 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |  Row 1 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Key TCP Fields & Exam Scoring Points:
1. **Source / Destination Port (16 bits each)**: Identifies sending and receiving application processes.
2. **Sequence Number (32 bits)**: Byte stream numbering. Identifies the sequence number of the first data byte in this segment.
3. **Acknowledgment Number (32 bits)**: Cumulative acknowledgment. Contains the sequence number of the *next expected byte* from the other party.
4. **Data Offset / HLEN (4 bits)**: Number of 32-bit words in header (5 = 20B, 15 = 60B).
5. **Control Flags (8 bits)**:
   - `URG`: Urgent pointer valid.
   - `ACK`: Acknowledgment field valid (always 1 after initial SYN).
   - `PSH`: Push function (deliver data to app immediately, don't wait for buffer).
   - `RST`: Reset connection (reject invalid segment or abort).
   - `SYN`: Synchronize sequence numbers (connection initiation).
   - `FIN`: Finish (no more data from sender; connection teardown).
   - `CWR / ECE`: Congestion Window Reduced / Explicit Congestion Echo.
6. **Window Size (16 bits)**: Advertises receiver buffer space (`rwnd`) for flow control.
7. **Checksum (16 bits)**: Mandatory 1's complement sum covering pseudo-header (IPs), TCP header, and data.

---

## Diagram 2: TCP 3-Way Handshake & 4-Way Teardown

![TCP Handshake and Teardown](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit5_Diagram2_ThreeWayHandshake_Teardown.png)

### Hand-Drawn Exam Redraw Guide
```
      [ 3-Way Handshake (Establishment) ]               [ 4-Way Handshake (Teardown) ]

      Client (Active)         Server (Passive)        Client (Active)         Server (Passive)
           |                         |                     |                         |
  SYN_SENT | --- SYN=1, Seq=x -----> | LISTEN     FIN_WAIT_1 | --- FIN=1, Seq=u -----> | ESTABLISHED
           |                         | SYN_RCVD            |                         | CLOSE_WAIT
           | <--- SYN=1, ACK=1 ----- |            FIN_WAIT_2 | <--- ACK=1, Ack=u+1 --- |
ESTABLISHED|      Seq=y, Ack=x+1     |                     |                         |
           |                         |                     | <--- FIN=1, Seq=w ----- | LAST_ACK
           | --- ACK=1, Ack=y+1 ---> |                     |      Ack=u+1            |
           |                         | ESTABLISHED TIME_WAIT| --- ACK=1, Ack=w+1 ---> | CLOSED
           v                         v            (2*MSL)  v                         v
                                                   CLOSED
```

### High-Yield Exam Defense:
- **Why 3-way instead of 2-way?** Prevents obsolete, delayed duplicate SYN segments from establishing erroneous half-open connections at the server.
- **Why TIME_WAIT = 2 * MSL (Maximum Segment Lifetime)?**
  1. Ensures final ACK reaches the server (if lost, server retransmits FIN).
  2. Allows all wandering duplicate segments from old connection to completely die out before port reuse.

---

## Diagram 3: TCP Congestion Control (Tahoe vs Reno)

![TCP Congestion Control](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit5_Diagram3_CongestionControl_Tahoe_Reno.png)

### Hand-Drawn Exam Redraw Guide
```
  cwnd (MSS)
   ^
32 |
24 |                                  Triple Dup ACK! (R12)
   |                                     *  <--- TCP Reno enters Fast Recovery (cwnd = ssthresh = 12)
16 |             * (ssthresh)           / \
   |           /   \                   /   \
 8 |         /       \ (linear +1)    /     \ (linear +1)
   |       /           \             /       \
 4 |     * (exp x2)      \          /         * Timeout at R18!
 2 |   *                   \       /          | (Both drop to 1)
 1 | *                      * ----            * (cwnd = 1)
   +----------------------------------------------------------------->
     0   1   2   3   4   5   ...  12  13  ... 18  19  20  Transmission Rounds (RTT)
       [ Slow Start ]  [ Congestion Avoidance ]
```

### Congestion Control Mathematical Rules:
1. **Slow Start**: `cwnd < ssthresh`. For every valid ACK received, `cwnd = cwnd + 1 MSS`. (Doubles every RTT: 1, 2, 4, 8, 16...).
2. **Congestion Avoidance (AIMD)**: `cwnd ≥ ssthresh`. Additive Increase: For every RTT, `cwnd = cwnd + 1 MSS`.
3. **On Timeout (Severe Congestion)**:
   - `ssthresh = max(cwnd / 2, 2 MSS)`.
   - `cwnd = 1 MSS` (drops to 1 for both Tahoe and Reno; resumes Slow Start).
4. **On 3 Duplicate ACKs (Mild Congestion / Isolated Packet Loss)**:
   - `ssthresh = max(cwnd / 2, 2 MSS)`.
   - **TCP Tahoe**: Drops `cwnd = 1 MSS` (Slow Start).
   - **TCP Reno (Fast Retransmit & Fast Recovery)**: Retransmits missing segment immediately; sets `cwnd = ssthresh` (skips Slow Start, resumes Congestion Avoidance).

---

## Diagram 4: Sliding Window Flow Control & Silly Window Syndrome

![Sliding Window and SWS](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit5_Diagram4_SlidingWindow_FlowControl.png)

### Hand-Drawn Exam Redraw Guide
```
  [ Sender Window Partitions ]
  +------------------+------------------+------------------+------------------+
  | 1. Sent & ACKed  | 2. Sent, Not ACK | 3. Can Send Now  | 4. Cannot Send   |
  | (Can discard)    | (Retain buffer)  | (Usable Window)  | (Blocked)        |
  +------------------+------------------+------------------+------------------+
                     ^                  ^                  ^
                 LastByteAcked     LastByteSent      Usable Edge
                     |<------- Advertised rwnd ----------->|

  [ Silly Window Syndrome Solutions ]
  - Sender: Nagle's Algorithm -> Send 1st byte, then buffer tiny data until full MSS OR ACK received.
  - Receiver: Clark's Solution -> Advertise rwnd = 0 until free buffer >= min(MSS, Buffer_Total / 2).
```

---

## Diagram 5: QUIC Protocol vs Traditional TCP+TLS Stack

![QUIC vs TCP Stack](file:///D:/2026%20EXAM/AI%20GENERATED/CN/Diagrams/CN_Unit5_Diagram5_QUIC_vs_TCP_Stack.png)

### Hand-Drawn Exam Redraw Guide
```
      [ Traditional Stack (HTTP/2) ]               [ Modern Stack (HTTP/3) ]
     +------------------------------+             +------------------------------+
     |   HTTP/2 (Muxed Streams)     |             |            HTTP/3            |
     +------------------------------+             +------------------------------+
     |   TLS 1.2 / 1.3 (Security)   |             |  QUIC (Stream Mux + TLS 1.3  |
     +------------------------------+             |  + Congestion Control + CID) |
     |      TCP (Byte Stream)       |             +------------------------------+
     +------------------------------+             |       UDP (User Datagram)    |
     |         IP (Network)         |             +------------------------------+
     +------------------------------+             |         IP (Network)         |
                                                  +------------------------------+
```

### Three Architectural Triumphs of QUIC (10-Mark Question Points):
1. **0-RTT Connection Establishment**: Merges transport and TLS 1.3 handshakes into a single round trip (or 0-RTT with cached session tickets).
2. **Elimination of Head-of-Line (HoL) Blocking**: Multi-stream multiplexing is natively handled at transport layer. Dropping a packet in Stream 1 never blocks delivery of data in Streams 2, 3, or 4!
3. **Connection Migration via Connection ID (CID)**: Connections are bound to a 64-bit CID rather than the IP:Port 4-tuple. When a mobile device switches from Wi-Fi to cellular data (changing IP), streaming and transfers continue without reconnection.
