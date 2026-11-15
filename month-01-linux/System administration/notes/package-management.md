# Package Management

1. What is package management?

A linux package is a collection of files and metadata required to install a software.

A package manager handles installation, updates , dependencies and removal.

2. How does APT work?

APT gets software from repositories. 
A repository is a server containing software packages.

* `sudo apt update` 
The process: 
Connects to configured repositories.
Refreches updated package information.
Refreches the list of available package versions.
* `sudo apt upgrade` 
Upgrades installed packages to newer available versions.


