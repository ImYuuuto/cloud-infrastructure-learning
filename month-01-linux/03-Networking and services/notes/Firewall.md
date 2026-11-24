# Firewall Basics

**Month 1 · Week 3 · Networking and Services**

---

## 1. What Is a Firewall?

A **firewall** is a security mechanism that controls incoming and outgoing network traffic based on predefined rules. Think of it as a security guard protecting your Linux server.

For example, imagine you have a Linux server running:

- **SSH** on port `22`
- **HTTP** on port `80`
- **HTTPS** on port `443`
- **MySQL** on port `3306`

You might want users to access your website over HTTP/HTTPS while preventing external users from connecting directly to your MySQL database. A firewall allows you to enforce that boundary.

---

## 2. How Does a Firewall Work?

A firewall evaluates network packets against ordered rules and applies an action.

### Fundamental Actions

| Action | Meaning |
| :--- | :--- |
| **ACCEPT / ALLOW** | Allow the traffic through |
| **DROP** | Silently discard the traffic without replying |
| **REJECT** | Block the traffic and send a rejection response back |

### Example Rule Set

| Port | Service | Rule |
| :--- | :--- | :--- |
| `22` | SSH | `ALLOW` |
| `80` | HTTP | `ALLOW` |
| `443` | HTTPS | `ALLOW` |
| `3306` | MySQL | `DENY` |

With this setup:

- Administrators can connect to the server via SSH.
- Visitors can reach the website over HTTP and HTTPS.
- External hosts cannot connect directly to MySQL on port `3306`.

> **Key Concept:** Firewalls can match traffic based on **source IP**, **destination IP**, **protocol** (such as TCP or UDP), **port**, and **connection state**.

---

## 3. Linux Firewall Technologies

Linux includes several layers and tools involved in packet filtering and firewall management:

- **Netfilter:** The Linux kernel framework that performs packet filtering, NAT, and connection tracking.
- **`iptables`:** The traditional command-line utility for configuring Netfilter rule tables.
- **`nftables`:** The modern kernel packet-classification framework replacing much of legacy `iptables` functionality.
- **UFW (Uncomplicated Firewall):** A user-friendly CLI front end designed to simplify firewall configuration—widely used on Ubuntu servers.

---

## 4. UFW — Uncomplicated Firewall

### A. Check or Install UFW

Check if UFW is installed:

```bash
ufw version
```

If it is not installed on a Debian/Ubuntu system:

```bash
sudo apt update
sudo apt install ufw
```

### B. Check Firewall Status

```bash
sudo ufw status
```

For more detailed output (including default policies and logging level):

```bash
sudo ufw status verbose
```

If UFW is not active yet, you will see:

```text
Status: inactive
```

### C. Enable and Disable UFW

Enable UFW:

```bash
sudo ufw enable
```

Disable UFW:

```bash
sudo ufw disable
```

> **Important Security Warning:** If you are connected to a remote server over SSH, **allow SSH before enabling UFW**. Otherwise, enabling a default-deny incoming policy can lock you out of your session.

### D. Allow Traffic

Allow SSH (port `22`):

```bash
sudo ufw allow 22
```

Allow HTTP (port `80`):

```bash
sudo ufw allow 80
```

Allow HTTPS (port `443`):

```bash
sudo ufw allow 443
```

You can also allow by service name or specify a protocol (`tcp` / `udp`):

```bash
sudo ufw allow ssh
sudo ufw allow 80/tcp
```

### E. Deny Traffic

Deny incoming traffic on MySQL (port `3306`):

```bash
sudo ufw deny 3306
```

Deny a specific port and protocol:

```bash
sudo ufw deny 8080/tcp
```

### F. Delete Firewall Rules

Suppose you allowed port `8080`:

```bash
sudo ufw allow 8080
```

Delete the rule by matching it:

```bash
sudo ufw delete allow 8080
```

Alternatively, list rules with numbers and delete by rule number:

```bash
sudo ufw status numbered
sudo ufw delete 2
```

*(Always check `sudo ufw status numbered` first so you delete the exact rule number you intend.)*

---

## 5. Default Firewall Policies

Default policies define what happens to traffic that does not match an explicit allow or deny rule.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

- **`deny incoming`:** Blocks incoming connections unless explicitly permitted.
- **`allow outgoing`:** Allows the server to initiate outbound connections (for example, to fetch updates or reach external APIs) by default.

---

## 6. Restrict Access by IP Address

You can restrict sensitive services—such as SSH—to specific trusted IP addresses.

Allow SSH only from `192.168.1.50`:

```bash
sudo ufw allow from 192.168.1.50 to any port 22
```

Block all traffic from a specific IP address (`192.168.1.100`):

```bash
sudo ufw deny from 192.168.1.100
```

---

## 7. Real-World Server Configuration Example

Suppose you are deploying a web application on an Ubuntu server with these requirements:

- **SSH** allowed for server administration.
- **HTTP** and **HTTPS** allowed for web visitors.
- **MySQL** kept internal (not exposed externally).
- **All other** incoming traffic blocked.

A clean UFW setup for this scenario:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

Because port `3306` is not explicitly allowed, incoming external traffic to MySQL is blocked by `default deny incoming`.

> **Defense in Depth:** A firewall is one security layer, not the entire solution. Also bind MySQL to `127.0.0.1` (or an internal private interface), enforce strong authentication, keep packages updated, and harden SSH access.