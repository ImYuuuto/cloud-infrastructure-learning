## Process Management

### Viewing & Monitoring
* `ps` — Processes in current terminal
* `ps aux` — All processes (`a`: all users, `u`: user format, `x`: no terminal)
* `ps aux | grep <name>` — Filter for a specific process
* `top` — Real-time monitor (`q`: quit, `P`: sort CPU, `M`: sort RAM, `k`: kill/signal)
* `htop` — Interactive process monitor

### Terminating Processes
* `kill <PID>` — Graceful stop (sends `SIGTERM` / `15` by default)
* `kill -9 <PID>` — Force immediate stop (sends `SIGKILL` / `9`)
* `pkill <name>` — Kill by process name *(caution: stops all matches)*

| Signal | # | Purpose |
| :--- | :--- | :--- |
| **SIGTERM** | 15 | Graceful termination (Default) |
| **SIGKILL** | 9 | Force immediate termination |
| **SIGSTOP** | 19 | Pause process |
| **SIGCONT** | 18 | Resume stopped process |

### Foreground & Background Jobs
* `<cmd> &` — Start process in background (outputs `[Job#] <PID>`)
* `Ctrl + Z` — Suspend foreground process
* `Ctrl + C` — Interrupt/kill foreground process
* `jobs` — List active jobs in current shell
* `bg` — Resume suspended job in background
* `fg` — Bring background job to foreground