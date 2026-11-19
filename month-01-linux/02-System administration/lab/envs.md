# Practical Exercises
---
## Exercise 1 — Inspect

1. Display all environment variables:

```bash
printenv
```

2. Display only `HOME`, `USER`, `SHELL`, and `PATH`:

```bash
printenv HOME USER SHELL PATH
```

3. Display your current username and home directory using variables:

```bash
echo "User: $USER | Home: $HOME"
```

---

## Exercise 2 — Create and Export

1. Create a local shell variable named `PROJECT_NAME` with the value `TaskFlow`:

```bash
PROJECT_NAME="TaskFlow"
```

2. Display its value in the current shell:

```bash
echo "$PROJECT_NAME"
```

3. Export it so child processes can inherit it:

```bash
export PROJECT_NAME
```

4. Launch a child Bash shell:

```bash
bash
```

5. Check whether `PROJECT_NAME` is available inside the child shell:

```bash
echo "$PROJECT_NAME"
```

6. Exit the child shell to return to your parent shell:

```bash
exit
```

7. Remove the variable using `unset`:

```bash
unset PROJECT_NAME
echo "$PROJECT_NAME"  # Should output nothing (empty line)
```

> **Why exporting matters:** Without `export`, `PROJECT_NAME` only lives inside the shell where you defined it. Child processes (such as a subshell started with `bash` or a Node.js server) do not automatically inherit non-exported shell variables.

---

## Exercise 3 — Persistent Configuration

1. Open `~/.bashrc` in an editor (for example, `nano`):

```bash
nano ~/.bashrc
```

2. Add the following lines to `~/.bashrc` (save and exit):

```bash
export APP_ENV=development
export PROJECT_DIR="$HOME/linux-practice"
```

3. Reload `~/.bashrc` in your current terminal session:

```bash
source ~/.bashrc
```

4. Verify both variables:

```bash
echo "$APP_ENV"
echo "$PROJECT_DIR"
```

5. Open a **new** terminal window or tab (without running `source ~/.bashrc`) and verify that both variables are still available:

```bash
echo "$APP_ENV"
echo "$PROJECT_DIR"
```

---

## Exercise 4 — Independent Challenge

Suppose you are preparing a Node.js API for deployment and need `APP_NAME=TaskFlow`, `APP_ENV=production`, and `PORT=4000`.

1. Export all three variables in your current shell:

```bash
export APP_NAME="TaskFlow"
export APP_ENV="production"
export PORT=4000
```

2. Verify each variable individually:

```bash
printenv APP_NAME
printenv APP_ENV
printenv PORT
```

3. Launch a child Bash shell:

```bash
bash
```

4. Verify that all three variables are accessible inside the child shell:

```bash
echo "APP_NAME=$APP_NAME"
echo "APP_ENV=$APP_ENV"
echo "PORT=$PORT"
```

5. Exit the child shell:

```bash
exit
```

6. **How a Node.js application accesses these values:**
   When you start a Node.js process from a shell where these variables are exported, Node.js inherits them into `process.env`. Inside your application code, you read them via `process.env.APP_NAME`, `process.env.APP_ENV`, and `process.env.PORT` (typically with fallback defaults, such as `process.env.PORT || 3000`).