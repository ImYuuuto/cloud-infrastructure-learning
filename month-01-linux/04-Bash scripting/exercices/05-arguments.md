# Exercise 4 — Mini challenge: Service manager
Create service-manager.sh.

Your script should accept two arguments:
./service-manager.sh <service> <action>


Supported actions:
- status
- start
- stop
- restart
Requirements:
1. Check that exactly two arguments are provided.
2. Store the service name and action in variables.
3. Use case to handle the action.
4. Execute the appropriate systemctl command.
5. Display an error for unsupported actions.
6. Display a usage message if arguments are missing.
Example:
./service-manager.sh ssh status


Expected behavior: Display the status of SSH.

Important: Test with harmless services and use sudo where required.

```bash
#!/bin/bash

if [ $# -eq 2 ]; then
    service_name=$1
    action=$2

    case $action in
        status|start|stop|restart)
            systemctl "$action" "$service_name"
            ;;
        *)
            echo "Command not supported"
            ;;
    esac
else
    echo "Insufficient number of arguments"
fi
```