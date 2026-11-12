9. Practical lab — Simulate a development team
Now let's simulate a real Linux server used by a development team.

Your objective is to create two developers and give them access to a shared project directory.
Target structure
 Ubuntu Server

 developers group

 developer1

 developer2

 /opt/team-project

 app.js


Both developers should be able to access the shared project directory.
Part A — Create users and groups
1. Create a group called developers.
2. Create users developer1 and developer2.
3. Set their default shell to Bash and create their home directories.
4. Assign both users to the developers group.
5. Give both users passwords.
Part B — Verify everything
Use:
id developer1
id developer2
getent group developers

Check that both users belong to developers.
Part C — Create a shared directory
Create:
/opt/team-project/
└── app.js

Then:
1. Change the directory's group ownership to developers.
2. Give the owner and group full access, but no access to others.
3. Configure the directory so newly created files inherit its group.
Hint: The last requirement involves a special directory permission called the setgid bit. We'll explain it in more depth later, but research chmod g+s.

Useful commands to investigate:
sudo mkdir -p /opt/team-project
sudo chown
sudo chmod
ls -ld /opt/team-project

Don't use chmod 777.
Part D — Cleanup
Once you've verified everything:
1. Remove developer1.
2. Remove developer2.
3. Remove the developers group.
4. Remove the test directory.
Be careful not to delete your own account or home directory.