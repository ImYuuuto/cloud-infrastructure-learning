# Linux Users & Groups Management

1. Understanding Linux users
Linux is a multi-user operating system.

Linux Server
│
├── root
│   └── Administrator
│
├── yuuto
│   └── Normal user
│
├── developer1
│   └── Normal user
│
└── developer2
    └── Normal user

2. Understanding Linux groups
Groups allow administrators to manage permissions for multiple users at once.

Imagine you have a development team:
Group: developers
│
├── yuuto
├── developer1
└── developer2

Instead of giving permissions individually to each developer, you can assign permissions to the developers group.

4. Creating users
Now let's start using actual commands.
useradd
Creates a new user.
- sudo useradd developer1

However, this command doesn't automatically create a home directory unless configured to do so.

A more practical command:
- sudo useradd -m -s /bin/bash developer1

Understanding the options:
Option	Meaning
-m	Create a home directory
-s	Specify the login shell
-d	Specify the home directory

The resulting home directory will normally be:
/home/developer1

Set a password
- sudo passwd developer1

You'll be prompted to enter and confirm a password.
Verify the user
id developer1

And:
- ls -ld /home/developer1

Alternative: adduser
Ubuntu also provides:
- sudo adduser developer1

Unlike the lower-level useradd, adduser is an interactive utility that guides you through creating the account and setting its password.

5. Creating groups
groupadd
Creates a new group.
- sudo groupadd developers

Verify:
- getent group developers

6. Adding users to groups
This is where usermod becomes important.
usermod
Modifies an existing user account.

Add a user to a supplementary group:
- sudo usermod -aG developers developer1

Let's break it down:
- usermod: Modify a user.
- -a: Append to existing supplementary groups.
- -G: Specify supplementary groups.
- developers: Target group.
- developer1: Target user.
## Important
Never forget -a when you intend to add a supplementary group without replacing existing memberships.

This replaces the user's supplementary group list with the specified list. It may remove memberships they previously had.
Verify group membership
- groups developer1
Or:
- id developer1

Adding a user to sudo
On Ubuntu, the administrative group is called sudo.
- sudo usermod -aG sudo developer1

This grants the user access to sudo, subject to the system's sudo configuration.

7. Removing users and groups
userdel
Deletes a user account.
sudo userdel developer1

This removes the account but normally leaves its home directory.

To also remove the home directory and associated user files:
sudo userdel -r developer1

Be careful with -r: make sure you don't need the user's data.
groupdel
Deletes a group.
sudo groupdel developers

A group generally cannot be deleted if it is the primary group of an existing user.
gpasswd
Another useful group management command.

Remove a user from a supplementary group:
sudo gpasswd -d developer1 developers