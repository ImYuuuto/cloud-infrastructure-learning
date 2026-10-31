# Explore Linux filesystem

Open your Ubuntu VM terminal and execute these commands one by one.

## Exercise A — Explore directories
1. pwd => /home/yuuto
2. ls / => bin boot cdrom dev etc home lib lib64 lost+found media mnt opt proc 
        root run sbin snap srv swap.img sys tmp usr var
3. ls /home => yuuto
4. ls /etc => a lot of files 
5. ls /var => backups cache crash lib local lock log mail opt run snap spool tmp

What these commands do:
- pwd — Displays your current directory.
- ls — Lists directory contents.

## Exercise B — Explore system information
1. cat /etc/hostname ubuntu-lab
2. cat /proc/cpuinfo
3. cat /proc/meminfo