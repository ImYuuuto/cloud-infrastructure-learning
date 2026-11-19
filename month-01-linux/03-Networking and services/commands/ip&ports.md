# IP Addresses & Ports — Commands Cheat Sheet

## IP & Interface Inspection
```bash
ip addr
ip -4 addr show scope global
ip -6 addr show scope global
ip -6 addr show
hostname -I
ip -o link show up | awk -F': ' '{print $2}'
```

## Gateway & Routing
```bash
ip route
ip route | grep default
ip route show default | awk '{print $5}'
```

## Listening Ports & Processes
```bash
sudo ss -tulnp
sudo ss -tulnp | grep LISTEN
sudo ss -tulnp | grep :3000
sudo ss -tulnp | grep ':22\b'
```

## Connection Testing
```bash
nc -zv 127.0.0.1 22
```