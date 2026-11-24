Exercise 4 — Mini challenge: Server monitoring simulation
Create server-monitor.sh.

Requirements:
1. Start with a variable server_status="offline".
2. Use a while loop to simulate checking the server.
3. Ask the user to enter the server status (online or offline).
4. If the server is online, display Server is running and stop the loop.
5. If it's offline, display WARNING: Server is offline.
6. Continue checking until the server becomes online.


```bash
#!/bin/bash

server_status="offline"

while [ "$server_status" = "offline" ]; do
        echo "Checking server....."

        read -p "Server status: " server_status

        if [ "$server_status" = "online" ]; then
                echo "Server is running....."
                break
        fi

        echo "WARINING: Server is offline"
done
```

