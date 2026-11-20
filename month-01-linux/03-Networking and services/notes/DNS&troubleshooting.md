# DNS and Network Troubleshooting

---

## 1. What is DNS?

**DNS (Domain Name System)** translates human-readable domain names into IP addresses (like the internet's phonebook).

* **Domain:** `google.com`
* **IPv4 address:** `142.250.185.14` *(example; addresses can change)*

```text
Without DNS:
Browser ---> IP address

With DNS:
User enters google.com ---> DNS resolver finds the IP address ---> Browser connects to the IP address
```

---

## 2. How DNS Resolution Works

Simplified lookup flow when you enter `example.com` in a browser:

1. **Browser cache:** Checks whether it already knows the IP.
2. **Operating system cache:** Checks locally cached DNS results.
3. **DNS resolver:** Your computer asks a DNS resolver (usually from your ISP or a public DNS provider).
4. **Root DNS server:** Directs the resolver toward the appropriate top-level domain (TLD) servers.
5. **TLD DNS server:** For `.com`, directs it toward the domain's authoritative DNS server.
6. **Authoritative DNS server:** Provides the requested DNS record.
7. **Response:** The resolver returns the IP address to your computer, and the result can be cached for faster future lookups.

> **Important Distinction:**
> * **DNS resolver:** Finds the answer on behalf of your computer.
> * **Authoritative DNS server:** Stores the official DNS records for a domain.

---

## 3. DNS Records

DNS supports multiple record types beyond IPv4 lookups:

| Record | Purpose | Example |
| :--- | :--- | :--- |
| **A** | Domain → IPv4 | `example.com` → `93.184.216.34` |
| **AAAA** | Domain → IPv6 | IPv6 address |
| **CNAME** | Alias to another domain | `www` → `example.com` |
| **MX** | Mail servers | Email delivery |
| **NS** | Authoritative name servers | DNS management |
| **TXT** | Text information | Domain verification, SPF |

> **Note:** You don't need to memorize every record immediately—focus on understanding what each one does.

---

## 4. DNS Servers

Your computer needs to know which DNS resolver to ask.

### Common Public DNS Providers

| Provider | IPv4 Address |
| :--- | :--- |
| **Google DNS** | `8.8.8.8` |
| **Google DNS** | `8.8.4.4` |
| **Cloudflare DNS** | `1.1.1.1` |
| **Cloudflare DNS** | `1.0.0.1` |

These are public DNS resolver addresses.

### Inspecting Linux DNS Configuration
```bash
cat /etc/resolv.conf
```
*(Depending on your Linux configuration, this file may point to a local resolver such as `127.0.0.53`.)*

You can also inspect:
```bash
resolvectl status
```

---

## 5. DNS Commands on Linux

Important tools for infrastructure troubleshooting:

### A. `nslookup`
Performs DNS queries.
```bash
nslookup google.com
```
*(Displays information such as the DNS server used, domain queried, and returned IP address.)*

Query a specific DNS server:
```bash
nslookup google.com 1.1.1.1
```

### B. `dig`
A more detailed DNS diagnostic tool.
```bash
dig google.com
```

Useful variations:
```bash
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com NS
```

Get only the answer:
```bash
dig +short google.com
```

Query Cloudflare directly:
```bash
dig @1.1.1.1 google.com
```

### C. `host`
A simpler DNS lookup tool.
```bash
host google.com
```

For a mail record:
```bash
host -t MX google.com
```

### Installing on Ubuntu (if missing)
```bash
sudo apt update
sudo apt install dnsutils
```

## 6. Network Troubleshooting

When you cannot access a website, the issue could be:
* Your network interface is disconnected.
* Your computer has no IP address.
* Your default gateway is unreachable.
* DNS isn't resolving domain names.
* The destination server is unreachable.
* The server's port is closed.

A systematic approach is required.

### Useful Troubleshooting Commands

| Command | What It Checks |
| :--- | :--- |
| `ip addr` | Network interfaces and IP addresses |
| `ip route` | Routing table and default gateway |
| `ping` | Basic reachability |
| `dig` | DNS resolution |
| `traceroute` | Network path |
| `ss -tuln` | Listening ports |
| `curl` | HTTP/HTTPS connectivity |

### A Practical Troubleshooting Sequence

Suppose `google.com` isn't loading:

**Step 1 — Check your IP address**
```bash
ip addr
```

**Step 2 — Check your default gateway**
```bash
ip route
```

**Step 3 — Test internet connectivity without DNS**
```bash
ping -c 4 1.1.1.1
```

**Step 4 — Test DNS resolution**
```bash
dig google.com
```

**Step 5 — Test the website**
```bash
curl -I [https://google.com](https://google.com)
```

**Step 6 — Inspect the network path**
```bash
traceroute google.com
```

If `traceroute` isn't installed:
```bash
sudo apt install traceroute
```

> **Note:** A failed `ping` doesn't always mean a host is down—some networks block ICMP traffic.