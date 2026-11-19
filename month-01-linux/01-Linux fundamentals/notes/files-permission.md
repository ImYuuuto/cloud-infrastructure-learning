# Linux File Permissions

1. Understanding Linux permissions
Every file and directory in Linux has:
- An owner (user).
- An associated group.
- Permissions defining what users can do.

- example : -rwxr-xr-- 1 yuuto developers 1024 Nov 12 server.sh

    The first character represents the file type:
    Character	Meaning
    -	Regular file
    d	Directory
    l	Symbolic link

    The next nine characters are permissions divided into three groups:
    Position	Applies to
    First 3	    Owner
    Middle 3	Group
    Last 3	    Others

2. The three permission types
There are three fundamental permissions.
Permission	Symbol	     Meaning
- Read	r	Read file    contents
- Write	w	Modify file  contents
- Execute	x Execute    a file

3. Understanding chmod
chmod means change mode.

It changes file and directory permissions.

There are two ways to use it:
1. Symbolic mode.
2. Numeric (octal) mode.

A. Symbolic mode

You use letters to specify the user and the permission.
Symbol	Meaning
u	    Owner (user)
g	    Group
o	    Others
a	    All

Operators:
Operator	Meaning
+	        Add permission
-	        Remove permission
=	        Set exact permissions

B. Numeric permissions (octal)

This is another way to use chmod, and you'll see it frequently in Linux tutorials and server configuration.

Each permission has a numeric value:
Permission	Value
Read (r)	4
Write (w)	2
Execute (x)	1

You add the values together.

4. Understanding chown and chgrp
So far, we've changed permissions. But what if we want to change who owns a file?

That's where chown comes in.
chown — Change ownership
Check the current ownership:
ls -l server.sh

Example:
-rwxr-xr-- 1 yuuto developers 1024 server.sh

Change the owner:
sudo chown root server.sh

chgrp — Change group ownership

5. Understanding sudo
You've already encountered sudo when you tried moving a directory into a protected location.

sudo means superuser do.

It allows an authorized user to execute a command with elevated privileges.
