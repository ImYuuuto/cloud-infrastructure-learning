## 5. Practical Exercises

Open your Ubuntu environment (or WSL terminal) and work through the following lab exercises to master package management workflows using `apt`.

---

### Exercise 1 — Package Information & Queries
*Objective: Learn how to query the local package database without modifying system state.*

1. **Refresh package lists:** Synchronize your local package index with remote repositories to ensure you have the latest software versions.
2. **Search for Nginx:** Query the repository cache to find available packages matching `nginx`.
3. **Inspect package metadata:** Display detailed information about `nginx` (maintainer, size, description, dependencies) without installing it.
4. **Check binary availability:** Verify whether `curl` is currently available in your PATH or installed on the system.
5. **Filter installed packages:** List all currently installed packages on your system and filter the output specifically for `git`.

> **Key Commands Reference:**
> * `sudo apt update`
> * `apt search <package>`
> * `apt show <package>`
> * `which curl` or `dpkg -l | grep curl`
> * `dpkg -l | grep git`

---

### Exercise 2 — Software Lifecycle Management
*Objective: Practice installing, executing, verifying, and removing a lightweight utility package.*

1. **Install `tree`:** Install the directory-listing utility `tree` using root privileges.
2. **Test execution:** Create a dummy directory structure (`mkdir -p ~/linux-practice`) and run `tree ~/linux-practice` to observe its output tree layout.
3. **Check installed version:** Query the binary version to confirm successful installation.
4. **Remove the package:** Uninstall `tree` from the system. (Bonus: Try `apt remove` vs. `apt purge` to see the difference regarding configuration files).
5. **Verify uninstallation:** Confirm that the binary is no longer accessible or installed.

> **Key Commands Reference:**
> * `sudo apt install tree`
> * `tree --version`
> * `sudo apt remove tree`
> * `which tree`

---

### Exercise 3 — Dependency Resolution
*Objective: Understand how package managers automatically handle software dependencies and libraries.*

1. **Select a target package:** Choose a utility you don't currently have installed (e.g., `htop` or `neofetch`).
2. **Inspect package metadata:** Use `apt show` to examine its control file fields.
3. **Examine dependencies:** Use `apt-cache depends <package>` to view its dependency tree (Depends, Pre-Depends, Recommends).
4. **Analyze design rationale:** Write a short personal note explaining *why* modern package managers (like APT) handle dependency trees automatically instead of requiring manual library downloads.

> **Key Commands Reference:**
> * `apt-cache depends <package>`

---

### Exercise 4 — Capstone Challenge: Node.js Server Provisioning
*Scenario:* Imagine you are provisioning a fresh Ubuntu cloud server to host a production Node.js backend. 

Your environment requires the following baseline toolchain:
* **`git`** (for repository cloning and source code management)
* **`curl`** (for HTTP requests and fetching remote installation scripts)
* **`htop`** (for real-time process and resource monitoring)
* **`nginx`** (for reverse proxying traffic to your Node.js application)

#### Your Task:
Construct a clean, sequential deployment script (or set of commands) that accomplishes the following phases:
1. **System Preparation:** Update local package repositories and upgrade existing packages to ensure security patches are applied.
2. **Batch Installation:** Install all four required tools in a single, non-interactive execution command.
3. **Verification Workflow:** Write specific verification commands for each tool to confirm proper installation and active status before deploying your application code.