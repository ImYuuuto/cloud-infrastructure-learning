# PATH and Executable Permissions

## 1. What is `PATH`?
Imagine you have a program or script called `hello` located inside:

```bash
/home/user/scripts/hello
```

To run it directly by its location, you can type its full (or relative) path:

```bash
/home/user/scripts/hello
```

But what if you want to run `hello` from **any** directory just by typing `hello`, without typing the full path every time?

That is what the **`PATH`** environment variable does: **`PATH` tells Linux which directories to search in order when you type a command name.**

### Inspecting Your `PATH`
View the directories currently in your `PATH`:

```bash
echo $PATH
```

You will see a colon-separated (`:`) list of directories, such as:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Each entry is a directory path separated by a colon (`:`). When you run a command like:

```bash
ls
```

Linux scans the directories in `PATH` from left to right and runs the first matching executable file named `ls` it finds.

## 2. How Linux Finds Commands
Suppose you type a command such as:

```bash
python3
```

Linux roughly follows this lookup flow:

1. **You enter a command name** (without a slash `/`, such as `python3`).
2. **Linux checks each directory in `$PATH` in order** (left to right).
3. **If it finds a matching executable file:** it runs that program and stops searching.
4. **If no matching executable is found in any `PATH` directory:** Linux returns:

```text
command not found
```

### Finding Where a Command Lives
Check which executable file Linux will run for a command using `which` or `command -v`:

```bash
which python3
```

Or:

```bash
command -v python3
```

Example output:

```text
/usr/bin/python3
```

This shows the exact path to the executable Linux resolved when you ran `python3`.

## 3. Executable Permissions
Remember the file permissions model from earlier? Linux files have three basic permission flags:

| Permission | Meaning |
| :--- | :--- |
| **`r`** | Read (view contents) |
| **`w`** | Write (modify or delete contents) |
| **`x`** | Execute (run as a program or script) |

The **`x` (execute)** permission is essential if you want to run a script or program directly.

### Creating a Sample Script
Create a scripts directory, move into it, and create `hello.sh`:

```bash
mkdir ~/scripts
cd ~/scripts
nano hello.sh
```

Add the following content to `hello.sh` (save and exit):

```bash
#!/bin/bash

echo "Hello from my first script!"
```

> **Note:** The first line (`#!/bin/bash`) is a **shebang**, which tells the kernel which interpreter should execute the script.

### Checking Permissions and Testing Execution
Inspect the file's permissions with `ls -l`:

```bash
ls -l hello.sh
```

You will likely see something like:

```text
-rw-r--r-- 1 user user 48 hello.sh
```

There is no `x` flag set yet. If you try to run it directly:

```bash
./hello.sh
```

You will get a permission denied error (`bash: ./hello.sh: Permission denied`).

### Adding Execute Permission (`chmod +x`)
Grant execute permission to the file:

```bash
chmod +x hello.sh
```

Verify the updated permissions:

```bash
ls -l hello.sh
```

You should now see `x` flags in the permission string, such as:

```text
-rwxr-xr-x 1 user user 48 hello.sh
```

Now run the script:

```bash
./hello.sh
```

Output:

```text
Hello from my first script!
```

> **Important:** `./hello.sh` explicitly tells Linux to run `hello.sh` from the **current directory** (`.`). Linux does **not** include the current directory in `$PATH` by default for security reasons, so typing `hello.sh` alone won't work unless its directory is added to `PATH`.

## 4. Understanding the Shebang
Look at the first line of your script:

```bash
#!/bin/bash
```

This `#!` sequence at the very top of a script is called the **shebang** (or hashbang). It tells the operating system kernel which interpreter program should run the rest of the file when executed directly.

### Common Shebang Examples
* **`#!/bin/bash`** — Runs the script using the Bash shell.
* **`#!/usr/bin/python3`** — Runs the script using the Python 3 interpreter.

### Example with a Python Script
Create `hello.py` (making sure the shebang is on its own first line and the code starts on the next line):

```python
#!/usr/bin/python3

print("Hello from Python!")
```

Grant execute permission:

```bash
chmod +x hello.py
```

Run it directly:

```bash
./hello.py
```

Output:

```text
Hello from Python!
```

Because of the `#!/usr/bin/python3` shebang, you can run `./hello.py` directly instead of manually typing `python3 hello.py`.

## 5. Adding Your Own Directory to `PATH`
Now let's make your scripts runnable from anywhere without typing `./` or the full path.

Suppose your script is located at:

```bash
/home/user/scripts/hello.sh
```

Running `./hello.sh` only works when your current working directory is `/home/user/scripts`. By adding `~/scripts` to `PATH`, you can invoke `hello.sh` from any directory.

### Temporarily (Current Session Only)
Append `$HOME/scripts` to `PATH` in your current shell:

```bash
export PATH="$PATH:$HOME/scripts"
```

Move to another directory (such as your home directory) and run the script by name:

```bash
cd ~
hello.sh
```

Output:

```text
Hello from my first script!
```

> **Note:** This `export` only lasts for the current shell session and disappears when you close the terminal.

### Permanently (Across New Bash Sessions)
Open `~/.bashrc` in an editor:

```bash
nano ~/.bashrc
```

Add the following line at the bottom of the file (save and exit):

```bash
export PATH="$PATH:$HOME/scripts"
```

Reload `~/.bashrc` so your current session picks up the change right away:

```bash
source ~/.bashrc
```

Whenever you open a new interactive Bash shell, `~/scripts` will already be part of your `PATH`.

### Key Commands to Remember
* `echo $PATH` — Inspect the colon-separated directories currently in `PATH`.
* `which <command>` — Check which executable file Linux resolves for a command name.
* `export PATH="$PATH:$HOME/scripts"` — Add `~/scripts` to `PATH` for the current session (put it in `~/.bashrc` to persist).

## 6. Important Security Considerations
Misconfiguring `PATH` can introduce serious security risks because Linux executes the **first** matching command it finds when scanning `PATH` directories from left to right.

Imagine an untrusted or shared directory placed *before* `/usr/bin` in `PATH` contains a malicious or fake `ls` script/binary. When you run:

```bash
ls
```

Linux may run that fake `ls` instead of `/usr/bin/ls`—potentially exposing sensitive files, running unwanted commands, or spoofing output.

### Best Practices for Safe `PATH` Configuration
* **Never add untrusted directories to `PATH`:** Only include directories you control or trust.
* **Avoid putting world-writable or shared directories early in `PATH`:** If other users can write to a directory in your `PATH`, they could place malicious executables there.
* **Avoid adding `.` (the current directory) to `PATH`:** If `.` is in `PATH` (especially near the front) and you `cd` into a shared or untrusted directory containing a rogue script named `ls` or `git`, running a routine command could execute that script unexpectedly.
* **Append user directories thoughtfully:** Putting your own scripts directory after system directories (`export PATH="$PATH:$HOME/scripts"`) reduces the chance of accidentally shadowing standard system commands.