# Lesson 1 — Networking Fundamentals

### 1. What is a Computer Network?
A **computer network** is a group of interconnected devices that can communicate and exchange data.

Devices on a network commonly include:
* Computers and laptops
* Servers
* Smartphones and tablets
* Network printers
* Routers, switches, and access points

For example, when your laptop and smartphone are on the same home Wi-Fi network, they can discover and communicate with each other over that network.

In infrastructure and cloud engineering, networking is foundational because servers constantly need to talk to:
* Other application or microservice servers
* Databases and caches
* Client apps and browsers
* External APIs and third-party services
* The public internet

---

### 2. Types of Networks

#### LAN — Local Area Network
A **LAN** connects devices within a limited physical or geographical area—such as a single room, home, office, or campus building.

* **Examples:** Home network, university computer lab, office floor network.
* **Characteristics:** Typically privately owned or administered, offers relatively high bandwidth/speed, and covers a small area.

#### WAN — Wide Area Network
A **WAN** connects multiple networks across wide geographical areas—such as cities, regions, or countries.

* **Examples:** Interconnecting branch offices across cities, cross-country enterprise links, and the internet itself.
* **Characteristics:** Spans large distances, links smaller networks together, and frequently relies on telecom/carrier infrastructure.

#### WLAN — Wireless Local Area Network
A **WLAN** is a LAN that connects devices wirelessly rather than over Ethernet cables—most commonly using **Wi-Fi**.

* **Example:** Your home Wi-Fi network or an office wireless SSID.

#### Quick Comparison

| Type | Full Name | Scope | Typical Example |
| :--- | :--- | :--- | :--- |
| **LAN** | Local Area Network | Small / local area (home, office, lab) | Office wired/local network |
| **WAN** | Wide Area Network | Large / wide area (cities, countries, global) | The internet |
| **WLAN** | Wireless LAN | Local area using wireless signals (Wi-Fi) | Home Wi-Fi |

---

### 3. Core Network Devices

#### A. Router
A **router** connects **different networks** together and forwards data packets between them based on network routing decisions (such as IP addresses).

* **Example:** Your home or edge router connects your local network to your Internet Service Provider (ISP) network.

#### B. Switch
A **switch** connects multiple devices **within the same LAN** and forwards Ethernet frames to the specific destination port using **MAC addresses**.

#### C. Modem
A **modem** (modulator–demodulator) bridges your local network to your **ISP** using the physical line/signal technology of your connection (such as cable, DSL, or fiber termination).

#### D. Access Point (AP)
An **Access Point** allows wireless devices (like phones and laptops) to join a wired network over **Wi-Fi**.

#### Key Distinctions
* **Switch:** Connects devices **inside** a local network (MAC-based forwarding).
* **Router:** Connects **different** networks together (routing between networks).
* **Access Point:** Bridges **wireless (Wi-Fi)** clients into a network.
* **Modem:** Connects to the **ISP** using the line/access technology.

> **Note:** Consumer "home Wi-Fi routers" often bundle a **router**, **switch** (LAN ports), **access point** (Wi-Fi radios), and sometimes a **modem** into a single box—even though each function is distinct in enterprise and cloud networking.

### 4. Client–Server Architecture
This is one of the most foundational concepts in infrastructure and backend engineering. Many network applications follow a **client–server model**:

* **Client:** Initiates communication by requesting a service, data, or resource.
* **Server:** Listens for requests, processes them, and returns a response or resource.

#### Web Browser Example (e.g., Opening YouTube)
1. Your web browser acts as the **client**.
2. It sends a request to a remote **server**.
3. The server processes the request (fetching data, video metadata, or pages).
4. The server sends a response back over the network.
5. Your browser renders and displays the result.

#### Full-Stack App Example (React, Express, and MySQL)
The exact same pattern applies to modern web applications and cloud systems—where a service can act as a client to another downstream service:

```text
React Frontend (Client)
       │
       │ HTTP Request
       ▼
Express Backend (Server / API Client to DB)
       │
       │ Database Query
       ▼
     MySQL (Database Server)
```

---

### 5. How Data Travels Across a Network
When data is sent across a network, it is split into smaller chunks called **packets**. Each packet includes delivery metadata—such as addressing and protocol details—so network devices know where it is going and how to handle it.

#### Request–Response Flow (e.g., Loading a Website)
1. Your device prepares the outgoing data and breaks it into packets.
2. Packets travel across the network link.
3. Routers forward packets hop-by-hop toward the destination.
4. The destination server receives, reassembles, and processes the packets.
5. Response packets travel back to your device.

#### Why Use Packets?
Breaking large transmissions into smaller **packets** lets multiple devices and streams share network links efficiently, handle congestion, and retransmit only small pieces if something is dropped—rather than sending one massive block at a time. *(We will cover IP addresses and ports in the next lesson.)*

---

### 6. Network Protocols
A **protocol** is a standardized set of rules that defines how devices format, transmit, and interpret data over a network.

| Protocol | Purpose |
| :--- | :--- |
| **HTTP** | Unencrypted web communication between clients and servers |
| **HTTPS** | Encrypted web communication (HTTP secured with TLS/SSL) |
| **TCP** | Connection-oriented, reliable, ordered data delivery |
| **UDP** | Lightweight, connectionless datagram-based communication |
| **DNS** | Resolves human-readable domain names into IP addresses |
| **SSH** | Encrypted, secure remote shell and command access |
| **DHCP** | Automatically assigns IP addresses and network settings to devices |
| **ICMP** | Network diagnostics, reachability checks, and error reporting |

---

### 7. Practical Linux Commands
Let's start interacting with your Linux environment to inspect network state and test connectivity.

#### A. Display Network Interfaces
```bash
ip link
```
Lists network interfaces on your system (such as loopback `lo`, Ethernet `enp*`/`eth*`, and Wi-Fi `wlan*`), along with their link state (`UP`/`DOWN`) and MAC addresses.

#### B. Display IP Addresses
```bash
ip addr
```
Shows the IPv4 and IPv6 addresses and subnet masks assigned to each network interface.

#### C. Display Routing Information
```bash
ip route
```
Displays the kernel's routing table—including your **default gateway**—which determines where your machine sends traffic destined for local vs. remote networks.

#### D. Test Network Connectivity
```bash
ping 8.8.8.8
```
Sends ICMP echo requests to `8.8.8.8` (Google's public DNS) and measures round-trip response times to verify basic IP reachability.

Stop continuous output with:
```text
Ctrl + C
```

#### E. Display Network Connections
```bash
ss -tuln
```
Lists listening **TCP** (`-t`) and **UDP** (`-u`) sockets in a listening state (`-l`) using numeric (`-n`) IP addresses and port numbers instead of resolving names. *(We'll look closer at ports in the next lesson.)*