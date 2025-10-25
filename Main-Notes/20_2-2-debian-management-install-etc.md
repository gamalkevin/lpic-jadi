# Part 2 of 2: Debian Package Management 

In previous video, I noted the requirement of LPIC being `apt-get` instead of the modern `apt`. How different are they?

## 🧩 Practical Difference

| Task                | `apt-get`                    | `apt`                    |
| ------------------- | ---------------------------- | ------------------------ |
| Install a package   | `sudo apt-get install nginx` | `sudo apt install nginx` |
| Remove a package    | `sudo apt-get remove nginx`  | `sudo apt remove nginx`  |
| Update package list | `sudo apt-get update`        | `sudo apt update`        |
| Upgrade packages    | `sudo apt-get upgrade`       | `sudo apt upgrade`       |
| Show info           | `apt-cache show nginx`       | `apt show nginx`         |
| Search package      | `apt-cache search nginx`     | `apt search nginx`       |
| Full-upgrade        | `sudo apt-get dist-upgrade`  | `sudo apt full-upgrade`  |

The `apt` command combines both `apt-get` and `apt-cache` features, while improving output readability.

### 💡 Things to note
- ✅ **Exam:** Focus on `dpkg`, `apt-get`, and `apt-cache`. Know that `apt` exists.
- 💻 **Real life:** Use `apt` for convenience (it’s cleaner and safer interactively).
- ⚙️ **Scripts:** Keep using `apt-get` — it’s stable and won’t change behavior unexpectedly.

## 🏋️‍♀️ Real-world Usage

### ⤵️ `--download-only`
- **Purpose:** Pre-download `.deb` packages (and dependencies) _without installing them_ — great for minimizing downtime, or for systems with limited connectivity.
    
- **Commands:**
    ```
    sudo apt-get install --download-only nginx
    ```
    The downloaded `.deb` files are stored in:
    ```
    /var/cache/apt/archives/
    ```
- **Universal equivalent:**
    - `yum` → `--downloadonly` (plugin)        
    - `dnf` → `download` subcommand
    - `pacman` → `-w` or `--downloadonly`        
    - `zypper` → `--download-only`

What's the use? So perhaps a system is a webserver, and sysadmin can only afford literal minutes of downtime to upgrade the system. By using `--download-only` options, sysadmin can predownload the packages before the planned down time. 

This will save time during the upgrade; time will only used to upgrade the packages instead of downloading from scratch. 


### 👟 `-s` 
This is the **“dry run”** mode, showing what would happen without actually making changes.

Example:
```
sudo apt-get install -s nginx
```
- Shows what packages will be installed, upgraded, or removed.
- Great for predicting dependency impact and avoiding unwanted system-wide upgrades.
- Works also with `remove`, `upgrade`, etc.

Equivalent in other systems:
- `dnf` / `yum`: `--assumeno`
- `zypper`: `--dry-run`
- `pacman`: `-p`

### ⬆️ Update/upgrade behavior 🔁
On systems with `apt-get` / `apt`, the upgrading process involves two steps: 
1. Updating the **index** of the repositories first
	1. First, run `sudo apt-get update`
	2. This updates the _package index_ (metadata) from all repositories defined in `/etc/apt/sources.list` and `.d/`.
	3. Does **not** upgrade or install anything — just refreshes the local cache.
2. Upgrading the actual packages. 
	1. After the above steps, we can run `sudo apt-get upgrade`
	2. This will compare the installed packages on the system with the new 'database' that was just downloaded. 
	3. This installs the **new versions** of packages currently installed, however,
	4. Will never remove or install _new_ packages; only upgrades in-place.
	5. A more aggressive measure would be - `sudo apt-get dist-upgrade` (or `sudo apt full-upgrade`)
		1. This allows installing/removing packages to satisfy dependencies. Typically used during major version upgrades.
3. To see upgradeable packages without actually upgrading them, run `apt list upgradeable`. 
	1. `apt-get` doesn’t have a direct equivalent; it’s a feature of the newer `apt` front-end.