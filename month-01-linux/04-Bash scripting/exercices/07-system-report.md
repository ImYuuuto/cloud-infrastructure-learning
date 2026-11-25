# Exercise 4 — Mini challenge: System report generator
Create system-report.sh.

Your script should generate a report containing:
1. Current hostname.
2. Current username.
3. Disk information using df -h.
4. Memory information using free -h.
5. Running processes using ps aux.
Requirements:
- Save everything into system-report.txt.
- Use > for the initial report creation.
- Use >> for subsequent sections.
- Display the report in the terminal using tee where appropriate.
```bash
#!/bin/bash

host=$(hostname)
username=$(whoami)

echo "===== SYSTEM REPORT =====" > system-report.txt
echo "Hostname: $host" >> system-report.txt
echo "Username: $username" >> system-report.txt
echo "===== DISK INFORMATION =====" >> system-report.txt
df -h >> system-report.txt
echo "===== MEMORY INFORMATION =====" >> system-report.txt
free -h >> system-report.txt
echo "===== RUNNING PROCESSES =====" >> system-report.txt
ps aux >> system-report.txt
```