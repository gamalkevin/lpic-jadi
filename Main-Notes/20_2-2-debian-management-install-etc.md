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

## Real-world Usage

### `--download-only`
I believe such option is universal; `rpm` and `pacman` also have such option. 

What's the use? So perhaps a system is a webserver, and sysadmin can only afford literal minutes of downtime to upgrade the system. By using `--download-only` options, sysadmin can predownload the packages before the planned down time. It will download the `.deb` file of the package.

This will save time during the upgrade; time will only used to upgrade the packages instead of downloading from scratch. 

### `-s` 
This option is to **simulate an install**. 

Sometimes we want to see what deps are being installed with the package, or whether the package will trigger another script or not, etc.

### Update/upgrade behavior
On systems with `apt-get` / `apt`, the upgrading process involves two steps: 
1. Updating the **cache** of the repositories first
	1. First, run `sudo apt-get update`
	2. This will download the cache from the repositories about all the packages installed on the system
2. Upgrading the actual packages. 
	1. After the above steps, we can run `sudo apt-get upgrade`
	2. This will compare the installed packages on the system with the new 'database' that was just downloaded. If it finds packages that are upgradeable, it will upgrade it. 
	3. To see upgradeable packages without actually upgrading them, run `apt list upgradeable`. (not sure if there's an `apt-get` equivalent)