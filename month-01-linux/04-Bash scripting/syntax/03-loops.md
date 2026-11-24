# Bash Loops Cheat Sheet

## 1. `for` Loop

Iterates over a list of items, numbers, or files.

```bash
# Basic syntax
for variable in item1 item2 item3; do
    # Commands using "$variable"
done

# Number range
for num in {1..5}; do
    echo "$num"
done

# Number range with step ({start..end..step})
for num in {0..10..2}; do
    echo "$num"
done

# Looping through files (always quote "$file" for filenames with spaces)
for file in /var/log/*.log; do
    if [ -f "$file" ]; then
        ls -lh "$file"
    fi
done
```

## 2. `while` Loop

Executes repeatedly **while** the condition remains **true**.

```bash
counter=1

while [ "$counter" -le 5 ]; do
    echo "Iteration $counter"
    counter=$((counter + 1))   # Arithmetic increment
done
```

## 3. `until` Loop

Executes repeatedly **until** the condition becomes **true** (opposite of `while`).

```bash
counter=1

until [ "$counter" -gt 5 ]; do
    echo "$counter"
    counter=$((counter + 1))
done
```

## 4. Loop Control (`break` & `continue`)

```bash
# break: Exits the loop immediately
for num in {1..10}; do
    [ "$num" -eq 5 ] && break
    echo "$num"                # Prints 1 to 4
done

# continue: Skips current iteration and moves to the next
for num in {1..5}; do
    [ "$num" -eq 3 ] && continue
    echo "$num"                # Prints 1, 2, 4, 5
done
```

## 5. Arithmetic Expansion (Counters)

```bash
var=$((var + 1))               # Increment variable by 1
```