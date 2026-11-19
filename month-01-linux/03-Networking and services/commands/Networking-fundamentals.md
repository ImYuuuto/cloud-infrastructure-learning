## Networking Fundamentals

### Inspecting Network & Testing Connectivity
- `ip link` — List network interfaces (`lo`, `eth*`, `enp*`, `wlan*`), MAC addresses, and link state (`UP`/`DOWN`)
- `ip addr` — Show IPv4/IPv6 addresses and subnet masks assigned to interfaces
- `ip route` — Display the kernel routing table, including the **default gateway** (`default via <IP>`)
- `ping 8.8.8.8` — Continuously test reachability and latency via ICMP echo requests (`Ctrl + C` to stop)
- `ping -c 4 8.8.8.8` — Send a fixed count (`4`) of ICMP echo requests and print summary stats
- `ss -tuln` — List listening TCP (`-t`) and UDP (`-u`) sockets (`-l`) with numeric (`-n`) addresses and ports

### Network Types & Devices
| Term | Role / Meaning |
| :--- | :--- |
| **LAN / WLAN** | Local Area Network / Wireless LAN (Wi-Fi) within a limited area (home, office, lab) |
| **WAN** | Wide Area Network spanning large distances (cities, countries, the internet) |
| **Router** | Connects **different** networks and forwards packets between them |
| **Switch** | Connects devices **within a LAN** and forwards frames using MAC addresses |
| **Modem** | Connects local network to the ISP over the physical line/access technology |
| **Access Point (AP)** | Lets wireless clients connect to a network via Wi-Fi |

### Core Protocols
| Protocol | Purpose |
| :--- | :--- |
| **HTTP / HTTPS** | Unencrypted / TLS-encrypted web traffic |
| **TCP / UDP** | Reliable, ordered connection-oriented delivery / lightweight datagram transport |
| **DNS** | Resolves domain names to IP addresses |
| **SSH** | Encrypted remote shell and command access |
| **DHCP** | Automatically assigns IP addresses and network configuration |
| **ICMP** | Network diagnostics, reachability checks (`ping`), and error reporting |