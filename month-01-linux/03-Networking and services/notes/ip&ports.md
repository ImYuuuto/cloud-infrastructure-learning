# IP Addresses and Ports

> **Overview:** How devices identify each other on networks and how applications communicate through ports. Essential for cloud infrastructure (server IPs, ports, firewalls, and network configurations).

---

## 1. What is an IP Address?

An **IP address (Internet Protocol address)** is a logical address assigned to a network interface that identifies where network packets should be delivered.

* **IP address** = Address of a device/interface (postal address).
* **Port** = Particular application or service receiving the communication.

**Example:**
```text
192.168.1.10
```
*(IP address of a laptop on a home network)*

---

## 2. IPv4 Addresses

IPv4 is the most commonly encountered IP addressing format in basic networking. An IPv4 address contains **32 bits**, divided into four 8-bit groups called **octets** (each ranging from `0` to `255`).

**Example Breakdown (`192.168.1.10`):**

| Octet | Binary | Decimal |
| :--- | :--- | :--- |
| **First** | `11000000` | `192` |
| **Second** | `10101000` | `168` |
| **Third** | `00000001` | `1` |
| **Fourth** | `00001010` | `10` |

* **Address Range:** `0.0.0.0` to `255.255.255.255`

---

## 3. Public IP vs. Private IP

### Private IP
Used inside private networks (home, school, or company networks). **Not directly routable** over the public internet.

| Private IPv4 Range | Example |
| :--- | :--- |
| `10.0.0.0 – 10.255.255.255` | `10.0.0.5` |
| `172.16.0.0 – 172.31.255.255` | `172.16.2.10` |
| `192.168.0.0 – 192.168.255.255` | `192.168.1.10` |

### Public IP
An address used to identify a network endpoint on the public internet (e.g., a cloud server's public IP so users can access its website).

---

## 4. What is NAT?

**NAT (Network Address Translation)** allows devices with private IP addresses to communicate with external networks through a single public IP address.

```text
Laptop: 192.168.1.10
          |
          |
       Router
        (NAT)
          |
          |
Public IP: 105.x.x.x
          |
       Internet
```

**How it works:**
1. Your laptop sends a request to the internet.
2. The router translates the private source address into its public IP address and tracks the connection so responses return to the correct device.
3. This allows multiple devices to share one public IPv4 address.

## 4. IPv6

IPv4 has a limited address space because it uses **32 bits**. **IPv6** was introduced to provide a much larger address space and uses **128 bits**.

**Example:**
```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### IPv4 vs. IPv6 Comparison

| Feature | IPv4 | IPv6 |
| :--- | :--- | :--- |
| **Size** | 32 bits | 128 bits |
| **Format** | Decimal | Hexadecimal |
| **Separator** | `.` | `:` |
| **Example** | `192.168.1.10` | `2001:db8::1` |

> **Note:** You don't need to master IPv6 subnetting yet—just understand why it exists and how to recognize its format.

---

## 5. Subnet Masks and CIDR

A network contains multiple IP addresses, and a device uses a **subnet mask** to determine which addresses belong to its local network.

**Example:**
```text
IP address:  192.168.1.10
Subnet mask: 255.255.255.0
```

This means the first three octets identify the **network**, while the last octet identifies the **host** within that network.

The same information can be represented using **CIDR notation**:
```text
192.168.1.10/24
```
`/24` means that the first **24 bits** are the network portion.

**Same `/24` Network:**
```text
192.168.1.10/24
192.168.1.20/24
192.168.1.50/24
```

**Different Network:**
```text
192.168.2.10/24
```

> **Important:** With a `/24` IPv4 subnet, there are **256** total addresses, conventionally **254** usable host addresses in an ordinary subnet. Subnet calculations will be practiced more later in your infrastructure journey.

---

## 6. What Are Ports?

An **IP address** identifies the network destination, while a **port** identifies the particular application or service receiving traffic on that destination.

**Example:**
```text
192.168.1.10:3000
```
* `192.168.1.10` = IP address
* `3000` = Port

Imagine a server running multiple applications simultaneously, each listening on a different port:
```text
Server: 192.168.1.10
Port 80    -> HTTP website
Port 443   -> HTTPS website
Port 22    -> SSH
Port 3306  -> MySQL
```

### Important Ports to Know

| Port | Protocol / Service |
| :--- | :--- |
| `20/21` | FTP |
| `22` | SSH |
| `25` | SMTP |
| `53` | DNS |
| `80` | HTTP |
| `110` | POP3 |
| `143` | IMAP |
| `443` | HTTPS |
| `3306` | MySQL |
| `5432` | PostgreSQL |
| `27017` | MongoDB |
| `3000` | Common development port |
| `4000` | Common development port |
| `8080` | Common alternative HTTP port |

* **Port Range:** `0` to `65535`
* **Note:** Ports `3000` and `4000` are common conventions, not reserved exclusively for those applications.

## 7. TCP vs. UDP Ports

Connecting **TCP** and **UDP** to ports:

### TCP (Transmission Control Protocol)
* Establishes a connection.
* Provides reliable, ordered delivery.
* Retransmits lost data when needed.
* **Examples:** HTTP/HTTPS over TCP, SSH, MySQL.

### UDP (User Datagram Protocol)
* Sends datagrams without establishing a connection.
* Does not guarantee delivery or ordering.
* Has less protocol overhead.
* **Examples:** DNS queries commonly use UDP, and many real-time communication applications use UDP.

> **Important Distinction:** A port number can be used by both TCP and UDP independently (e.g., `TCP port 53` and `UDP port 53` are separate endpoints).

---

## 8. Practical Linux Commands

### A. Find Your IP Address
```bash
ip addr
```
Or:
```bash
hostname -I
```

### B. Find Your Default Gateway
```bash
ip route
```
Look for:
```text
default via 192.168.1.1
```
*(Here, `192.168.1.1` is your default gateway.)*

### C. Inspect Listening Ports
```bash
sudo ss -tulnp
```
* `-t`: TCP
* `-u`: UDP
* `-l`: Listening sockets
* `-n`: Numeric addresses and ports
* `-p`: Associated processes

### D. Check a Particular Port
```bash
sudo ss -tulnp | grep :3000
```
*(Checks whether something is listening on port `3000`.)*

### E. Test a Connection
```bash
nc -zv 127.0.0.1 22
```
*(Attempts to connect to TCP port `22` on your local machine. If SSH isn't installed or running, the connection may fail—which is normal.)*