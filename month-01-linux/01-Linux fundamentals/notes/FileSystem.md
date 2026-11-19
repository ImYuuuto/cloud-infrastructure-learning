# Important Linux directories

/ - Root directory.
/home - where normal users store their personal files.
/root - it is the home directory of the administrator account called root.
/etc - it contains system-wide configuration files. ex: SSH, Networking, Nginx.
/var - Stores data that changes while the system runs:
        Important subdirectories: 
        /var/log - System and application logs,
        /var/cache - Cached data,
        /var/lib - Persistent application state,
        /var/tmp - Temporary files.
/usr - contains installed programs, libraries and shared resources.
        Important paths:
        /usr/bin - Many executable programs,
        /usr/lib - Libraries used by programs,
        /usr/local - Software installed locally by the administrator,
        /usr/share - Architecture independent shared data.
/bin - Traditionally contains essential executable commands
    Ex: ls | cp | mv | cat
/tmp - Used by programs to store temporary data.
/dev - Linux represents many hardware devices and virtual devices as files.
/proc - a virtual filesystem that exposes information about running processes and the kernel.
/boot - contains files needed to start Linux
    like : Linux kernel images, initial RAM filessystem, Bootloader-related files.