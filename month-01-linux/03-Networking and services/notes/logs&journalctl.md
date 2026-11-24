# Logs and `journalctl`

## 1. What Are System Logs?

Logs are records of events that happen inside an operating system. Linux generates logs to help administrators understand:

- **What** happened on the system.
- **When** something happened.
- **Which service** caused an error.
- **Whether** a service started or stopped successfully.
- **Whether** someone attempted an unauthorized action.

For example, imagine your SSH service suddenly stops working. Instead of guessing what is wrong, you inspect its logs to understand the problem. Logs are one of the most important tools for troubleshooting Linux servers.

---

## 2. Where Are Linux Logs Stored?

Most traditional Linux log files are stored inside `/var/log/`.

Explore the directory:

```bash
ls /var/log/
```

### Common Log Files

| File | Purpose |
| :--- | :--- |
| `/var/log/syslog` | General system messages (Debian/Ubuntu) |
| `/var/log/auth.log` | Authentication and login events (Debian/Ubuntu) |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/dpkg.log` | Package installation and removal |
| `/var/log/boot.log` | Boot-related messages |

> **Note:** Not every Linux distribution uses the same files. Some rely heavily on `systemd`'s journal instead.

---

## 3. Working with Log Files

Before learning `journalctl`, let's look at core commands used to inspect plain text log files.

### A. `cat`

Displays the entire file at once:

```bash
cat /var/log/syslog
```

**Limitation:** Log files can become very large, making `cat` impractical for long logs.

### B. `less`

Allows you to scroll and search through a file interactively:

```bash
less /var/log/syslog
```

Useful keyboard shortcuts inside `less`:

- `Space` — Next page
- `b` — Previous page
- `/error` — Search forward for `"error"`
- `n` — Jump to next search result
- `q` — Quit `less`

### C. `tail`

Displays the end of a file, where the newest entries appear:

```bash
tail /var/log/syslog
```

Display the last 20 lines:

```bash
tail -n 20 /var/log/syslog
```

Follow new log entries in real time (great when monitoring an active service or connection attempt):

```bash
tail -f /var/log/syslog
```

Press `Ctrl + C` to stop following.

---

## 4. `journalctl` — The `systemd` Logging Tool

`systemd` manages Linux services and provides a centralized, structured logging system called **`systemd-journald`**. We use `journalctl` to query and read those logs.

Unlike manually inspecting individual log files, `journalctl` lets you filter entries by service unit, boot, time range, priority, and more.

### A. Display All Available Journal Entries

```bash
journalctl
```

### B. Display the Latest Entries

```bash
journalctl -n 20
```

### C. Follow Logs in Real Time

Similar to `tail -f`:

```bash
journalctl -f
```

### D. Display Logs by Boot

Display logs from the current boot:

```bash
journalctl -b
```

Display logs from the previous boot:

```bash
journalctl -b -1
```

### E. Filter Logs by Service (`-u`)

Filtering by unit (`-u`) is essential for server administration.

Inspect SSH logs:

```bash
journalctl -u ssh
```

Follow SSH logs in real time:

```bash
journalctl -u ssh -f
```

Inspect `cron` logs:

```bash
journalctl -u cron
```

### F. Filter by Priority (`-p`)

The Linux journal supports standard syslog priority levels (from most severe `0` to least severe `7`):

| Priority | Meaning |
| :--- | :--- |
| `0` | Emergency |
| `1` | Alert |
| `2` | Critical |
| `3` | Error |
| `4` | Warning |
| `5` | Notice |
| `6` | Informational |
| `7` | Debug |

Display errors (`err`, priority `3`) and more severe messages:

```bash
journalctl -p err
```

### G. Filter by Time (`--since`)

Filter entries from a relative or exact timestamp:

```bash
journalctl --since "1 hour ago"
journalctl --since "today"
journalctl --since "2026-11-24 10:00:00"
```

Combine filters to narrow down results—for example, SSH errors from today:

```bash
journalctl -u ssh --since "today" -p err
```

---

## 5. Practical Troubleshooting Workflow

Imagine your SSH service is not working. A clean, methodical troubleshooting workflow looks like this:

1. **Check service state** to see if it is active, failed, or inactive:

   ```bash
   systemctl status ssh
   ```

2. **Inspect recent unit logs** for context around the failure:

   ```bash
   journalctl -u ssh -n 50
   ```

3. **Filter for errors** to isolate root causes:

   ```bash
   journalctl -u ssh -p err
   ```

4. **Follow live logs** while testing a new connection from another terminal:

   ```bash
   journalctl -u ssh -f
   ```

This step-by-step inspect-and-follow workflow is one you will use constantly when administering Linux servers.