# Pipes and Redirection

**Pipes** and **redirection** let you control where command input comes from, where output goes, and how commands pass data to one another. In Linux server administration and automation, these mechanisms are essential for:

- Processing system logs.
- Filtering information.
- Saving command output.
- Automating backups.
- Troubleshooting Linux servers.

---

## 1. Understanding Standard Streams

Linux commands typically interact with three standard I/O streams:

| Stream | Name | File Descriptor |
| :--- | :--- | :--- |
| Standard input | `stdin` | `0` |
| Standard output | `stdout` | `1` |
| Standard error | `stderr` | `2` |

- **`stdin` (`0`):** Input data read by a command (for example, `read username` reads from the keyboard by default).
- **`stdout` (`1`):** Normal output produced by a command (for example, `echo "Hello Linux"`).
- **`stderr` (`2`):** Error or diagnostic messages produced by a command (for example, `ls /nonexistent` writes its error message to `stderr`).

Keeping `stdout` and `stderr` distinct lets you capture normal results and error messages independently.

---

## 2. Output Redirection

Redirection sends stream output to a file or another descriptor instead of the terminal screen.

### A. The `>` Operator (Overwrite)

Redirects `stdout` (descriptor `1`) to a file, creating it if needed or **overwriting** existing contents:

```bash
echo "Hello Linux" > output.txt
```

Example demonstrating overwrite behavior:

```bash
echo "First line" > notes.txt
echo "Second line" > notes.txt
```

`notes.txt` will contain only:

```text
Second line
```

### B. The `>>` Operator (Append)

Appends `stdout` to the end of a file without erasing existing content:

```bash
echo "First line" > notes.txt
echo "Second line" >> notes.txt
```

Resulting contents of `notes.txt`:

```text
First line
Second line
```

- `>` overwrites the target file.
- `>>` appends to the target file.

### C. Redirecting Errors with `2>`

Because `stderr` uses file descriptor `2`, you can direct error messages into a file:

```bash
ls /nonexistent 2> errors.txt
```

The error message goes to `errors.txt` instead of printing to the terminal.

### D. Redirecting Both `stdout` and `stderr`

Send both normal output and errors into a single file:

```bash
command > output.txt 2>&1
```

- `> output.txt` directs `stdout` to `output.txt`.
- `2>&1` redirects `stderr` (`2`) to the same target as `stdout` (`1`).

In Bash, you can also use the shorthand syntax:

```bash
command &> output.txt
```

---

## 3. Input Redirection (`<`)

Input redirection feeds a file's contents into a command's `stdin`:

```bash
sort < names.txt
```

Instead of typing lines manually into `sort`, it reads from `names.txt`.

Another common example—counting lines in a file:

```bash
wc -l < names.txt
```

---

## 4. Pipes (`|`)

A **pipe** (`|`) connects the `stdout` of one command directly to the `stdin` of the next command:

```bash
command1 | command2
```

For example, to list files in `/var/log` and filter for entries matching `syslog`:

```bash
ls /var/log | grep syslog
```

How this pipeline runs:

1. `ls /var/log` generates the list of files on `stdout`.
2. `|` passes that stream to `grep syslog` as `stdin`.
3. `grep syslog` outputs only matching lines.

### More Pipeline Examples

#### A. Search Inside Files

```bash
cat /var/log/syslog | grep error
```

While this works as a pipeline example, calling `grep` directly is cleaner and avoids an extra process:

```bash
grep error /var/log/syslog
```

#### B. Count Matching Lines

Case-insensitive search (`-i`) piped into line count (`wc -l`):

```bash
grep -i error /var/log/syslog | wc -l
```

#### C. Find Running Processes

```bash
ps aux | grep nginx
```

#### D. Show First Results (`head`)

```bash
ls /var/log | head -n 5
```

#### E. Show Last Results (`tail`)

```bash
ls /var/log | tail -n 5
```

---

## 5. Useful Commands for Processing Data

Several core CLI utilities pair naturally with pipes and log analysis:

| Command | Purpose |
| :--- | :--- |
| `grep` | Search lines for matching patterns/text |
| `sort` | Sort lines alphabetically or numerically |
| `wc` | Count lines (`-l`), words (`-w`), or bytes/characters (`-c`/`-m`) |
| `head` | Display the first lines of input (default `10`) |
| `tail` | Display the last lines of input (default `10`) |
| `uniq` | Filter out **adjacent** (consecutive) duplicate lines |
| `cut` | Extract specific columns/fields using a delimiter (`-d`) and field list (`-f`) |
| `tee` | Write input to both `stdout` and one or more files simultaneously |

Sort lines and collapse consecutive duplicates (`uniq` expects sorted input to deduplicate across the whole file):

```bash
cat access.log | sort | uniq
```

Extract the first space-delimited field from `access.log`:

```bash
cut -d ' ' -f 1 access.log
```

---

## 6. The `tee` Command

`tee` acts like a T-junction in a pipe: it displays output on the terminal while saving a copy to a file:

```bash
ls /var/log | tee log-files.txt
```

- Prints the output to the screen.
- Writes the output to `log-files.txt`.

Use `-a` to append to the file instead of overwriting it:

```bash
ls /var/log | tee -a log-files.txt
```

---

## 7. Real Infrastructure Example — Log Analysis

When investigating SSH issues on a Linux server, combining `journalctl` with filtering and output tools speeds up diagnosis:

Filter SSH unit logs for error entries (case-insensitive):

```bash
journalctl -u ssh | grep -i error
```

Count how many error entries match:

```bash
journalctl -u ssh | grep -i error | wc -l
```

Save matching error lines to a file:

```bash
journalctl -u ssh | grep -i error > ssh-errors.txt
```

View matching error lines in the terminal and save them to a file at the same time:

```bash
journalctl -u ssh | grep -i error | tee ssh-errors.txt
```

---

## 8. Bonus — `pipefail`

In a pipeline like:

```bash
command1 | command2
```

By default, Bash sets the pipeline's exit status to the exit status of the **last** command (`command2`). Even if `command1` fails, the pipeline can report success (`0`) if `command2` succeeds.

Enabling:

```bash
set -o pipefail
```

causes a pipeline to return a non-zero (failure) exit status if **any** command in the pipeline fails—making it a key part of `set -euo pipefail` in reliable automation scripts.