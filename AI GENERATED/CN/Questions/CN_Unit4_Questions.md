# Computer Networks (CS3213) — Unit IV: Network Layer Exam Questions

**Target Exam**: End-Semester Written Examination  
**Pattern**: 4 Marks (Short Answer), 8 Marks (Analytical / Numerical), 10 Marks (Comprehensive Long Answer)

---

## Section 1: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Differentiate between Datagram Packet Switching and Virtual Circuit Packet Switching with respect to connection setup, routing overhead, packet delivery ordering, and resilience to intermediate node failures.

### Q2. [4 Marks]
For the IP address `172.16.20.0/22`, determine:
1. Subnet mask in dotted decimal notation
2. Total number of host addresses and number of usable host addresses
3. Network ID
4. Directed broadcast address

### Q3. [4 Marks]
Explain the functions of the following fields in the IPv4 datagram header:
1. `Internet Header Length (IHL)`
2. `Time to Live (TTL)`
3. `Protocol`
4. `Header Checksum`  
State the exact ICMP message returned when TTL decrements to 0.

### Q4. [4 Marks]
Describe the operation of the Address Resolution Protocol (ARP). Distinguish between an ARP Request and an ARP Reply in terms of transmission mode (unicast vs broadcast). Briefly state the purpose of Gratuitous ARP.

### Q5. [4 Marks]
What causes the Count-to-Infinity problem in Distance Vector routing? Explain how Split Horizon and Poison Reverse techniques resolve this issue.

### Q6. [4 Marks]
Compare the Leaky Bucket algorithm with the Token Bucket algorithm for network traffic regulation. Why is Token Bucket preferred for bursty computer data traffic?

---

## Section 2: Analytical & Numerical Questions (8 Marks Each)

### Q7. [8 Marks]
1. Draw the complete 32-bit aligned IPv4 Datagram Header format, clearly labeling all 14 fields and their bit lengths. *(4 Marks)*
2. An IPv4 datagram of total size 4000 bytes (comprising 20 bytes of IP header and 3980 bytes of data payload) arrives at a router whose outgoing link has an MTU of 1500 bytes. Determine the number of fragments generated, and for each fragment, calculate:
   - Total Length
   - More Fragments (MF) flag
   - Fragment Offset *(4 Marks)*

### Q8. [8 Marks]
An organization is granted the address block `130.34.12.64/26`. The organization needs to partition this block into **4 subnets** with an equal number of hosts in each subnet.  
Design the subnetting scheme and determine:
1. Subnet mask in dotted decimal notation and CIDR slash format.
2. Total number of IP addresses per subnet.
3. Number of usable host IP addresses per subnet.
4. For all 4 subnets, construct a complete allocation table listing:
   - Subnet Index
   - Network ID
   - First Usable Host IP
   - Last Usable Host IP
   - Directed Broadcast Address

### Q9. [8 Marks]
Provide an exhaustive comparative analysis between **Distance Vector Routing** and **Link State Routing** across the following parameters:
1. Underlying mathematical algorithm
2. Information shared (what is sent)
3. Recipients of routing information (who receives it)
4. Update frequency and triggers
5. Convergence speed and loop susceptibility
6. Router CPU and memory overhead
7. Real-world Internet protocol examples

### Q10. [8 Marks]
1. Explain the mechanism of Network Address Translation (NAT) and Port Address Translation (PAT / NAT Overload). Why is PAT essential in modern IPv4 networks? *(4 Marks)*
2. Consider two internal hosts `Host A (192.168.1.10)` and `Host B (192.168.1.20)` behind a NAT router with single public IP `203.0.113.5`. Show using a step-by-step NAT translation table how concurrent HTTP sessions to a web server `93.184.216.34:80` are disambiguated and forwarded. *(4 Marks)*

---

## Section 3: Comprehensive Long Answer Questions (10 Marks Each)

### Q11. [10 Marks]
1. Compare IPv4 and IPv6 architectures in exhaustive detail. Draw the IPv6 base header format (40 Bytes) and explain each of its 8 fields. *(5 Marks)*
2. State three architectural design reasons why IPv6 completely dropped the **Header Checksum** and **Router Fragmentation** fields found in IPv4. *(2 Marks)*
3. Explain the three standard migration mechanisms used to transition existing network infrastructure from IPv4 to IPv6: Dual Stack, Tunneling, and Header Translation (NAT64). *(3 Marks)*

### Q12. [10 Marks]
1. Consider a network of 5 routers `(A, B, C, D, E)` with the following bidirectional link costs:
   - `(A, B) = 3`, `(A, C) = 1`, `(B, C) = 7`, `(B, D) = 5`, `(B, E) = 1`, `(C, D) = 2`, `(D, E) = 7`
   Using **Dijkstra's Shortest Path First (SPF)** algorithm with **Node A as the source**, show the step-by-step execution table tracing the Permanent Set of vertices `S`, tentative distances `D(v)`, and predecessors `p(v)` at every iteration. Construct the final Shortest Path Tree and the resulting routing table for Router A. *(6 Marks)*
2. Write concise technical notes on:
   - **Border Gateway Protocol (BGP-4)**: Explain why BGP is classified as a Path Vector protocol and how the `AS-PATH` attribute prevents routing loops between Autonomous Systems. *(2 Marks)*
   - **Multi-Protocol Label Switching (MPLS)**: Explain the function of the 32-bit shim header, Label Edge Routers (LER), and Label Switch Routers (LSR). *(2 Marks)*
