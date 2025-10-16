## 🧩 **YUM — Most-Used Scenarios**

| **Scenario**                          | **Command**                                                | **What It Does**                                                   |
| ------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ |
| 🔹 Install a new package              | `yum install package`                                      | Installs a package and all required dependencies.                  |
| 🔹 Re-install a package               | `yum reinstall package`                                    | Re-installs the package even if it’s already present.              |
| 🔹 Remove a package                   | `yum remove package`                                       | Uninstalls the package and (optionally) its unneeded dependencies. |
| 🔹 Upgrade a specific package         | `yum update package`                                       | Updates that package to the latest version.                        |
| 🔹 Upgrade all packages               | `yum update`                                               | Updates all installed packages system-wide.                        |
| 🔹 Check for available updates        | `yum check-update`                                         | Lists packages that have newer versions available.                 |
| 🔹 Downgrade a package                | `yum downgrade package`                                    | Reverts to a previous version if available in repos.               |
| 🔹 List all installed packages        | `yum list installed`                                       | Displays all installed packages.                                   |
| 🔹 List available packages            | `yum list available`                                       | Shows packages that can be installed.                              |
| 🔹 Get package info                   | `yum info package`                                         | Displays detailed info about a package.                            |
| 🔹 Find which package provides a file | `yum provides /path/to/file`                               | Finds which package owns or offers that file.                      |
| 🔹 Search by keyword                  | `yum search keyword`                                       | Searches package names and summaries.                              |
| 🔹 Clean cache and metadata           | `yum clean all`                                            | Clears cached data and metadata (useful after repo changes).       |
| 🔹 Show enabled repositories          | `yum repolist enabled`                                     | Lists repositories currently active.                               |
| 🔹 Manage repositories                | `yum-config-manager --add-repo URL`                        | Adds a new repo (from `yum-utils` package).                        |
| 🔹 List packages in a repo            | `yum --disablerepo="*" --enablerepo="base" list available` | Lists only from a specific repo.                                   |
| 🔹 View transaction history           | `yum history`                                              | Shows install/update/remove history.                               |
| 🔹 Undo a transaction                 | `yum history undo <ID>`                                    | Rolls back a previous transaction (found via `yum history`).       |

> 💡 **Tip:** On Rocky 10 and RHEL 9+, every one of these commands works identically if you replace `yum` with `dnf`.

---

## 🧩 **RPM — Low-Level Package Operations**

| **Scenario**                    | **Command**                          | **What It Does**                                                                                            |
| ------------------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| 🔹 Install an RPM manually      | `rpm -ivh package.rpm`               | Installs a local `.rpm` file (no dependency resolution).                                                    |
| 🔹 Upgrade an RPM manually      | `rpm -Uvh package.rpm`               | Installs or upgrades the given RPM.                                                                         |
| 🔹 Freshen only installed pkgs  | `rpm -Fvh package.rpm`               | Updates only if the package is already installed.                                                           |
| 🔹 Remove a package             | `rpm -e package`                     | Erases (removes) a package by name.                                                                         |
| 🔹 Query installed package      | `rpm -q package`                     | Shows if a package is installed and which version.                                                          |
| 🔹 Query by file                | `rpm -qf /path/to/file`              | Shows which package owns that file.                                                                         |
| 🔹 List all installed packages  | `rpm -qa`                            | Lists every installed package.                                                                              |
| 🔹 Display package info         | `rpm -qi package`                    | Gives detailed package info (vendor, version, summary).                                                     |
| 🔹 List package files           | `rpm -ql package`                    | Shows all files installed by that package.                                                                  |
| 🔹 Verify package integrity     | `rpm -V package`                     | Checks if installed files have changed (permissions, hashes, etc.).                                         |
| 🔹 List config files in package | `rpm -qc package`                    | Lists only the configuration files.                                                                         |
| 🔹 Check dependencies           | `rpm -qR package`                    | Lists all required dependencies.                                                                            |
| 🔹 Verify package signature     | `rpm --checksig package.rpm`         | Checks GPG signature and integrity.                                                                         |
| 🔹 Convert to CPIO archive      | `rpm2cpio package.rpm \| cpio -idmv` | Extracting files from an RPM manually — often used to **inspect package contents** without installing them. |

> ⚠️ **RPM vs YUM:**
> 
> - `rpm` = low-level, local package operations.
>     
> - `yum` = high-level, repository-aware package manager (handles dependencies automatically).  
>     Use `rpm` for diagnostics or manual installations, and `yum`/`dnf` for everything else.
>     

---

## 🧠 **Quick LPIC Exam Tips**

- Know that **YUM auto-resolves dependencies**, while **RPM does not**.
- Understand `/etc/yum.conf` and `/etc/yum.repos.d/*.repo` structure.
- Be aware that **DNF = next-gen YUM** (exam mentions “awareness of DNF”).
- Memorize the **RPM query and verify flags** (`-q`, `-qa`, `-ql`, `-qi`, `-qf`, `-V`, `-qR`).