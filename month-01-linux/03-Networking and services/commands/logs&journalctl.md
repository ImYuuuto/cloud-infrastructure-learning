## Cheat Sheet 1: Logs, `systemctl`, and `journalctl`

### Traditional Log Files (`/var/log/`)

| Command | Purpose |
| :--- | :--- |
| `ls -la /var/log/` | List log files in `/var/log` |
| `cat /var/log/syslog` | Print an entire log file at once (best for small files) |
| `less /var/log/syslog` | Scroll and search large logs interactively (`Space` next page, `b` back, `/term` search, `n` next match, `q` quit) |
| `tail /var/log/syslog` | Show the last 10 lines of a file |
| `tail -n 20 /var/log/syslog` | Show the last `N` lines (here, `20`) |
| `tail -f /var/log/syslog` | Stream new log lines in real time (`Ctrl + C` to stop) |

### Service Status (`systemctl`)

| Command | Purpose |
| :--- | :--- |
| `systemctl status ssh` | Check active state, PID, recent log snippet, and failure reason for a service |

### Systemd Journal (`journalctl`)

| Command | Purpose |
| :--- | :--- |
| `journalctl` | View all available journal entries |
| `journalctl -n 30` | Show the most recent `N` entries (here, `30`) |
| `journalctl -f` | Follow live journal entries in real time (`Ctrl + C` to stop) |
| `journalctl -b` | Show logs from the current boot |
| `journalctl -b -1` | Show logs from the previous boot |
| `journalctl -u ssh` | Filter logs by systemd service unit (e.g., `ssh`, `cron`) |
| `journalctl -u ssh -f` | Follow live logs for a specific service unit |
| `journalctl -p err` | Filter by minimum priority (`0` emerg, `1` alert, `2` crit, `3` err, `4` warning, `5` notice, `6` info, `7` debug) |
| `journalctl --since "1 hour ago"` | Show logs since a relative time (`"1 hour ago"`, `"today"`) or exact timestamp (`"2026-11-24 10:00:00"`) |
| `journalctl -u ssh --since "today" -p err` | Combine unit, time, and priority filters |

---

