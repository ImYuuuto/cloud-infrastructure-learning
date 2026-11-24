Exercise 4 — Mini challenge: Server environment selector

Create environment-selector.sh.

Requirements:
1. Ask the user to select dev, test, or prod.
2. Use case to identify the environment.
3. Display the environment name.
4. Display an appropriate message:
   - dev: Debugging enabled.
   - test: Testing mode enabled.
   - prod: Production mode enabled.
5. Invalid input should display an error message.  GNU nano 8.7.

## answer

#! /bin/bash

read -p "Select dev-test-prod: " answer

case "$answer" in
        dev)
        echo "Debugging enabled"
        ;;
        test)
        echo "Testing mode enabled"
        ;;
        prod)
        echo "Production mode enabled"
        ;;
        *)
        echo "error"
esac










