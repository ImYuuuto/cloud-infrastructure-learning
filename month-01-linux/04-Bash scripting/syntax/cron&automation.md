# Cron Jobs and Automation

**Lesson 8 · Final Lesson of Week 4**

**Automation** lets you run commands and scripts automatically on a schedule without manual intervention. In Linux server administration and cloud infrastructure, scheduled automation is used to:

- Back up important files every night.
- Check disk usage every hour.
- Clean temporary files every week.
- Generate a system report every morning.
- Monitor services periodically.

Instead of running repetitive tasks by hand, you can schedule them with **Cron**.

---

## 1. What Is Cron?

**Cron** is a Linux background service (daemon) that runs scheduled commands and scripts at specified times or intervals.

Two key terms to distinguish:

- **`cron`:** The background daemon responsible for executing scheduled jobs.
- **`crontab`:** The command-line utility used to create, view, edit, and remove scheduled task tables.

Check whether `cron` is active on Ubuntu:

```bash
systemctl status cron
```

Start or enable `cron` to run automatically at boot if needed:

```bash
sudo systemctl start cron
sudo systemctl enable cron
```

---

## 2. Managing Scheduled Tasks (`crontab`)

| Command | Purpose |
| :--- | :--- |
| `crontab -e` | Edit your user's scheduled tasks |
| `crontab -l` | List your currently scheduled tasks |
| `crontab -r` | Remove **all** of your scheduled tasks |

> **Warning:** `crontab -r` deletes your entire user crontab without prompting in many setups. Avoid running it casually.

To add or modify a scheduled job, run:

```bash
crontab -e
```

This opens your crontab in your configured text editor so you can add schedule entries.

---

## 3. Cron Syntax

A standard cron schedule line has five time/date fields followed by the command to execute:

```text
MIN  HOUR  DAY  MONTH  WEEK  command
```

| Field | Allowed Values | Range / Notes |
| :--- | :--- | :--- |
| **Minute** (`MIN`) | `0–59` | Minute of the hour |
| **Hour** (`HOUR`) | `0–23` | 24-hour clock (`0` = midnight) |
| **Day of month** (`DAY`) | `1–31` | Calendar day of the month |
| **Month** (`MONTH`) | `1–12` | Month of the year |
| **Day of week** (`WEEK`) | `0–7` | Both `0` and `7` represent Sunday |

### Schedule Examples

Every 5 minutes:

```cron
*/5 * * * * /home/yuuto/check-server.sh
```

Every day at 2:00 AM:

```cron
0 2 * * * /home/yuuto/backup.sh
```

Every Monday at 9:00 AM:

```cron
0 9 * * 1 /home/yuuto/weekly-report.sh
```

First day of every month at 8:30 AM:

```cron
30 8 1 * * /home/yuuto/monthly-report.sh
```

### Cron Operators and Symbols

| Symbol | Meaning | Example |
| :--- | :--- | :--- |
| `*` | Any / every value in the field | `*` in `HOUR` = every hour |
| `,` | List of discrete values | `1,5,10` |
| `-` | Inclusive range of values | `1-5` |
| `/` | Step / interval within a range | `*/10` (every 10 units) |

Example combining an interval with a fixed minute:

```cron
0 */6 * * * /home/yuuto/check-server.sh
```

This runs every 6 hours—at `00:00` (midnight), `06:00`, `12:00` (noon), and `18:00` (6:00 PM).

---

## 4. Important: The Cron Environment

Cron jobs run in a minimal, non-interactive environment with a restricted `PATH` and no terminal attached. Keep these rules in mind:

### 1. Use Absolute Paths

Prefer full paths for scripts and commands rather than relative paths:

```cron
0 2 * * * /home/yuuto/scripts/backup.sh
```

Avoid relying on `./backup.sh`.

### 2. Make Scripts Executable

Ensure your script has execute permissions (`+x`) and a valid shebang (such as `#!/bin/bash`):

```bash
chmod +x /home/yuuto/scripts/backup.sh
```

### 3. Redirect Output and Errors to Log Files

Because cron does not display output on an interactive screen, redirect `stdout` and `stderr` to a log file so you can audit runs and debug failures:

```cron
0 2 * * * /home/yuuto/backup.sh >> /home/yuuto/backup.log 2>&1
```

This appends both standard output and error messages to `backup.log`.

### 4. Do Not Assume a Working Directory

Cron does not start inside your project or current working directory. Always use full paths inside your scripts (or explicitly `cd` to a known directory inside the script).

### 5. User Crontab vs. Root Crontab

- `crontab -e`: Manages scheduled tasks for your **current user**.
- `sudo crontab -e`: Manages scheduled tasks for **`root`**.

Only use `sudo crontab -e` when the task genuinely requires root privileges.

---

## 5. Special Cron Schedules

Cron provides convenient shorthand keywords for common intervals:

| Shortcut | Equivalent / Meaning |
| :--- | :--- |
| `@reboot` | Runs once when the system starts up |
| `@hourly` | Runs once an hour (`0 * * * *`) |
| `@daily` | Runs once a day at midnight (`0 0 * * *`) |
| `@weekly` | Runs once a week (`0 0 * * 0`) |
| `@monthly` | Runs once a month on the 1st at midnight (`0 0 1 * *`) |

### Example

```cron
@reboot /home/yuuto/start-server.sh
```

This executes `/home/yuuto/start-server.sh` when the machine boots.