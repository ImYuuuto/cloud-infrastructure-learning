# Bash Conditionals & Operators Cheat Sheet

## 1. Conditional Statements (`if` / `elif` / `else`)

*Rule: Ensure spaces exist inside the square brackets: `[ condition ]`.*

```bash
# Basic if
if [ condition ]; then
    # Commands
fi

# if / else
if [ condition ]; then
    # Commands if true
else
    # Commands if false
fi

# if / elif / else
if [ condition1 ]; then
    # Commands for condition 1
elif [ condition2 ]; then
    # Commands for condition 2
else
    # Fallback commands
fi
```

## 2. Numeric Comparison Operators

| Operator | Meaning | Example |
| :--- | :--- | :--- |
| `-eq` | Equal | `[ "$a" -eq "$b" ]` |
| `-ne` | Not equal | `[ "$a" -ne "$b" ]` |
| `-gt` | Greater than | `[ "$a" -gt 10 ]` |
| `-ge` | Greater than or equal | `[ "$a" -ge 18 ]` |
| `-lt` | Less than | `[ "$a" -lt 10 ]` |
| `-le` | Less than or equal | `[ "$a" -le 30 ]` |

## 3. String Comparison Operators

| Operator | Meaning | Example |
| :--- | :--- | :--- |
| `=` | Equal | `[ "$str" = "admin" ]` |
| `!=` | Not equal | `[ "$str" != "guest" ]` |
| `-z` | String is empty | `[ -z "$str" ]` |
| `-n` | String is not empty | `[ -n "$str" ]` |

## 4. Logical Operators

| Operator | Meaning | Example |
| :--- | :--- | :--- |
| `&&` | AND | `[ "$age" -ge 18 ] && [ "$age" -le 30 ]` |
| `||` | OR | `[ "$user" = "admin" ] || [ "$user" = "root" ]` |
| `!` | NOT | `[ ! -f "/path/to/file" ]` |

## 5. File & Directory Test Operators

| Operator | Meaning | Example |
| :--- | :--- | :--- |
| `-f` | File exists | `[ -f "/etc/ssh/sshd_config" ]` |
| `-d` | Directory exists | `[ -d "/var/log" ]` |
| `-e` | File or directory exists | `[ -e "/path/to/item" ]` |
| `-r` | Has read permission | `[ -r "file.txt" ]` |
| `-w` | Has write permission | `[ -w "file.txt" ]` |
| `-x` | Has execute permission | `[ -x "script.sh" ]` |

## 6. `case` Statement

Used to match a single variable against multiple patterns cleanly.

```bash
case "$variable" in
    pattern1)
        # Commands for pattern1
        ;;
    pattern2)
        # Commands for pattern2
        ;;
    *)
        # Default fallback (matches anything else)
        ;;
esac
```

*Syntax Notes:*
- `;;` terminates each case block.
- `*)` acts as the default wildcard catch-all.
- `esac` closes the `case` statement.