1. Navigation commands

cd ~ : "to access home directory",
pwd : "Displays your current directory",
ls : "Lists directory contents",
cd .. : "Moves one directory backward (to the parent)",
cd ~ : "Returns to your home directory",
cd - : "Returns to your previous working directory",
ls -l : "Detailed list",
ls -a : "Include hidden files",
ls -ls : "Detailed list including hidden files",
ls -lh : "Human-readable file sizes",
ls -R :	"Recursive listing",

2. Creating files and directories

mkdir <projects> : "Creates a directory",
mkdir -p </home/projects>: "The -p option creates missing parent directories automatically",
touch <index.js> : "Creates an empty file if it doesn't exist",
touch index.js server.js package.json: "Create multiple files",

3. Copying files and directories

cp index.js backup.js: "Creates a copy called backup.js",
cp index.js backup/: "Copy file to another directory",
cp -r project project-backup: "copy a directory",
cp -i index.js backup.js: "i is to ovewrite an existing destination",

4. Moving and renaming

mv index.js scripts/ : "Move file",
mv project archive/ : "Move directory",
mv <old name> <new name>: "Rename file (same applies for directories)",

5. Deleting files and directories

rm index.js : "Delete file",
rmdir old-folder : "Delete directory",
rm -r old-folder : "Delete directory containing files",
rm -rf old-folder : "Force deletion"

6. Finding files and directories

find . -name "server.js" : "find server.js inside the current directory",
find . -name ".js" : "find all js files",
find . -type d : "find directories",
find . -type f : "find files only",
find . -type f -size +10M : "files larger than 10mb"

7. Wildcards

* : "Matches zero or more characters",
? : "Matches exactly one character",
[abc] : "Matches one character from the set"

8. Files manipulation

cat notes.txt : "display file content",
cat -n notes.txt : "with line numbers",
cat > notes.txt : "write into a file",
cat >> notes.txt : "append into a file",

less notes.txt : "for large files",
Space : "Next page",
b : "Previous page",
Arrow Down : "Move down",
Arrow Up : "Move up",
/error : "Search for "error"",
n :	"Next search result",
N : "Previous search result",
q : "Quit"

head notes.txt : "displays 10 first lines",
head -n 5 notes.txt : "display 5",
tail notes.txt : "last 10 lines",
tail -f /var/log/syslog : "continuously displays new lines added to the file",

grep "error" server.log : "Search in a file",
grep -i "error" server.log : "Case insensitive",
grep -r "TODO" . : "Search recusively inside directories",
grep -E "error|warning" server.log  : "seach for multiple terms",
-v : "Show lines that DON'T match",
-c : "Count matching lines".

wc notes.txt : "return nb lines, words, bytes"
-l : "count lines",
-w : "count words",
-c : "count bytes",