# Practical lab — Backend log investigation
Let's simulate a real situation you'll encounter as a backend developer.

Create a directory:
mkdir -p ~/linux-practice/logs
cd ~/linux-practice/logs

Create a log file:
nano server.log

Put this content inside:
[INFO] Server started on port 4000
[INFO] Database connected
[INFO] User authenticated
[WARNING] High memory usage
[ERROR] Failed to connect to database
[INFO] Request GET /api/tasks
[ERROR] Internal server error
[WARNING] Slow database query
[INFO] Server running normally
[ERROR] Authentication failed

Save and exit.
Your missions

1. Display the complete log file using cat.
2. Display only the first 3 lines.
3. Display only the last 4 lines.
4. Search for all errors.
5. Search for warnings without worrying about uppercase/lowercase.
6. Display errors with their line numbers.
7. Count the total number of lines.
8. Count how many lines contain ERROR.

## Correction
1. cat server.log
2. head -n 3 server.log
3. tail -n 4 server.log
4. grep -i "error" server.log
5. grep -i "warning" server.log
6. cat -n server.log | grep -i "error" 
7. wc -l
8. cat server.log | grep -i "error" | wc -l