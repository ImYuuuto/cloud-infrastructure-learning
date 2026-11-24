# Bash Functions

**Functions** allow you to group reusable blocks of commands into named units. Instead of repeating the same logic across a script, you define a function once and call it whenever needed.

In infrastructure automation and Bash scripting, functions are especially useful for:

- Checking server health.
- Monitoring disk usage.
- Checking service status.
- Creating backups.
- Managing users.

---

## 1. Creating a Function

### Syntax

```bash
function_name() {
    # Commands
}
```

### Example

```bash
#!/bin/bash

greet() {
    echo "Hello, welcome to Linux!"
}

greet
```

**Output:**

```text
Hello, welcome to Linux!
```

**How it works:**

- `greet()` defines the function name and parameter list.
- `{ ... }` contains the commands executed when the function is called.
- `greet` invokes (runs) the function.

You can also use the `function` keyword:

```bash
function greet {
    echo "Hello!"
}
```

Both syntaxes are valid in Bash; `name() { ... }` is concise and widely used.

---

## 2. Functions with Arguments

Just like scripts, functions accept positional arguments when called.

### Example

```bash
#!/bin/bash

greet() {
    echo "Hello $1"
}

greet "Ahmed"
```

**Output:**

```text
Hello Ahmed
```

### Argument Variables Inside a Function

| Variable | Meaning |
| :--- | :--- |
| `$1` | First argument passed to the function |
| `$2` | Second argument passed to the function |
| `$3` | Third argument passed to the function |
| `$@` | All arguments passed to the function |
| `$#` | Total number of arguments passed to the function |

### Example with Multiple Arguments

```bash
#!/bin/bash

user_info() {
    echo "Name: $1"
    echo "Age: $2"
}

user_info "Ahmed" 21
```

**Output:**

```text
Name: Ahmed
Age: 21
```

---

## 3. Returning Values from Functions

Bash functions communicate results back to the caller in two primary ways: **output via `echo`/command substitution** and **exit status via `return`**.

### A. Using `echo` and Command Substitution (`$(...)`)

Use `echo` (or another command's standard output) and capture it with `$(...)` when returning text, paths, or computed values:

```bash
#!/bin/bash

get_hostname() {
    hostname
}

server=$(get_hostname)

echo "Server: $server"
```

**How it works:**

1. `get_hostname` runs `hostname` and prints the result to standard output.
2. `$(get_hostname)` captures that output.
3. The captured value is stored in the `server` variable.

### B. Using `return` (Exit Status)

In Bash, `return` sets the **exit status** (an integer from `0` to `255`) of the function—not arbitrary strings or large return values:

```bash
#!/bin/bash

check_number() {
    if [ "$1" -gt 10 ]; then
        return 0
    else
        return 1
    fi
}

check_number 15

if [ $? -eq 0 ]; then
    echo "Number is greater than 10"
else
    echo "Number is 10 or less"
fi
```

- `return 0` indicates success / true condition.
- `return 1` (or any non-zero code) indicates failure / false condition.
- `$?` holds the exit status of the most recently executed command or function.

---

## 4. Local Variables

By default, variables assigned inside a Bash function live in the current shell scope and can overwrite or leak into surrounding variables. Use `local` to scope a variable strictly inside the function.

### Example

```bash
#!/bin/bash

show_user() {
    local username="Ahmed"
    echo "Inside function: $username"
}

show_user

echo "Outside function: $username"
```

The second `echo` prints nothing (empty) if no global `username` variable exists outside the function.

> **Best Practice:** Always declare temporary or internal helper variables inside a function using `local` to avoid unintended side effects across your script.

---

## 5. Practical Infrastructure Example

Suppose you want to verify whether several services are active on a Linux server. Instead of repeating `systemctl` checks, wrap the check in a reusable function:

```bash
#!/bin/bash

check_service() {
    local service=$1

    if systemctl is-active --quiet "$service"; then
        echo "$service is running"
    else
        echo "$service is not running"
    fi
}

check_service ssh
check_service cron
```

**How it works:**

1. Accepts a service unit name as `$1` and stores it in `local service`.
2. Uses `systemctl is-active --quiet "$service"` to test whether the service is active.
3. Prints whether the service is running or not—ready to call for any list of services.