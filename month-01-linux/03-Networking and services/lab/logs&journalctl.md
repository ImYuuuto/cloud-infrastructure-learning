# Practical Exercises: Logs and `journalctl` (With Answers)

---

## Exercise 1 — Exploring Logs

### 1. Navigate to `/var/log`

```bash
cd /var/log
```

### 2. Display its contents

```bash
ls -la
```

### 3. Identify three log files and explain their purposes

- **`/var/log/syslog`** — Stores general system and service messages (common on Debian/Ubuntu systems).
- **`/var/log/auth.log`** — Records authentication, login attempts, `sudo` usage, and SSH/PAM security events (common on Debian/Ubuntu systems).
- **`/var/log/kern.log`** — Captures kernel-level messages, hardware events, and driver notices.

### 4. Display the last 15 lines of one log file

```bash
tail -n 15 /var/log/syslog
```

---

## Exercise 2 — `journalctl`

### 1. Display the system journal

```bash
journalctl
```

### 2. Display the last 30 entries

```bash
journalctl -n 30
```

### 3. Display logs from the current boot

```bash
journalctl -b
```

### 4. Display logs from the previous boot

```bash
journalctl -b -1
```

*(Note: Persistent logs across reboots may require `/var/log/journal` to exist or `Storage=persistent` in `/etc/systemd/journald.conf`.)*

### 5. Display only error messages

```bash
journalctl -p err
```

### 6. Display logs generated during the last hour

```bash
journalctl --since "1 hour ago"
```

---

## Exercise 3 — Service Troubleshooting

Use SSH as your example service (`ssh` on Debian/Ubuntu, or `sshd` on RHEL/Fedora-based distros).

### 1. Check the status of SSH

```bash
systemctl status ssh
```

### 2. Display its last 20 log entries

```bash
journalctl -u ssh -n 20
```

### 3. Display only SSH errors

```bash
journalctl -u ssh -p err
```

### 4. Follow SSH logs in real time

```bash
journalctl -u ssh -f
```

### 5. In another terminal, execute a command that checks SSH's status

Open a second terminal/session and run:

```bash
systemctl status ssh
```

Then switch back to the terminal running `journalctl -u ssh -f` to observe any newly logged events, and press `Ctrl + C` when finished.

---

## Exercise 4 — Understanding the Commands

### 1. What's the difference between `/var/log` and `journalctl`?

`/var/log` is a directory containing traditional plain-text (or rotated/compressed) log files written by the OS, daemons, or `rsyslog` (such as `/var/log/syslog` and `/var/log/auth.log`). `journalctl` is the command-line tool used to query the binary, structured log database maintained by **`systemd-journald`**, making it easy to filter by unit, boot ID, priority level, or time range.

### 2. What's the difference between `tail -f` and `journalctl -f`?

`tail -f` streams newly appended lines from a **single specific file** (for example, `/var/log/syslog`). `journalctl -f` streams live entries from **`systemd-journald`**, and can be combined with filters like `-u <service>`, `-p <priority>`, or `--since` to watch only relevant events across the journal.

### 3. Why would you use `journalctl -u ssh` instead of displaying the entire system journal?

A busy Linux system generates logs from dozens of services, kernel subsystems, and background tasks. Using `-u ssh` filters out unrelated noise and isolates only events belonging to the SSH service unit, making diagnosis much faster.

### 4. What does `journalctl -b -1` do?

It displays journal logs from the **previous system boot** (`-1` steps back one boot from the current boot, which is `0` or omitted with `-b`). This is especially useful when investigating a crash or failed service that happened before a reboot.

### 5. You're managing a Linux server and a service suddenly crashes. Explain how you would investigate the problem using `systemctl` and `journalctl`.

1. **Check the service status** with `systemctl status <service>` to see if it shows `failed`, what its exit code/signal was, and grab recent lines right around the crash.
2. **Inspect recent logs for that unit** using `journalctl -u <service> -n 50` (or `--since "30 minutes ago"`) to see what happened immediately before the crash.
3. **Narrow down to errors** with `journalctl -u <service> -p err` to spot configuration errors, permission issues, port conflicts, or dependency failures.
4. **Test recovery and watch live logs** by running `journalctl -u <service> -f` in one terminal while restarting (`sudo systemctl restart <service>`) or checking status in another.