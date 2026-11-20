# CN Unit I: Networking Fundamentals & Physical Layer — Exam Cheat Sheet

**Course**: CS3213 | **Subject**: Computer Networks  
**Scope**: Communication Model, Topologies, Addressing Schemes, Devices & Domains, OSI vs TCP/IP, Line Coding, SDN

---

### 1. Data Transmission Modes & Classifications
- **Simplex**: Unidirectional only (Keyboard ➔ Monitor, TV broadcast).
- **Half-Duplex**: Bidirectional, one at a time (Walkie-talkie, CSMA/CD).
- **Full-Duplex**: Simultaneous bidirectional (Telephone, Switched Ethernet).
- **Networks**: PAN (~10m, Bluetooth), LAN (Km, Ethernet/Wi-Fi), MAN (City, Cable TV), WAN (Global, Internet).
- **Internet** (Global network) vs **internet** (any joined networks) vs **Intranet** (private internal network).

---

### 2. Network Topologies Summary

| Topology | Link Formula | I/O Ports | Fault Tolerance | Main Disadvantage |
| :--- | :---: | :---: | :--- | :--- |
| **Full Mesh** | `n(n-1)/2` | `n - 1` | High (independent links) | Costly $O(n^2)$ cabling & port overhead |
| **Star** | `n` | `1` | High (node fault isolated) | Single point of failure at central Hub/Switch |
| **Bus** | `1` backbone | `1` tap | Poor (cable break halts bus)| Difficult fault isolation; signal reflection |
| **Ring** | `n` | `2` | Poor (loop break halts net) | Unidirectional failure unless dual ring |

---

### 3. Addressing Schemes Across Layers

| Level | Layer | Size / Format | Example | Scope / Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Physical (MAC)** | Data Link (L2) | 48 bits (6 Bytes) hex | `00:1A:2B:3C:4D:5E` | Hop-to-hop in local subnet (flat) |
| **Logical (IP)** | Network (L3) | 32 bits (IPv4) / 128 (IPv6)| `192.168.1.1` | End-to-end host delivery (hierarchical) |
| **Port Address** | Transport (L4) | 16 bits (0 – 65,535) | Port 80 (HTTP), 443 | Identifies specific application process |
| **Specific** | Application (L7)| Human-readable string | `https://vit.edu` | User-friendly, resolved via DNS |

---

### 4. Connecting Devices & Domains

| Device | OSI Layer | Collision Domains | Broadcast Domains | Core Function |
| :--- | :---: | :---: | :---: | :--- |
| **Hub** | Layer 1 | **1** | **1** | Multi-port repeater; blindly floods electrical signals |
| **Switch** | Layer 2 | **N** (per port) | **1** | Hardware MAC table; full-duplex microsegmentation |
| **Router** | Layer 3 | **N** (per port) | **N** (per port) | IP Routing table; **breaks broadcast domains**! |

---

### 5. OSI 7-Layer vs TCP/IP Protocol Stack
- **OSI Model**: Physical (Bits) ➔ Data Link (Frames) ➔ Network (Packets) ➔ Transport (Segments) ➔ Session (Data) ➔ Presentation (Data) ➔ Application (Data).
- **TCP/IP Model**: Network Access (L1+L2) ➔ Internet (L3: IP) ➔ Transport (L4: TCP/UDP) ➔ Application (L5+L6+L7).

---

### 6. Digital Line Encoding Quick Reference

| Scheme | Bit 1 Rule | Bit 0 Rule | DC Component | Self-Clocking? |
| :--- | :--- | :--- | :---: | :---: |
| **Unipolar NRZ** | High (`+V`) | Zero (`0V`) | Very High | No (loses sync on runs) |
| **Polar NRZ-L** | Positive (`+V`) | Negative (`-V`) | Moderate | No (loses sync on runs) |
| **Polar NRZ-I** | **Transition** at start | **No transition** | Moderate | Only on runs of 1s |
| **Bipolar AMI** | Alternating `+V` / `-V` | Constant Zero (`0V`) | **Zero** | Error detection via violation |
| **Manchester** | Low ➔ High (mid-bit) | High ➔ Low (mid-bit) | **Zero** | **Yes (100% self-clocking)** |
| **Diff Manchester**| Mid-bit trans; **No trans** start | Mid-bit trans; **Trans** start | **Zero** | **Yes (differential sensing)** |

---

### 7. Software-Defined Networking (SDN)
- **Decoupling**: Separates the **Control Plane** (central controller, OpenFlow, routing policy) from the **Data Plane** (hardware switches, flow table packet forwarding).
- **APIs**: **Northbound APIs** (Apps ➔ Controller) & **Southbound APIs** (Controller ➔ Data Plane Switches via OpenFlow).
