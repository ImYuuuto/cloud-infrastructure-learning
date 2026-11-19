# Process Management: Revision Sheet

## 1. What is a Process?
A **process** is a running instance of a program. Every process contains:
* **PID:** Process ID (unique identifier)
* **PPID:** Parent Process ID
* **User:** Account running the process
* **State:** Running, sleeping, stopped, etc.
* **CPU / Memory usage:** Resource consumption metrics

---

## 2. Viewing Processes

### `ps` (Process Status)
Displays process information.
* `ps`: Shows processes tied to the current terminal.
* `ps aux`: Comprehensive snapshot.
  * **`a`**: Processes from all users with terminals.
  * **`u`**: User-oriented format.
  * **`x`**: Processes without a controlling terminal.
* **Key Columns:** `USER`, `PID`, `%CPU`, `%MEM`, `COMMAND`
* **Filter Example:** `ps aux | grep node`

### `top` / `htop` (Real-Time Monitoring)
Continuously updates process statistics.
* **`q`** – Quit
* **`P`** – Sort by CPU usage
* **`M`** – Sort by memory usage
* **`k`** – Send a signal to a process

---

## 3. Terminating Processes

### `kill`
Sends **SIGTERM (15)** by default for a graceful shutdown.
* **Syntax:** `kill <PID>` (e.g., `kill 1250`)
* **Best Practice:** Always try normal termination (`kill`) first. Use force kill only if the process is unresponsive and cannot clean up resources.

### Important Signals
| Signal | Number | Purpose |
| :--- | :--- | :--- |
| **SIGTERM** | 15 | Request graceful termination (Default) |
| **SIGKILL** | 9 | Force immediate termination |
| **SIGSTOP** | 19 | Pause a process |
| **SIGCONT** | 18 | Resume a stopped process |

* **Force Kill:** `kill -9 <PID>`
* **By Name:** `pkill <process_name>` *(Caution: terminates all matches)*

---

## 4. Foreground & Background Processes

* **Foreground:** Occupies the terminal until it exits.
* **Background (`&`):** Runs asynchronously (e.g., `node server.js &`). Outputs `[Job Number] [PID]`.

### Job Management Commands & Shortcuts
| Command / Key | Action |
| :--- | :--- |
| **`jobs`** | List active jobs in the current shell |
| **`bg`** | Resume a stopped job in the background |
| **`fg`** | Bring a job back to the foreground |
| **`Ctrl + Z`** | Suspend the current foreground process |
| **`Ctrl + C`** | Interrupt/stop the current foreground process |

> **Workflow Example:** Run process $\rightarrow$ Press `Ctrl + Z` (Suspend) $\rightarrow$ Type `bg` (Resume in background) $\rightarrow$ Type `fg` (Bring back). 
> *Note: Shell background jobs die when the terminal is closed.*