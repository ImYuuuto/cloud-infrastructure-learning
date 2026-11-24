# Exercise 3 — Service checker
Create service-checker.sh.

Requirements:
1. Create a function named check_service.
2. Accept a service name as an argument.
3. Use systemctl is-active --quiet.
4. Display whether the service is running.
5. Call the function for ssh, cron, and systemd-journald.
Exercise 4 — Mini challenge: Server health checker
Create health-check.sh.

Your script must have three functions:

1. check_disk
- Check disk usage using df.
- Display filesystem usage.
2. check_memory
- Check memory usage using free.
- Display memory information.
3. check_service
- Accept a service name.
- Check whether it is active.
- Display its status.
Finally, call all three functions.


```bash
#!/bin/bash

check_disk() {
    echo "--- DISK ---"
    echo "File system usage:"
    df -h
}

check_memory() {
    echo "--- MEMORY ---"
    echo "Memory information:"
    free -h
}

check_service() {
    echo "--- SERVICE ---"
    local service=$1
    if systemctl is-active --quiet "$service"; then
        echo "$service is running..."
    else
        echo "$service is NOT running"
    fi
}

echo "===== SERVER HEALTH CHECK ====="
check_disk
check_memory
check_service ssh
echo "===== CHECK COMPLETED ====="
```