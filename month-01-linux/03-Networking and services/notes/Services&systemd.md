# Services and systemd

> **Overview:** How Linux manages background programs (daemons), how services start and stop, and how to control them using `systemctl`.

---

## 1. What is a Service?

A **service** is a program that runs in the background and provides functionality to the system or other programs. A background process that provides a service is often called a **daemon** (for example, an SSH server waiting in the background for incoming connections).

| Service | Purpose |
| :--- | :--- |
| `ssh` | Allows remote connections |
| `cron` | Runs scheduled tasks |
| `mysql` | Manages database operations |
| `docker` | Manages containers |
| `NetworkManager` | Manages network connections |

---

## 2. What is systemd?

**systemd** is the system and service manager used by many modern Linux distributions, including Ubuntu.

It is responsible for:
* Starting services during boot.
* Managing running services.
* Handling dependencies between services.
* Managing system startup.
* Monitoring system state.

On most modern Linux systems, `systemd` runs as **process ID 1**.

Verify with:
```bash
ps -p 1 -o comm=
```
Expected output:
```text
systemd
```

---

## 3. Managing Services with systemctl

`systemctl` is the command-line tool used to communicate with `systemd`.

| Command | Purpose |
| :--- | :--- |
| `sudo systemctl start ssh` | Start a service |
| `sudo systemctl stop ssh` | Stop a service |
| `sudo systemctl restart ssh` | Restart a service |
| `sudo systemctl reload ssh` | Reload configuration, if supported |
| `systemctl status ssh` | Display service status |
| `systemctl is-active ssh` | Check whether it is running |
| `systemctl is-enabled ssh` | Check whether it starts at boot |

### Start vs. Enable

* **`start`**: Starts the service immediately, but doesn't necessarily make it start at boot.
* **`enable`**: Configures the service to start automatically during boot, but doesn't necessarily start it immediately.

Combine them to enable at boot and start immediately:
```bash
sudo systemctl enable --now ssh
```

Disable automatic startup and stop immediately:
```bash
sudo systemctl disable --now ssh
```

## 4. Understanding Service Status

Run:
```bash
systemctl status ssh
```

Example output:
```text
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (...)
     Active: active (running)
   Main PID: 1234 (sshd)
```

**Important fields:**
* **Loaded:** Whether `systemd` found and loaded the service definition.
* **Active:** Whether the service is currently running.
* **Main PID:** The process ID of the main service process.

### Common States

| State | Meaning |
| :--- | :--- |
| `active (running)` | Service is running |
| `inactive (dead)` | Service is stopped |
| `failed` | Service encountered an error |
| `enabled` | Configured to start at boot |
| `disabled` | Not configured to start at boot |

---

## 5. Listing Services

List currently loaded service units:
```bash
systemctl list-units --type=service
```

List installed service unit files, including disabled services:
```bash
systemctl list-unit-files --type=service
```

Show the definition of a specific service (displays the service's unit configuration):
```bash
systemctl cat ssh
```

---

## 6. Service Dependencies

Services sometimes depend on other services or system components (for example, a network-dependent service may need networking to be available before it starts). `systemd` manages these relationships through dependencies.

Two directives you'll encounter in service files:
* **`Requires=`**: Establishes a required dependency.
* **`After=`**: Specifies startup ordering.

`After=` controls order, while `Requires=` establishes a dependency—they are not identical.

> **Note:** You don't need to create custom service files yet—just understand the concept.