## Practical Lab — DNS and Network Troubleshooting

### 1. Inspect Your Network Interfaces
```bash
ip addr
```
* **Expected / Answer:** Shows network interfaces (such as `lo`, `eth0`/`enp...`, `wlan0`/`wlp...`), their operational state (`UP`/`DOWN`), and assigned IPv4 (`inet`) / IPv6 (`inet6`) addresses.

---

### 2. Inspect Your Routing Table
```bash
ip route
```
* **Expected / Answer:** Displays kernel routing entries, including the `default via <gateway_ip>` route used to send traffic outside the local subnet.

---

### 3. Query a Domain
```bash
nslookup google.com
```
* **Expected / Answer:** Queries the configured DNS resolver and returns the server answering, the queried domain (`google.com`), and the resolved IP address(es).

---

### 4. Query Using Cloudflare DNS
```bash
dig @1.1.1.1 google.com
```
* **Expected / Answer:** Sends the DNS query directly to Cloudflare's public resolver (`1.1.1.1`) instead of the system default resolver, returning full query details, flags, answer records, and query time.

---

### 5. Display Only the Returned IP
```bash
dig +short google.com
```
* **Expected / Answer:** Prints only the resolved record value(s) (such as the IPv4 address(es) for `google.com`) without header, authority, or statistics sections.

---

### 6. Test Internet Connectivity
```bash
ping -c 4 8.8.8.8
```
* **Expected / Answer:** Sends 4 ICMP echo requests to Google DNS (`8.8.8.8`) to verify basic IP-level reachability without relying on DNS resolution; shows round-trip times (RTT) and packet loss percentage.

---

### 7. Test HTTP Connectivity
```bash
curl -I [https://google.com](https://google.com)
```
* **Expected / Answer:** Fetches only the HTTP response headers (`-I` / `HEAD` request) from `https://google.com`, confirming TCP/TLS connection and HTTP status (e.g., `HTTP/2 200` or redirect status).

---

### 8. Inspect the Network Path
```bash
traceroute google.com
```
* **Expected / Answer:** Traces each hop (router/gateway) along the route from your machine to `google.com`, displaying hop numbers, hostnames/IPs, and latency per hop.