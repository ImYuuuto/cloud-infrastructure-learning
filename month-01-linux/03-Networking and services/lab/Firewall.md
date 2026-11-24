# Practical Exercises: Firewall Administration (UFW)

---

## Exercise 1 — Installation and Inspection

### 1. Check whether UFW is installed

```bash
ufw version
```

*(If not installed: `sudo apt update && sudo apt install ufw`)*

### 2. Display its current status

```bash
sudo ufw status
```

### 3. Display its verbose status

```bash
sudo ufw status verbose
```

---

## Exercise 2 — Firewall Configuration

### 1. Deny incoming traffic by default

```bash
sudo ufw default deny incoming
```

### 2. Allow outgoing traffic by default

```bash
sudo ufw default allow outgoing
```

### 3. Allow SSH

```bash
sudo ufw allow 22/tcp
```

*(Or `sudo ufw allow ssh`)*

### 4. Allow HTTP

```bash
sudo ufw allow 80/tcp
```

### 5. Allow HTTPS

```bash
sudo ufw allow 443/tcp
```

### 6. Enable the firewall

```bash
sudo ufw enable
```

### Verify your configuration

```bash
sudo ufw status verbose
```

---

## Exercise 3 — Managing Rules

### 1. Allow port 3000

```bash
sudo ufw allow 3000
```

### 2. Display numbered firewall rules

```bash
sudo ufw status numbered
```

### 3. Remove the rule allowing port 3000

By matching rule specification:

```bash
sudo ufw delete allow 3000
```

*(Or by rule number from `sudo ufw status numbered`, e.g., `sudo ufw delete <number>`.)*

### 4. Verify that the rule was removed

```bash
sudo ufw status numbered
```

---

## Exercise 4 — IP Restrictions

### 1. Allow SSH only from `192.168.1.50`

```bash
sudo ufw allow from 192.168.1.50 to any port 22
```

### 2. Deny all traffic from `192.168.1.100`

```bash
sudo ufw deny from 192.168.1.100
```

### 3. Explain the difference between allowing a port and allowing a specific IP to access that port

Allowing a port alone (such as `sudo ufw allow 22`) opens that port to **any** source IP address on the network/internet. Allowing a specific IP to access that port (such as `sudo ufw allow from 192.168.1.50 to any port 22`) restricts access to **only** packets originating from `192.168.1.50`, reducing the exposed attack surface.

---

## Knowledge Check

### 1. What is the primary purpose of a firewall?

To control incoming and outgoing network traffic based on a set of security rules, protecting the system by permitting only authorized traffic and blocking unwanted connections.

### 2. What's the difference between `DROP` and `REJECT`?

`DROP` silently discards incoming/matching packets without sending any response back to the sender, whereas `REJECT` blocks the traffic and sends a rejection reply (such as an ICMP unreachable or TCP reset) back to the sender.

### 3. What is Netfilter?

Netfilter is the packet-filtering and network-address-translation framework built directly into the Linux kernel that inspects and filters network packets.

### 4. What's the difference between UFW and `iptables`?

`iptables` is a lower-level, traditional CLI tool for configuring Netfilter tables, chains, and rules directly with more granular/complex syntax. **UFW (Uncomplicated Firewall)** is a higher-level, beginner-friendly front end that simplifies common firewall tasks and generates the underlying firewall rules behind the scenes.

### 5. What happens when you execute `sudo ufw default deny incoming`?

Any incoming network traffic that does not match an explicit `ALLOW` rule is blocked/denied by default.

### 6. Why should you allow SSH before enabling UFW on a remote server?

If incoming traffic is denied by default and SSH (port `22`) is not explicitly allowed first, enabling the firewall will block your active or future SSH connections and lock you out of remote access to the server.

### 7. What's the difference between `sudo ufw allow 3306` and `sudo ufw allow from 192.168.1.50 to any port 3306`?

`sudo ufw allow 3306` opens port `3306` (MySQL) to **all** source IP addresses, whereas `sudo ufw allow from 192.168.1.50 to any port 3306` restricts access on port `3306` to **only** traffic coming from `192.168.1.50`.

### 8. Why is leaving MySQL publicly accessible a security risk?

Exposing a database port (`3306`) directly to the public internet increases exposure to brute-force login attempts, credential stuffing, vulnerability exploits, and unauthorized access or data exfiltration from external attackers.