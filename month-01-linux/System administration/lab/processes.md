Exercise 1 — Process inspection
1. Display all processes.
2. Find processes belonging to your current user.
3. Find the PID of your Bash process.
4. Find Node.js processes (if any).

Exercise 2 — Process monitoring
1. Launch top.
2. Identify the process using the most CPU.
3. Identify the process using the most memory.
4. Exit using q.

Exercise 3 — Background processes
1. Start a long-running process in the background using sleep 300.
2. Display your jobs.
3. Bring the process to the foreground.
4. Suspend it using Ctrl + Z.
5. Resume it in the background.
6. Terminate it gracefully.

Exercise 4 — Independent challenge

Imagine you've deployed a Node.js API on an Ubuntu server.

You suspect that two instances of the API are running.

Find:
- How many Node.js processes are running.
- Their PIDs.
- Their owners.
- Their CPU and memory consumption.
Don't terminate anything yet.