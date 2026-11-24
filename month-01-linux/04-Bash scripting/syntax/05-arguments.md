# Bash Script Arguments

So far, collecting input with `read` requires interactive prompts while the script runs. **Command-line arguments** let you pass values directly when invoking a script, making automation non-interactive, scriptable, and repeatable.

---

## 1. Passing Arguments to a Script

Suppose you create `greet.sh`:

```bash
#!/bin/bash

echo "Hello $1"
```

Run it with an argument:

```bash
./greet.sh Ahmed
```

**Output:**

```text
Hello Ahmed
```

Here, `$1` holds the first positional argument. You can pass multiple space-separated arguments:

```bash
./greet.sh Ahmed 21 Morocco
```

Inside the script, each positional parameter maps in order:

| Variable | Value |
| :--- | :--- |
| `$1` | `Ahmed` |
| `$2` | `21` |
| `$3` | `Morocco` |

Example reading multiple arguments:

```bash
#!/bin/bash

echo "Name: $1"
echo "Age: $2"
echo "Country: $3"
```

---

## 2. Important Special Variables

Bash provides special parameters for inspecting the script invocation and argument list:

| Variable | Meaning |
| :--- | :--- |
| `$0` | Script name or path as invoked |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$#` | Total number of arguments passed |
| `$@` | All arguments as separate items |
| `$*` | All arguments as a single combined string (when quoted) |

Example:

```bash
#!/bin/bash

echo "Script: $0"
echo "First argument: $1"
echo "Number of arguments: $#"
echo "All arguments: $@"
```

Execution:

```bash
./example.sh Ahmed 21 Morocco
```

**Output:**

```text
Script: ./example.sh
First argument: Ahmed
Number of arguments: 3
All arguments: Ahmed 21 Morocco
```

---

## 3. Difference Between `"$@"` and `"$*"`

When quoted, `"$@"` and `"$*"` behave differently—especially when an argument contains spaces.

Using `"$@"`:

```bash
#!/bin/bash

for arg in "$@"; do
    echo "Argument: $arg"
done
```

Execution:

```bash
./example.sh "Ahmed Benali" 21 Morocco
```

**Output:**

```text
Argument: Ahmed Benali
Argument: 21
Argument: Morocco
```

Using `"$*"`:

```bash
#!/bin/bash

for arg in "$*"; do
    echo "Argument: $arg"
done
```

**Output:**

```text
Argument: Ahmed Benali 21 Morocco
```

- **`"$@"`** preserves each argument as its own distinct item (respecting quotes and spaces).
- **`"$*"`** joins all arguments into one single string separated by the first character of `IFS` (by default, a space).

> **Best Practice:** Always use `"$@"` when iterating over or forwarding arguments individually.

---

## 4. Checking Whether Arguments Exist

Never assume a caller passes arguments unless you check. Use `$#` to validate argument count before using `$1`, `$2`, etc.

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Error: No arguments provided"
    exit 1
fi

echo "Hello $1"
```

Running with no arguments:

```bash
./greet.sh
```

**Output:**

```text
Error: No arguments provided
```

Running with an argument:

```bash
./greet.sh Ahmed
```

**Output:**

```text
Hello Ahmed
```

`exit 1` exits the script immediately with a non-zero status code to signal failure.

---

## 5. The `shift` Command

`shift` drops the first positional argument (`$1`), shifts remaining arguments down by one (`$2` becomes `$1`, `$3` becomes `$2`, and so on), and decrements `$#` by `1`.

```bash
#!/bin/bash

echo "First: $1"

shift

echo "First after shift: $1"
```

Execution:

```bash
./example.sh Ahmed Sara Youssef
```

**Output:**

```text
First: Ahmed
First after shift: Sara
```

You can also shift by `N` positions at once:

```bash
shift 2
```

### Practical Use of `shift`

A classic pattern for processing an arbitrary number of arguments one by one:

```bash
#!/bin/bash

while [ $# -gt 0 ]; do
    echo "Processing: $1"
    shift
done
```

Execution:

```bash
./process.sh file1.txt file2.txt file3.txt
```

**Output:**

```text
Processing: file1.txt
Processing: file2.txt
Processing: file3.txt
```

This loop is widely used in CLI utilities and automation scripts.

---

## 6. Using Arguments in Infrastructure Scripts

Command-line arguments make administrative scripts flexible without editing the file for every run. Here is `service-manager.sh`:

```bash
#!/bin/bash

service_name=$1
action=$2

if [ "$action" = "start" ]; then
    sudo systemctl start "$service_name"
elif [ "$action" = "stop" ]; then
    sudo systemctl stop "$service_name"
else
    echo "Invalid action"
fi
```

Usage examples:

```bash
./service-manager.sh ssh start
```

```bash
./service-manager.sh cron stop
```

> **Production Note:** In real-world automation, also verify that required arguments are provided (`$#`), validate allowed service names and actions (such as `start`, `stop`, `restart`, `status`), and ensure proper permissions before executing privileged commands.