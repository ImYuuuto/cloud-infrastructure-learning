# Bash Exit Codes

**Exit codes** are critical in Linux administration and automation. They let commands, pipelines, and scripts report whether an operation succeeded or failed so caller scripts or automation tools can react accurately.

---

## 1. What Are Exit Codes?

Every command executed in Linux returns an integer **exit status** (from `0` to `255`) when it finishes.

| Exit Code | Meaning |
| :--- | :--- |
| `0` | Success |
| `1` | General error |
| `2` | Incorrect command usage (common convention) |
| `126` | Command found, but not executable (permission issue) |
| `127` | Command not found |
| `128+` | Terminated by a signal (for example, `130` for `SIGINT` / `Ctrl + C`, `137` for `SIGKILL`) |

The core rule to remember:

- **`0`** = Success.
- **Non-zero (`1`–`255`)** = Failure or abnormal termination.

### Example

```bash
ls
echo $?
```

If `ls` succeeds, it outputs:

```text
0
```

Now test a failing command:

```bash
ls /directory_that_does_not_exist
echo $?
```

This prints a non-zero exit code (typically `2` for `ls` when a path cannot be accessed).

---

## 2. The `$?` Variable

`$?` holds the exit status of the **most recently executed** command.

```bash
mkdir test_folder
echo $?
```

If `mkdir` succeeds, `$?` is `0`.

> **Watch Out:** `$?` is overwritten by *every* command you run next.

In the snippet below, `echo` runs after `mkdir`, so `$?` reflects `echo` (usually `0`), not `mkdir`:

```bash
mkdir test_folder
echo "Directory creation attempted"
echo $?
```

To preserve a command's exit status before running other commands, save it to a variable immediately:

```bash
mkdir test_folder
result=$?

echo "Exit code: $result"
```

---

## 3. Using Exit Codes with Conditions

You can inspect `$?` explicitly inside an `if` test:

```bash
mkdir test_folder

if [ $? -eq 0 ]; then
    echo "Directory created successfully"
else
    echo "Failed to create directory"
fi
```

However, Bash lets you pass the command directly to `if`—which is cleaner and avoids accidental `$?` overwrites:

```bash
if mkdir test_folder; then
    echo "Directory created successfully"
else
    echo "Failed to create directory"
fi
```

Using the command directly in `if ...; then` is generally preferred in Bash scripts.

---

## 4. The `exit` Command

Inside a script, `exit [N]` stops execution immediately and returns exit code `N` (or the previous command's exit status if `N` is omitted) to the calling shell or process:

```bash
exit 0
```

Signals successful completion.

```bash
exit 1
```

Signals an error/failure.

### Example

```bash
#!/bin/bash

if [ ! -d "/var/log" ]; then
    echo "Log directory does not exist"
    exit 1
fi

echo "Log directory exists"
exit 0
```

As soon as `exit` runs, no further lines in the script execute.

---

## 5. Why Exit Codes Matter in Automation

Consider an automated backup pipeline that:

1. Creates a backup archive.
2. Compresses/packages the data.
3. Transfers the archive to a remote server.

If step 1 fails, continuing to step 2 or 3 can corrupt state, waste resources, or upload incomplete/missing data. Checking exit codes lets you halt early on failure:

```bash
#!/bin/bash

if tar -czf backup.tar.gz /etc; then
    echo "Backup created successfully"
else
    echo "Backup failed"
    exit 1
fi

echo "Backup process completed"
```

If `tar` fails, the script prints `"Backup failed"` and exits with status `1` instead of claiming the backup completed.

---

## 6. Bonus — `set -e` and `set -u`

Bash provides shell options that make scripts fail fast when errors or missing variables occur.

### `set -e` (Exit on Error)

Causes the script to exit immediately if a simple command returns a non-zero exit status (with exceptions such as commands inside `if` conditions, `&&`/`||` lists, or negated `!` pipelines):

```bash
#!/bin/bash

set -e

mkdir backup
cp important.txt backup/

echo "Backup completed"
```

If `mkdir` or `cp` fails in this flow, the script terminates instead of continuing past the failure.

### `set -u` (Treat Unset Variables as Errors)

Treats referencing an undefined variable as an error and exits the script rather than silently expanding it to an empty string:

```bash
#!/bin/bash

set -u

echo "$username"
```

If `username` was never set, Bash reports an unbound variable error and stops.

In strict automation scripts, administrators frequently combine these options:

```bash
set -euo pipefail
```

Here, `-o pipefail` ensures a pipeline fails if *any* command in the pipe fails, not just the last command.