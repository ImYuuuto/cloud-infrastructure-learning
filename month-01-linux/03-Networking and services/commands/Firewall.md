
## Cheat Sheet 2: Firewall Basics (`ufw`)

### Installation & Status

| Command | Purpose |
| :--- | :--- |
| `ufw version` | Check if UFW is installed and display its version |
| `sudo apt update && sudo apt install ufw` | Install UFW on Debian/Ubuntu systems |
| `sudo ufw status` | Show current firewall status (`active` or `inactive`) and basic rules |
| `sudo ufw status verbose` | Show status, default policies, logging level, and detailed rules |
| `sudo ufw status numbered` | List active rules with rule numbers for targeted deletion |

### Enable, Disable & Default Policies

| Command | Purpose |
| :--- | :--- |
| `sudo ufw default deny incoming` | Block incoming traffic by default unless explicitly allowed |
| `sudo ufw default allow outgoing` | Allow outgoing traffic by default |
| `sudo ufw enable` | Activate UFW *(allow SSH first on remote servers)* |
| `sudo ufw disable` | Deactivate UFW |

### Allowing, Denying & Deleting Rules

| Command | Purpose |
| :--- | :--- |
| `sudo ufw allow 22` | Allow traffic on port `22` (or by name: `sudo ufw allow ssh`) |
| `sudo ufw allow 80/tcp` | Allow TCP traffic on port `80` (HTTP); also `443/tcp` for HTTPS |
| `sudo ufw deny 3306` | Deny traffic on port `3306` (e.g., MySQL) |
| `sudo ufw deny 8080/tcp` | Deny TCP traffic on port `8080` |
| `sudo ufw delete allow 3000` | Remove a matching rule (here, allowing port `3000`) |
| `sudo ufw delete 2` | Delete rule number `2` (check `sudo ufw status numbered` first) |

### IP-Based Rules

| Command | Purpose |
| :--- | :--- |
| `sudo ufw allow from 192.168.1.50 to any port 22` | Allow SSH (port `22`) only from IP `192.168.1.50` |
| `sudo ufw deny from 192.168.1.100` | Block all traffic from IP `192.168.1.100` |