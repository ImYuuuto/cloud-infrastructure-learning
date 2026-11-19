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

### APT
* `sudo apt update` — Updates package information
* `sudo apt upgrade` — Upgrade installed packages
* `sudo apt install <name>` — Install software
* `sudo apt remove <name>` — Remove software
* `sudo apt autoremove` — Remove unnecessary dependencies
* `apt search <name>` — Searchs available package information
* `apt list --installed` — List installed packages
* `apt list --installed | grep <name>` — Check for specific package
* `apt show <name>` — 	Displays package information
* `which <name>` — 	 locates the exact file path of an executable program


### Environment Variables
- `printenv` — Display all exported environment variables
- `printenv <VAR>` — Display value of a specific exported variable (e.g., `printenv PORT`)
- `env` — Display environment variables or run a command with a modified environment
- `echo $<VAR>` — Display value of a shell or environment variable (e.g., `echo $APP_NAME`)
- `<VAR>="<value>"` — Create local shell variable *(no spaces around `=`; not inherited by child processes)*
- `export <VAR>="<value>"` — Create/export environment variable *(inherited by child processes)*
- `export <VAR>` — Export an existing shell variable to child processes
- `unset <VAR>` — Remove/delete a shell or environment variable
- `source ~/.bashrc` — Reload `~/.bashrc` in current shell session
- `<VAR>=<value> <cmd>` — Pass a variable to a single command without exporting to the shell (e.g., `PORT=4000 node server.js`)

| Config File | Scope / Purpose |
| :--- | :--- |
| `~/.bashrc` | Interactive non-login Bash shell configuration (per-user) |
| `~/.profile` | User login-session configuration (per-user) |
| `/etc/environment` | System-wide static environment assignments *(edit with care)* |
| `/etc/profile` | System-wide login-shell configuration *(edit with care)* |

