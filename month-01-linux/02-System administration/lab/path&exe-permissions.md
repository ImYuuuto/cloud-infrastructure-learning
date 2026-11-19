# Practical Exercises — `PATH` and Executable Permissions


---

## Exercise 1 — Executable Permissions

1. Create `~/linux-lab` and move into it:

```bash
mkdir -p ~/linux-lab
cd ~/linux-lab
```

2. Create `test.sh` using an editor (such as `nano`):

```bash
nano test.sh
```

3. Add a shebang and commands that print your current username and current working directory:

```bash
#!/bin/bash

echo "Username: $USER"
echo "Current directory: $(pwd)"
```

4. Check its permissions:

```bash
ls -l test.sh
```

5. Try running it before adding execute permission:

```bash
./test.sh
```

*(Expected result: a permission denied error because `x` is not set yet.)*

6. Grant execute permission and run it again:

```bash
chmod +x test.sh
./test.sh
```

---

## Exercise 2 — `PATH`

1. Create `~/mycommands`:

```bash
mkdir -p ~/mycommands
```

2. Create a Bash script named `greeting` inside `~/mycommands` (notice it does not need a `.sh` extension to be a valid command):

```bash
nano ~/mycommands/greeting
```

Example contents:

```bash
#!/bin/bash

echo "Hello, $USER! Welcome to Linux."
```

3. Make `greeting` executable:

```bash
chmod +x ~/mycommands/greeting
```

4. Run it using its absolute path:

```bash
~/mycommands/greeting
```

*(Or `/home/<your-username>/mycommands/greeting`.)*

5. Add `~/mycommands` to `PATH` for the current shell session:

```bash
export PATH="$PATH:$HOME/mycommands"
```

6. Change to another directory (such as `/tmp` or `~`):

```bash
cd /tmp
```

7. Run `greeting` by name without specifying a path:

```bash
greeting
```

8. **Why it works:** When you type `greeting` without a slash, Bash searches each directory listed in `$PATH` in order. Because `$HOME/mycommands` was added to `PATH` and `greeting` has execute (`x`) permission, Bash finds `~/mycommands/greeting` and executes it regardless of your current directory.

---

## Exercise 3 — Understanding

1. **What is the purpose of the `PATH` variable?**
   `PATH` stores a colon-separated list of directories that the shell searches in order when you enter a command name without a path, allowing you to run executables from any directory without typing their full path.

2. **Why do we use `./hello.sh` instead of `hello.sh` when we're inside the script's directory?**
   Linux does not search the current directory through `PATH` by default (for security reasons). `./` explicitly points to the current directory (`.`) so the shell knows where to find `hello.sh`.

3. **What does `chmod +x` do?**
   It adds the **execute (`x`)** permission flag to the target file so the operating system permits running it directly as a program or script.

4. **What is a shebang?**
   A shebang (`#!` on the very first line of a script, such as `#!/bin/bash` or `#!/usr/bin/python3`) tells the kernel which interpreter program should execute the file when it is run directly.

5. **What's the difference between `export PATH=...` and adding the command to `~/.bashrc`?**
   Running `export PATH=...` directly in the terminal only updates `PATH` for the current shell session (and its child processes) and resets when the session closes. Adding `export PATH=...` to `~/.bashrc` makes it run automatically whenever a new interactive non-login Bash shell starts, persisting across sessions.

6. **What happens if two directories in `PATH` contain executables with the same name?**
   The shell scans `PATH` from left to right and executes the **first** matching executable it encounters (you can verify which one wins with `which <command>` or `command -v <command>`).