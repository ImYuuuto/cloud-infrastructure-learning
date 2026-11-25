# Exercise 4 — Mini challenge: Backup validator
Create backup.sh.

Requirements:
1. Accept a source directory as an argument.
2. Check whether the argument exists and is a directory.
3. Create a compressed archive using tar.
4. Check whether the archive creation succeeded.
5. Display an appropriate message.
6. Exit with 0 on success and a non-zero code on failure.


```bash
#!/bin/bash

if [ $# -lt 1 ]; then
    echo "Usage: $0 <directory>"
    exit 1
fi

if [ ! -d "$1" ]; then
    echo "Error: '$1' is not a directory or does not exist"
    exit 1
fi

src=$1
backup_name="backup_test.tar.gz"

if tar -czf "$backup_name" "$src"; then
    echo "Backup created: $backup_name"
    exit 0
else
    echo "Failed to create backup"
    exit 1
fi
```