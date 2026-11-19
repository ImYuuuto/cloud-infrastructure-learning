## PATH & Executable Permissions

### Inspecting & Locating Commands
- `echo $PATH` — Display colon-separated (`:`) directories searched for commands
- `which <command>` — Show exact path of executable Linux will run
- `command -v <command>` — Display command path, built-in status, or alias

### Permissions & Running Scripts
- `chmod +x <script>` — Add execute (`x`) permission to a script or binary
- `ls -l <script>` — Check permissions (look for `x` flags, e.g., `-rwxr-xr-x`)
- `./<script>` — Run an executable file in the current directory (`.`)
- `/path/to/<script>` — Run an executable using its full/relative path

### Adding Directories to `PATH`
- `export PATH="$PATH:<dir>"` — Append directory to `PATH` for current session *(temporary)*
- `echo 'export PATH="$PATH:~/scripts"' >> ~/.bashrc` — Persist directory in `PATH` for future Bash sessions
- `source ~/.bashrc` — Reload `~/.bashrc` in current shell session

| Concept / Syntax | Purpose / Key Detail |
| :--- | :--- |
| **`#!/bin/bash`** | Shebang on line 1 telling OS to run script with Bash |
| **`#!/usr/bin/python3`** | Shebang on line 1 telling OS to run script with Python 3 |
| **Left-to-right search** | First matching executable in `$PATH` wins if names collide |
| **Security rule** | Never add untrusted, world-writable, or `.` directories casually |