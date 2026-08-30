# OSI 7-Layer Model: Data Encapsulation & Packet Journey

Every layer of the OSI model adds its own wrapper (header) containing specific identifiers that only that layer's hardware or software understands.

---

### The 7 Layers Broken Down by Data Unit and Addressing

| Layer | Layer Name | Data Unit (PDU) | What it Actually Knows & Uses | Concrete Example / Mechanics |
| :--- | :--- | :--- | :--- | :--- |
| **7** | **Application** | **Data** | User-facing protocols | Your browser speaks **HTTP/HTTPS** or your terminal speaks **SSH**. It generates the raw human request (e.g., `GET /index.html`). |
| **6** | **Presentation** | **Data** | Syntax, formats, encryption | Handles encryption (**SSL/TLS**) and formatting (**JSON, ASCII, JPEG**) so both ends understand the data structure. |
| **5** | **Session** | **Data** | Connection state | Controls the dialogue (starting, keeping open, closing) between your app and the remote server. |
| **4** | **Transport** | **Segment** (TCP) / **Datagram** (UDP) | **Port Numbers** (e.g., 80, 443, 22) | It chops big data into pieces and assigns **Ports** to route data to the correct application/process running on the machine. |
| **3** | **Network** | **Packet** | **IP Addresses** (e.g., `192.168.1.1`, `8.8.8.8`) | Routers only look at IP addresses to determine the path across global networks and internet routers from source to destination. |
| **2** | **Data Link** | **Frame** | **MAC Addresses** (e.g., `00:1A:2B:3C:4D:5E`) | Switches and network cards (NICs) only read MAC addresses to deliver the frame locally across your physical network switch or router. |
| **1** | **Physical** | **Bits** (1s and 0s) | Electrical signals, light pulses, radio frequencies | The physical mediums (Ethernet copper wires, fiber cables, Wi-Fi antennas) that convert bits into raw voltages or light pulses. |

---

### The Complete Journey of a Request (Encapsulation)

When you send a request (e.g., `curl https://example.com`):

1. **Layer 7–5 (Data):** Your terminal creates an HTTP payload: `"GET / HTTP/1.1"`.
2. **Layer 4 (Segment):** TCP adds a header with **Source Port: 54321** and **Destination Port: 443**.
3. **Layer 3 (Packet):** IP adds a header with **Source IP: 192.168.1.50** and **Destination IP: 93.184.216.34**.
4. **Layer 2 (Frame):** Ethernet adds a header with your computer's **Source MAC** and the local gateway switch's **Destination MAC**.
5. **Layer 1 (Bits):** The physical network card converts the entire frame into a stream of **1s and 0s** via electrical voltages down the Ethernet cable.

---

### On the Receiving End (De-encapsulation)

* **Physical Layer:** Receives the electrical voltages -> turns them into bits.
* **Data Link Layer:** Reads the **MAC address** on the **Frame** -> verifies it belongs to this device, strips the Layer 2 header, passes the **Packet** up.
* **Network Layer:** Reads the **IP address** on the **Packet** -> verifies it reached the right server, strips the Layer 3 header, passes the **Segment** up.
* **Transport Layer:** Reads the **Port number** on the **Segment** -> sees Port 443, strips the Layer 4 header, hands the raw **Data** to Nginx/Web Server.
* **Application Layer:** The web server processes the `GET /` request and returns a response back down the stack.
