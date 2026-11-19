## 9. Practical Lab

### Task 1 — IP Inspection

**1. Find your IPv4 address.**
```bash
ip -4 addr show scope global
```
Or quickly:
```bash
hostname -I
```
* **Answer / Expected output:** Look for the address next to `inet` (e.g., `192.168.1.10/24`) or the first IPv4 address returned by `hostname -I`.

**2. Find your IPv6 address, if available.**
```bash
ip -6 addr show scope global
```
Or view all IPv6 addresses (including link-local `fe80::...`):
```bash
ip -6 addr show
```
* **Answer / Expected output:** Look for lines starting with `inet6` (e.g., `2001:db8::...` for global or `fe80::...` for link-local). If none appear, IPv6 is not configured/available on that interface.

**3. Identify your network interface.**
```bash
ip -o link show up | awk -F': ' '{print $2}'
```
Or check the interface used for default traffic:
```bash
ip route show default | awk '{print $5}'
```
* **Answer / Expected output:** Common names are `eth0` or `enp...` for Ethernet, and `wlan0` or `wlp...` for Wi-Fi (plus `lo` for loopback).

**4. Find your default gateway.**
```bash
ip route | grep default
```
* **Answer / Expected output:** Look for the IP after `via` on the `default` line, for example:
```text
default via 192.168.1.1 dev wlan0 proto dhcp metric 600
```
Here, `192.168.1.1` is the default gateway.

**5. Determine whether your IPv4 address is private or public.**
* **Answer:** Check if your IPv4 address falls inside any of the private IPv4 ranges:
  * `10.0.0.0 – 10.255.255.255`
  * `172.16.0.0 – 172.31.255.255`
  * `192.168.0.0 – 192.168.255.255`
  If it is within these ranges (such as `192.168.1.10`), it is **private** (used on a local network behind a router/NAT). If it is outside these ranges and routable on the internet, it is **public**.

---

### Task 2 — Ports

**1. Execute `sudo ss -tulnp`.**
```bash
sudo ss -tulnp
```
* **Answer / Expected output:** Lists active **TCP** (`-t`) and **UDP** (`-u`) **listening** (`-l`) sockets using **numeric** IPs/ports (`-n`) along with the **process** (`-p`) name and PID.

**2. Identify three listening ports, if available.**
```bash
sudo ss -tulnp | grep LISTEN
```
* **Answer:** Inspect the `Local Address:Port` column (e.g., `0.0.0.0:22`, `127.0.0.53:53`, `127.0.0.1:631`, or `*:3000`) and note any three ports currently in a listening state.

**3. Identify which processes are using them.**
* **Answer:** Look at the rightmost `Users:` column in the `ss -tulnp` output for each of those three ports. It displays the process name and PID in the format `users:(("process_name",pid=1234,fd=3))` (for example, `sshd`, `systemd-resolve`, `node`, or `postgres`).

**4. Check whether port 22 is listening.**
```bash
sudo ss -tulnp | grep ':22\b'
```
* **Answer:** If a line showing `:22` appears (such as `0.0.0.0:22` or `[::]:22`), port 22 is listening (typically for SSH). If nothing is returned, port 22 is not currently listening—which is normal if an SSH server (`sshd`) is not installed or stopped.

---

### Task 3 — CIDR Reasoning

**Addresses:**
* **A:** `192.168.10.15/24`
* **B:** `192.168.10.200/24`
* **C:** `192.168.11.15/24`
* **D:** `192.168.10.50/24`

**Answer & Reasoning:**
* **A, B, and D belong to the same network (`192.168.10.0/24`).**
* **C belongs to a different network (`192.168.11.0/24`).**

A `/24` prefix (subnet mask `255.255.255.0`) means the **first 24 bits**—the first three octets—define the network portion, while the last octet identifies the host.
* `192.168.10.15/24`, `192.168.10.200/24`, and `192.168.10.50/24` all share the network prefix `192.168.10.x`.
* `192.168.11.15/24` has `11` in the third octet instead of `10`, placing it in a separate `/24` subnet.

---

## 10. Knowledge Check

1. **What is the difference between a public and private IP address?**
   A **private IP** is used inside a local network (home, school, office, or private cloud subnet) and cannot be routed directly on the public internet. A **public IP** is globally unique/routable on the internet so external devices and users can reach a network endpoint (such as a public server or router).

2. **What problem does NAT solve?**
   **NAT (Network Address Translation)** solves IPv4 address exhaustion and allows multiple devices with private IP addresses on a local network to communicate with external networks/the internet using a single (or shared pool of) public IP address, translating addresses and tracking active connections so return traffic reaches the right device.

3. **What does `/24` mean in `192.168.1.10/24`?**
   `/24` is CIDR notation indicating that the **first 24 bits** of the 32-bit IPv4 address (the first three octets, `192.168.1`) form the **network** portion, leaving the remaining 8 bits (the fourth octet) for host addresses within that subnet.

4. **What is the difference between an IP address and a port?**
   An **IP address** identifies the specific device or network interface on a network (like a street/building address), while a **port** (from `0` to `65535`) identifies the specific application, service, or process on that device that sends or receives the traffic.

5. **Why can one server run multiple services simultaneously?**
   Network connections are identified by combinations of IP address, protocol (TCP/UDP), and port number. Because different services can bind to different ports on the same IP address (for example, `22` for SSH, `80` for HTTP, `443` for HTTPS, `5432` for PostgreSQL), the OS kernel routes incoming packets to the right service based on the destination port.

6. **What is the difference between TCP and UDP?**
   **TCP** is connection-oriented and provides reliable, ordered, error-checked delivery with retransmission of lost packets (used for HTTP/HTTPS, SSH, databases). **UDP** is connectionless and sends datagrams without guaranteeing delivery, ordering, or retransmission, trading reliability for lower overhead and lower latency (common for DNS queries and real-time streaming/voice).

7. **What service commonly uses port 22?**
   **SSH (Secure Shell)** commonly uses TCP port `22` for encrypted remote command-line access and file transfers (such as `scp`/`sftp`).

8. **What does `sudo ss -tulnp` display?**
   It displays **TCP** (`-t`) and **UDP** (`-u`) sockets currently in a **listening** (`-l`) state, showing **numeric** addresses and ports instead of resolving hostnames/service names (`-n`), along with the **process** name and PID (`-p`) owning each socket.