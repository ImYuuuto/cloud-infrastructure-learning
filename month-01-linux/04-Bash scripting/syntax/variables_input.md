# Bash Variables & Input Cheat Sheet

## Script Basics (Recap)
```bash
#!/bin/bash          # Shebang (specifies Bash interpreter)
chmod +x script.sh   # Make script executable
./script.sh          # Run script
echo "text"          # Print output
```

---

## 1. Creating Variables
*Rule: **No spaces** around the `=` sign.*
```bash
var_name="value"     # String
num_var=21           # Number
```

---

## 2. Accessing Variables
```bash
$var_name            # Standard access
${var_name}          # Curly braces (safe for concatenation)
"${var_name}_admin"  # Combining variable with adjacent text
```

---

## 3. Command Substitution
Store the output of a command inside a variable:
```bash
var_name=$(command)

# Examples:
current_user=$(whoami)
current_dir=$(pwd)
```

---

## 4. Reading User Input
```bash
read var_name                              # Basic input
read -p "Prompt message: " var_name        # Input with inline prompt (-p)
read -sp "Password: " password_var         # Silent/hidden input (-s) + prompt (-p)
read -p "Enter first & last: " var1 var2   # Read multiple space-separated values
```

---

## 5. Local vs. Environment Variables
```bash
var_name="value"         # Local shell variable (current shell only)
export var_name="value"  # Environment variable (inherited by child processes)
```
*Tip: Add `export` statements to `~/.bashrc` to make environment variables persistent.*