7. Practical lab — Secure a Node.js project
Let's apply everything to a development scenario.

Navigate to your project:
cd ~/linux-practice/projects/node-api

Create two files:
touch server.sh .env


Now perform these tasks:
1. Give server.sh read, write and execute permissions for the owner, and read/execute for everyone else.
2. Give .env read/write permissions only to its owner.
3. Inspect both files using ls -l.
4. Remove execute permission from everyone on server.sh.
5. Add read permission for everyone on .env.
6. Check your current username and groups using whoami, id and groups.
Expected permissions after tasks 1–3:
-rwxr-xr-x server.sh
-rw------- .env


After tasks 4–5, they should become:
-rw-r--r-- server.sh
-rw-r--r-- .env


You can accomplish this using either symbolic or numeric chmod.

Security note: In real applications, don't automatically make .env readable by everyone. We're changing it here only to practice permission management. Normally, keep secrets private.