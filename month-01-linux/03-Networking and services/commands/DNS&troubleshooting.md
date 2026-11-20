# DNS & Network Troubleshooting — Commands Cheat Sheet

## DNS Configuration
```bash
cat /etc/resolv.conf
resolvectl status
```

## DNS Lookups (`nslookup`, `dig`, `host`)
```bash
nslookup google.com
nslookup google.com 1.1.1.1

dig google.com
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com NS
dig +short google.com
dig @1.1.1.1 google.com

host google.com
host -t MX google.com
```

## Network & Path Diagnostics
```bash
ip addr
ip route
ping -c 4 1.1.1.1
ping -c 4 8.8.8.8
curl -I [https://google.com](https://google.com)
traceroute google.com
ss -tuln
```

## Package Installation (Ubuntu)
```bash
sudo apt update
sudo apt install dnsutils
sudo apt install traceroute
```