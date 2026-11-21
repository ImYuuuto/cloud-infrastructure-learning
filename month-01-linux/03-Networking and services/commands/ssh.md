# SSH & Remote Connections — Commands Cheat Sheet

## Server Setup & Service Management
```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl enable ssh
sudo ss -tulnp | grep :22
```

## Connecting & Session Control
```bash
whoami
hostname -I
ssh username@192.168.1.100
ssh your_username@localhost
ssh $(whoami)@localhost
exit
```

## SSH Keys & Config
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-keygen -t ed25519
ssh-copy-id username@server_ip
ls -la ~/.ssh
nano ~/.ssh/config
ssh myserver
```

## File Transfers (`scp` & `sftp`)
```bash
scp file.txt username@server_ip:/home/username/
scp username@server_ip:/home/username/file.txt .
scp -r myfolder username@server_ip:/home/username/
scp test.txt $(whoami)@localhost:/tmp/
cat /tmp/test.txt
sftp username@server_ip
```

### Inside `sftp`
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

## Server Config File
```text
/etc/ssh/sshd_config
```