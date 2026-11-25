# System Administration — Command Cheat Sheet

## Process management

### Viewing and monitoring

- `ps` — Show processes in the current terminal
- `ps aux` — Show all processes (`a`: all users, `u`: user format, `x`: no terminal)
- `ps aux | grep <name>` — Filter for a specific process
- `top` — Monitor processes in real time (`q`: quit, `P`: sort by CPU, `M`: sort by RAM, `k`: send a signal)
- `htop` — Open an interactive process monitor

### Terminating processes

- `kill <PID>` — Request a graceful stop (sends `SIGTERM` / `15` by default)
- `kill -9 <PID>` — Force an immediate stop (sends `SIGKILL` / `9`)
- `pkill <name>` — Stop processes by name *(caution: stops all matches)*

| Signal | Number | Purpose |
| --- | ---: | --- |
| `SIGTERM` | 15 | Graceful termination (default) |
| `SIGKILL` | 9 | Force immediate termination |
| `SIGSTOP` | 19 | Pause a process |
| `SIGCONT` | 18 | Resume a stopped process |

### Foreground and background jobs

- `<cmd> &` — Start a process in the background (outputs `[Job#] <PID>`)
- `Ctrl + Z` — Suspend the foreground process
- `Ctrl + C` — Interrupt the foreground process
- `jobs` — List active jobs in the current shell
- `bg` — Resume a suspended job in the background
- `fg` — Bring a background job to the foreground

## Package management with APT

- `sudo apt update` — Update package information
- `sudo apt upgrade` — Upgrade installed packages
- `sudo apt install <name>` — Install software
- `sudo apt remove <name>` — Remove software
- `sudo apt autoremove` — Remove unnecessary dependencies
- `apt search <name>` — Search available package information
- `apt list --installed` — List installed packages
- `apt list --installed | grep <name>` — Check for a specific package
- `apt show <name>` — Display package information
- `which <name>` — Locate the executable program's path

## Environment variables

- `printenv` — Display all exported environment variables
- `printenv <VAR>` — Display a specific exported variable (for example, `printenv PORT`)
- `env` — Display environment variables or run a command with a modified environment
- `echo $<VAR>` — Display a shell or environment variable (for example, `echo $APP_NAME`)
- `<VAR>="<value>"` — Create a local shell variable *(no spaces around `=`; not inherited by child processes)*
- `export <VAR>="<value>"` — Create and export a variable *(inherited by child processes)*
- `export <VAR>` — Export an existing shell variable to child processes
- `unset <VAR>` — Remove a shell or environment variable
- `source ~/.bashrc` — Reload `~/.bashrc` in the current shell session
- `<VAR>=<value> <cmd>` — Pass a variable to one command without exporting it to the shell (for example, `PORT=4000 node server.js`)

| Config file | Scope or purpose |
| --- | --- |
| `~/.bashrc` | Interactive non-login Bash shell configuration (per-user) |
| `~/.profile` | User login-session configuration (per-user) |
| `/etc/environment` | System-wide static environment assignments *(edit with care)* |
| `/etc/profile` | System-wide login-shell configuration *(edit with care)* |
