# Environment Variables

## 1. What Are Environment Variables?
Environment variables are dynamic, named key-value pairs stored in the operating system's runtime environment that running processes and applications can access. They separate configuration (such as API keys, database URLs, and ports) from application source code, eliminating the need to hardcode sensitive or environment-specific values.

---

## 2. Viewing Environment Variables
Use the following commands in a Unix-like shell (such as Bash or Zsh) to inspect variables:

* **`printenv`**: Prints all exported environment variables, or the value of a specific variable (e.g., `printenv PATH`).
* **`env`**: Displays the environment variables passed to a command, or runs a command in a modified environment.
* **`echo`**: Prints the value of a specific shell or environment variable using the `$` prefix (e.g., `echo $HOME`).

---

## 3. Creating Environment Variables in Bash
There are two primary ways to define variables in Bash, differing in scope and process inheritance:

### A. Local Shell Variables
Defining a variable without `export` makes it available **only** to the current shell session:

```bash
APP_NAME="Node API"
echo $APP_NAME  # Output: Node API
```

> **Note:** Local shell variables are **not** passed down to child processes (such as a Node.js server or Python script) started from that shell.

### B. Exported Environment Variables
Using the `export` command promotes a shell variable to a true environment variable, making it accessible to all child processes spawned from that shell:

```bash
export APP_NAME="Node API"
export PORT=4000
```

Any child process launched afterward (e.g., `node server.js`) can read `APP_NAME` and `PORT`. You can verify that a variable is exported using `printenv`:

```bash
printenv PORT  # Output: 4000
```

---

## 4. Scope & Inheritance Comparison

| Definition Method | Syntax Example | Available in Current Shell? | Inherited by Child Processes? |
| :--- | :--- | :---: | :---: |
| **Shell Variable** | `APP_NAME="Node API"` | Yes | **No** |
| **Environment Variable** | `export APP_NAME="Node API"` | Yes | **Yes** |

## 5. Removing Environment Variables
Use the `unset` command to delete a variable from both the current shell and any future child processes:

```bash
unset APP_NAME
```

Verify that the variable has been removed by printing it—the output should be blank:

```bash
echo $APP_NAME  # Output: (empty)
```
## 6. Temporary vs. Persistent Environment Variables
Understanding the difference between temporary and persistent variables is especially important when managing servers.

By default, variables created in a terminal session are **temporary**—they disappear as soon as you close the shell or log out. To make variables persist across new terminal sessions, define them inside a shell configuration file.

### Making Variables Persistent with `~/.bashrc`
`~/.bashrc` runs automatically when an interactive non-login Bash shell starts (such as opening a new terminal window or tab).

Add exported variables to `~/.bashrc`:

```bash
export APP_ENV=development
export PROJECT_DIR="$HOME/linux-practice"
```

Reload `~/.bashrc` so changes apply to your current session immediately (`source` runs the file's commands in the active shell):

```bash
source ~/.bashrc
```

Verify that the variables are loaded:

```bash
echo $APP_ENV          # Output: development
echo $PROJECT_DIR      # Output: /home/<username>/linux-practice
```

### Common Shell & Environment Configuration Files

| File | Purpose |
| :--- | :--- |
| `~/.bashrc` | Interactive non-login Bash shell configuration |
| `~/.profile` | User login-session configuration |
| `/etc/environment` | System-wide static environment variable assignments |
| `/etc/profile` | System-wide login-shell configuration |

> **Caution:** Avoid casually editing system-wide files like `/etc/environment` or `/etc/profile`. Mistakes there can affect every user or break login sessions on the system.

## 7. Environment Variables in Node.js
Connecting environment variables to backend development lets you configure servers per environment without changing application code.

In Node.js, `process.env` exposes the environment variables available to the running process. For example, reading a port with a fallback default:

```js
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Passing Variables When Starting Node.js
Set a variable inline for a single command (scoped only to that `node` process):

```bash
PORT=4000 node server.js
```

Or export it in the shell first so it persists across multiple commands in that session:

```bash
export PORT=5000
node server.js
```

### Important Security Practice
Never hardcode database passwords, API keys, tokens, or other secrets in source code—and never commit secrets or `.env` files containing real credentials to Git or GitHub.

While environment variables help separate configuration from code, they are not a full secret-management system on their own. In production, use dedicated secret managers or platform secret stores, and restrict access to the host and process environment.
