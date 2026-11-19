## 8. Practical Lab
Open your Linux terminal and perform these tasks.

### Task 1 — Inspect Your Network
Run the following commands to inspect your interfaces, IP addresses, and routing table:

```bash
ip link
ip addr
ip route
```

Look closely at the output and identify:
* **Your network interface names:** Look for entries like `lo` (loopback) and your active interface (such as `eth0`, `enp*`, or `wlan*`).
* **Your assigned IP address:** Look under your active interface for the `inet` line (e.g., `192.168.1.x/24` or `10.0.2.x/24`).
* **Your default gateway:** Look in `ip route` for the line starting with `default via <IP>`—that IP is your default gateway.

### Task 2 — Test Connectivity
Send 4 ICMP echo requests (`-c 4`) to `8.8.8.8`:

```bash
ping -c 4 8.8.8.8
```

Observe the summary output and note:
* **How many packets were transmitted?** (Should be `4` if completed.)
* **How many were received?** (Look at the packet loss / received count in the summary line.)
* **What is the average response time?** (Look at the `rtt min/avg/max/...` line and note the `avg` value in `ms`.)

### Task 3 — Inspect Sockets
List listening TCP and UDP sockets with numeric addresses/ports:

```bash
ss -tuln
```

Observe the listening ports shown (e.g., `Local Address:Port`). Don't worry if some addresses or port numbers look unfamiliar—we'll cover ports and sockets in more detail in upcoming lessons.

---

## 9. Knowledge Check
Try answering these questions from memory before checking the answers below:

1. **What is the difference between LAN and WAN?**
   A **LAN** (Local Area Network) connects devices within a small, local area (such as a home, office, or lab), whereas a **WAN** (Wide Area Network) connects networks across large geographical distances (such as cities, countries, or the internet).
2. **What is the main responsibility of a router?**
   A router connects different networks together and forwards packets between them toward their destination.
3. **What is the difference between a switch and a router?**
   A **switch** connects devices within the **same** local network (forwarding frames using MAC addresses), while a **router** connects **different** networks together and routes traffic between them.
4. **In client–server architecture, which device initiates a request?**
   The **client** initiates the request, and the **server** processes it and sends back a response.
5. **Why is data divided into packets?**
   Breaking data into smaller packets allows network links to be shared efficiently across traffic streams, handles congestion better, and avoids retransmitting huge blocks of data if part of a transmission is lost.
6. **What is the difference between TCP and UDP?**
   **TCP** provides connection-oriented, reliable, ordered delivery (with error checking/retransmission), while **UDP** is connectionless and lightweight (datagram-based) without delivery guarantees or ordering overhead.
7. **What does `ip route` display?**
   It displays the kernel's routing table—showing which interface and gateway (such as the default gateway) Linux uses to reach different network destinations.
8. **What does `ping` help you test?**
   It tests basic network reachability and round-trip latency to a target host using ICMP echo requests and replies.