# Practical Lab

### Task 1 — Install SSH Server
```bash
sudo apt install openssh-server
```
* **Answer / Expected output:** Installs the OpenSSH server package (`sshd`) and its dependencies so the machine can accept incoming SSH connections.

### Task 2 — Check the Service
```bash
sudo systemctl status ssh
```
* **Answer / Expected output:** Displays the systemd service status for `ssh`; look for `active (running)` indicating the SSH daemon is running.

### Task 3 — Check the Listening Port
```bash
sudo ss -tulnp | grep :22
```
* **Answer / Expected output:** Shows a listening socket on port `22` (such as `0.0.0.0:22` or `[::]:22`) associated with `sshd`.

### Task 4 — Generate an SSH Key Pair
```bash
ssh-keygen -t ed25519
```
* **Answer / Expected output:** Prompts for a save location (default `~/.ssh/id_ed25519`) and optional passphrase, then creates both the private key (`~/.ssh/id_ed25519`) and public key (`~/.ssh/id_ed25519.pub`).

### Task 5 — Connect to Your Own Machine
```bash
ssh $(whoami)@localhost
```
* **Answer / Expected output:** Substitutes your current username (`$(whoami)`), prompts to trust the host key (`yes`) on first connection, authenticates, and opens a shell session on `localhost`.

### Task 6 — Exit and Inspect Your SSH Directory
```bash
exit
ls -la ~/.ssh
```
* **Answer / Expected output:** `exit` closes the remote/localhost SSH session, and `ls -la ~/.ssh` lists files inside `~/.ssh` (such as `id_ed25519`, `id_ed25519.pub`, and `known_hosts`) along with their permissions.

### Task 7 — Practice File Transfer Locally

Create a test file and copy it through SCP:
```bash
echo "SSH file transfer test" > test.txt

scp test.txt $(whoami)@localhost:/tmp/
```
* **Answer / Expected output:** Creates `test.txt` locally and securely copies it over SSH to `/tmp/test.txt` on `localhost`.

Verify the copied file:
```bash
cat /tmp/test.txt
```
* **Answer / Expected output:** Outputs `SSH file transfer test`, confirming the transfer succeeded.