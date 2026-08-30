# TCP vs UDP Transport Layer Master Guide

A complete reference on Transport Layer (Layer 4) mechanics, packet headers, control flags, connection handshakes, flow control, and terminal inspection tools.

---

### 1. Transport Layer Units: Segments vs. Datagrams

Both protocols operate at **Layer 4** to route data to the correct application using **Port Numbers** (e.g., Port 80/443 for Web, Port 22 for SSH, Port 53 for DNS).

* **TCP uses Segments:** Adds sequence numbers, acknowledgement numbers, and checksums to guarantee delivery.
* **UDP uses Datagrams:** Adds only source port, destination port, length, and a lightweight checksum.

---

### 2. Control Flags Explained

| Flag | Name | Function & Purpose |
| :--- | :--- | :--- |
| **`SYN`** | Synchronize | Initiates connection handshake; establishes starting Sequence Numbers ($ISN$). |
| **`ACK`** | Acknowledge | Confirms receipt of bytes/flags; specifies the expected next byte sequence number. |
| **`FIN`** | Finish | Initiates graceful connection closure after payload transmission ends. |
| **`RST`** | Reset | Instantly terminates/rejects an invalid or broken connection. |
| **`PSH`** | Push | Tells the receiving OS to bypass buffer caching and pass data straight to the app. |
| **`URG`** | Urgent | Marks segment payload as priority for immediate processing. |

---

### 3. Connection Handshakes

#### The 3-Way Handshake (Establishment)
1. **Client $\rightarrow$ Server (`SYN`, Seq=100):** "Requesting connection. Starting sequence is 100."
2. **Server $\rightarrow$ Client (`SYN-ACK`, Seq=300, Ack=101):** "Accepted. My starting sequence is 300. Expecting byte 101."
3. **Client $\rightarrow$ Server (`ACK`, Ack=301):** "Confirmed. Socket open. Transmitting data."

#### The 4-Way Handshake (Graceful Teardown)
1. **Client $\rightarrow$ Server (`FIN`):** "I am finished sending data."
2. **Server $\rightarrow$ Client (`ACK`):** "Understood. Finishing my pending tasks."
3. **Server $\rightarrow$ Client (`FIN`):** "I am finished sending data too."
4. **Client $\rightarrow$ Server (`ACK`):** "Understood. Connection closed."

---

### 4. Reliability, Flow Control & Head-of-Line Blocking

* **Sequencing & Reordering:** If packets arrive out of order ($3, 1, 2$), the receiving buffer uses Sequence numbers to reconstruct $1, 2, 3$.
* **Sliding Window (Flow Control):** Both endpoints advertise buffer capacity (`Win`) in headers, preventing fast senders from overloading slow receivers.
* **No Head-of-Line Blocking in UDP:** In TCP, if packet 1 drops, packets 2 and 3 must wait in memory. In UDP, lost packets are ignored and real-time streaming continues uninterrupted.

---

### 5. Head-to-Head Comparison

| Feature | TCP | UDP |
| :--- | :--- | :--- |
| **Connection Style** | Stateful (Handshake required) | Stateless (Fire-and-forget) |
| **PDU Unit** | Segment | Datagram |
| **Delivery Guarantee** | Guaranteed (ACK retransmissions) | None (Packets dropped) |
| **Ordering** | Guaranteed sequential | No guarantee |
| **Header Overhead** | 20 to 60 Bytes | 8 Bytes fixed |
| **Congestion Control** | Built-in backoff algorithms | None |
| **Protocols** | HTTP/1.1, HTTP/2, HTTPS, SSH, SMTP | DNS, DHCP, WebRTC, VoIP, QUIC (HTTP/3) |

---

### 6. Linux Socket Inspection Commands

```bash
# View all listening TCP ports with process names and numeric ports
ss -tlpn

# View all active UDP sockets
ss -ulpn

# Trace network routing hops to destination
traceroute google.com
