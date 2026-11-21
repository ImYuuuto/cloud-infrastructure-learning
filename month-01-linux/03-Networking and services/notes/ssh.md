# SSH and Remote Connections

> **Module:** Linux & Bash — Connecting to another computer remotely using SSH, managing remote sessions, and securely transferring files. Essential for cloud infrastructure because you manage Linux servers without physical access.

---

## 1. What is SSH?

**SSH (Secure Shell)** is a network protocol that allows you to securely access and control another computer remotely.

* **Example:** Connecting from your laptop to a remote Ubuntu server (such as one on Azure) to execute commands without physical access.

```text
Your laptop                      Remote server
[SSH client]  --- SSH / Port 22 --->  [SSH server]
      Encrypted remote connection over a network.
```

**SSH provides:**
* Remote command execution.
* Secure authentication.
* Encrypted communication.
* Secure file transfers.
* Remote server administration.

---

## 2. How SSH Works

SSH uses a **client-server architecture**:
* **SSH client:** The machine initiating the connection.
* **SSH server:** The machine accepting the connection.
* **Default SSH port:** `22`

**Example:**
```bash
ssh username@192.168.1.100
```
* `ssh`: Start an SSH connection.
* `username`: Account on the remote machine.
* `192.168.1.100`: Remote machine's IP address.

You will be asked to authenticate.

---

## 3. Install and Configure SSH on Linux

Ubuntu usually needs the **OpenSSH server** package to accept incoming SSH connections.

**Install it:**
```bash
sudo apt update
sudo apt install openssh-server
```

**Check its status:**
```bash
sudo systemctl status ssh
```

**Start the service:**
```bash
sudo systemctl start ssh
```

**Enable it to start automatically at boot:**
```bash
sudo systemctl enable ssh
```

**Check whether SSH is listening:**
```bash
sudo ss -tulnp | grep :22
```
*(You should see a process listening on port `22`.)*

> **Important:** Installing `openssh-client` lets your machine initiate SSH connections. Installing `openssh-server` lets other machines connect to it.

## 4. Connect to Your Own Machine

Test SSH locally before connecting to another computer:

**Find your username:**
```bash
whoami
```

**Find your IP address:**
```bash
hostname -I
```

**Connect to `localhost`:**
```bash
ssh your_username@localhost
```
*(The first time you connect, SSH may ask whether you trust the server's host key—type `yes`, then authenticate to open a remote shell session on the same machine.)*

**Exit the session:**
```bash
exit
```

---

## 5. SSH Authentication Methods

Two common authentication methods:

### A. Password Authentication
```bash
ssh username@server_ip
```
* Connect using your account password.
* Simple, but passwords can be vulnerable to guessing and brute-force attacks.

### B. SSH Key Authentication
Widely used for server administration—authenticates using a cryptographic key pair instead of sending a password.

| Key Type | Handling | Location |
| :--- | :--- | :--- |
| **Private key** | Keep secret (never share) | Stays on your local machine (`~/.ssh/id_ed25519`) |
| **Public key** | Shareable | Installed on the remote server (`~/.ssh/id_ed25519.pub`) |

**Generate an SSH key pair:**
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
* `ssh-keygen`: Generates SSH keys.
* `-t ed25519`: Selects the Ed25519 algorithm.
* `-C`: Adds a comment to identify the key.

*(The `.pub` file is the public key; the file without `.pub` is the private key.)*

**Copy the public key to a remote server:**
```bash
ssh-copy-id username@server_ip
```

**Connect using your key:**
```bash
ssh username@server_ip
```

---

## 6. SSH Configuration

Create an SSH configuration file to avoid typing full usernames and IPs repeatedly:
```bash
nano ~/.ssh/config
```

**Example:**
```text
Host myserver
    HostName 192.168.1.100
    User ubuntu
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Instead of:
```bash
ssh ubuntu@192.168.1.100
```

Run:
```bash
ssh myserver
```
*(Especially useful when managing multiple cloud servers.)*

---

## 7. Transferring Files with SSH

### SCP — Secure Copy

**Copy a local file to a remote machine:**
```bash
scp file.txt username@server_ip:/home/username/
```

**Copy a remote file to your local machine:**
```bash
scp username@server_ip:/home/username/file.txt .
```

**Copy an entire directory:**
```bash
scp -r myfolder username@server_ip:/home/username/
```

### SFTP — SSH File Transfer Protocol

**Start an interactive file transfer session:**
```bash
sftp username@server_ip
```

**Useful commands inside SFTP:**

| Command | Purpose |
| :--- | :--- |
| `ls` | List remote files |
| `pwd` | Show remote directory |
| `lpwd` | Show local directory |
| `cd` | Change remote directory |
| `lcd` | Change local directory |
| `get file.txt` | Download a file |
| `put file.txt` | Upload a file |
| `exit` | Close session |

---

## 8. SSH Security Fundamentals

Important practices for managing real servers:
* Use SSH keys instead of passwords where possible.
* Protect private keys with appropriate permissions.
* Never share private keys.
* Avoid allowing root login directly.
* Keep OpenSSH updated.
* Restrict access using firewall rules.
* Consider disabling password authentication after verifying key-based access.

SSH daemon configuration is usually located at:
```text
/etc/ssh/sshd_config
```

Example authentication settings:
```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

> **Warning:** Don't change these settings on a remote server until you've verified that key-based access works—otherwise, you could lock yourself out.