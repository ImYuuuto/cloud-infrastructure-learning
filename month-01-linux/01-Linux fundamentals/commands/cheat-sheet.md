# Linux Fundamentals — Command Cheat Sheet

## 1. Navigation

| Command | Description |
| --- | --- |
| `cd ~` | Go to the home directory |
| `pwd` | Display the current directory |
| `ls` | List directory contents |
| `cd ..` | Move to the parent directory |
| `cd -` | Return to the previous working directory |
| `ls -l` | Show a detailed listing |
| `ls -a` | Include hidden files |
| `ls -lh` | Show file sizes in human-readable form |
| `ls -R` | List directories recursively |

## 2. Creating files and directories

| Command | Description |
| --- | --- |
| `mkdir projects` | Create a directory |
| `mkdir -p /home/projects` | Create missing parent directories automatically |
| `touch index.js` | Create an empty file if it does not exist |
| `touch index.js server.js package.json` | Create multiple files |

## 3. Copying files and directories

| Command | Description |
| --- | --- |
| `cp index.js backup.js` | Copy a file to `backup.js` |
| `cp index.js backup/` | Copy a file into another directory |
| `cp -r project project-backup` | Copy a directory recursively |
| `cp -i index.js backup.js` | Prompt before overwriting a destination |

## 4. Moving and renaming

| Command | Description |
| --- | --- |
| `mv index.js scripts/` | Move a file |
| `mv project archive/` | Move a directory |
| `mv old-name new-name` | Rename a file or directory |

## 5. Deleting files and directories

| Command | Description |
| --- | --- |
| `rm index.js` | Delete a file |
| `rmdir old-folder` | Delete an empty directory |
| `rm -r old-folder` | Delete a directory and its contents |
| `rm -rf old-folder` | Force deletion without prompting |

## 6. Finding files and directories

| Command | Description |
| --- | --- |
| `find . -name "server.js"` | Find `server.js` under the current directory |
| `find . -name "*.js"` | Find JavaScript files under the current directory |
| `find . -type d` | Find directories |
| `find . -type f` | Find files |
| `find . -type f -size +10M` | Find files larger than 10 MB |

## 7. Wildcards

| Pattern | Description |
| --- | --- |
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[abc]` | Matches one character from the set |

## 8. Reading and manipulating files

| Command or key | Description |
| --- | --- |
| `cat notes.txt` | Display file contents |
| `cat -n notes.txt` | Display file contents with line numbers |
| `cat > notes.txt` | Write input into a file |
| `cat >> notes.txt` | Append input to a file |
| `less notes.txt` | Read a large file interactively |
| `Space` / `b` | Move forward / backward one page in `less` |
| `Arrow Down` / `Arrow Up` | Move down / up in `less` |
| `/error` | Search for `error` in `less` |
| `n` / `N` | Move to the next / previous search result in `less` |
| `q` | Quit `less` |
| `head notes.txt` | Display the first 10 lines |
| `head -n 5 notes.txt` | Display the first 5 lines |
| `tail notes.txt` | Display the last 10 lines |
| `tail -f /var/log/syslog` | Continuously display new lines added to the file |

## 9. Searching and counting

| Command | Description |
| --- | --- |
| `grep "error" server.log` | Search for a term in a file |
| `grep -i "error" server.log` | Search without case sensitivity |
| `grep -r "TODO" .` | Search recursively in directories |
| `grep -E "error|warning" server.log` | Search for multiple terms |
| `grep -v "error" server.log` | Show lines that do not match |
| `grep -c "error" server.log` | Count matching lines |
| `wc notes.txt` | Count lines, words, and bytes |
| `wc -l notes.txt` | Count lines |
| `wc -w notes.txt` | Count words |
| `wc -c notes.txt` | Count bytes |

## 10. Permissions, users, and groups

| Command | Description |
| --- | --- |
| `chmod` | Change permissions |
| `whoami` | Display the current user |
| `id` | Display user ID, group ID, and group memberships |
| `groups` | Display the current user's groups |
| `getent passwd` | Display user account entries |
| `getent group` | Display group entries |
| `useradd` | Create a user |
| `adduser` | Create a user interactively |
| `usermod` | Modify a user |
| `userdel` | Delete a user |
| `passwd` | Set or change a password |
| `groupadd` | Create a group |
| `groupdel` | Delete a group |
| `gpasswd` | Manage group membership |
| `getent` | Retrieve account or group information |
