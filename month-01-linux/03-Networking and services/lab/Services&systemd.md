## 7. Practical Lab

Run these commands in your Linux VM (perform locally in your VM; do not stop SSH if connected remotely through it).

### Task 1 — Inspect the Service
```bash
systemctl status ssh
```
* **Answer / Expected output:** Displays service details including **Loaded** (`loaded`), **Active** state (such as `active (running)` or `inactive (dead)`), and **Main PID** (the PID number of `sshd` when running).

### Task 2 — Check Its Startup Configuration
```bash
systemctl is-enabled ssh
```
* **Answer / Expected output:** Prints whether the service is set to start automatically at boot, such as `enabled` or `disabled`.

### Task 3 — Restart the Service
```bash
sudo systemctl restart ssh
systemctl is-active ssh
```
* **Answer / Expected output:** Restarts the `ssh` daemon (assigning a new PID if it was running), and `systemctl is-active ssh` outputs `active` when running properly.

### Task 4 — Inspect Other Services
```bash
systemctl list-units --type=service
```
* **Answer / Expected output:** Lists loaded systemd service units (such as `cron.service`, `systemd-journald.service`, `NetworkManager.service`, or `docker.service`). Pick any three and note their role (e.g., `cron` runs scheduled tasks, `NetworkManager` manages network connections, `docker` manages containers).

### Task 5 — Inspect the SSH Unit File
```bash
systemctl cat ssh
```
* **Answer / Expected output:** Prints the systemd unit file configuration for `ssh.service`, showing sections such as `[Unit]` (metadata/ordering/dependencies), `[Service]` (how the daemon is started/managed), and `[Install]` (boot target/enablement settings).

---

## 8. Knowledge Check

### Part A — Multiple Choice

1. **What is the main responsibility of `systemd`?**
   * **Answer:** **B. Managing services and system startup**

2. **What does `sudo systemctl enable nginx` do?**
   * **Answer:** **C. Configures nginx to start automatically at boot**

3. **Which command checks whether a service is currently running?**
   * **Answer:** **B. systemctl is-active nginx**

### Part B — Explain in Your Own Words

1. **What's the difference between `start` and `enable`?**
   `start` starts a service right away in the current session without necessarily configuring it to launch at boot, whereas `enable` configures the service to start automatically on boot without necessarily starting it immediately.

2. **What is a daemon?**
   A **daemon** is a background process that runs continuously to provide a service or system functionality (often waiting for requests or events, such as `sshd` listening for incoming SSH connections).

3. **Why might one service depend on another?**
   A service may require another component or service to be available first—for example, a network-facing or database-backed service needing the network stack or database service to be ready before or when it starts.

