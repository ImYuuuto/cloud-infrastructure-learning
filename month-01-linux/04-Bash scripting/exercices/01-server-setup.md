# Exercise 4 — Mini challenge
Create server-setup.sh.

Your script should:
1. Ask the user for a server name.
2. Ask for an application name.
3. Store the current username automatically.
4. Store the current hostname automatically.
5. Display a configuration summary.

## Answer
  GNU nano 8.7.1                                                                                    server-setup.sh
#! /bin/bash

read -p "Enter server name: " serverName
read -p "Enter app name: " appName

username=$(whoami)
host=$(hostname)

echo "===== SERVER CONFIGURATION ====="
echo "Server name: $serverName"
echo "Application: $appName"
echo "Administrator: $username"
echo "Machine hostname: $host"
echo "                         "
echo "configuration completed."
