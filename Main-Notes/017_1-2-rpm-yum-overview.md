# Part 1 of 2: RPM & Yum Package Management

## 102.5: Use RPM and YUM package management

| **Weight**                              | **3**                                                                                                                                                                                                                                                                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Description**                         | Candidates should be able to perform package management using RPM, YUM and Zypper.                                                                                                                                                                                                                                 |
| **Key Knowledge Areas:**                | - Install, re-install, upgrade, and remove packages using RPM, YUM, and Zypper.<br>- Obtain information on RPM packages such as version, status, dependencies, integrity, and signatures.<br>- Determine what files a package provides and find which package a specific file comes from.<br>- Awareness of `dnf`. |
| **Common Files, Terms, and Utilities:** | - `rpm`<br>- `rpm2cpio`<br>- `/etc/yum.conf`<br>- `/etc/yum.repos.d/`<br>- `yum`<br>- `zypper`                                                                                                                                                                                                                     |

---

For some reason, Jadi starts 102.5 (RPM/Yum package management) before 102.4 (Debian package management). I will follow Jadi's order. 

Turns out, earlier LPIC-1 versions (like v4.0, perhaps the one Jadi used) had them swapped, which caused confusion — but the **newer 5.0 objectives** are the current and correct layout: 102.4 is for Debian Package Management, and 102.5 is for RHEL & its clones.

---

## `rpm`, `yum`, `zypper` (and `dnf`)

*Access `yum` and `rpm` cheat-sheet [here](/References/yum-zypper-cheatsheet.md).*

RHEL, Rocky Linux, Fedora, and openSUSE uses `rpm` as its main package manager. However, it is lower-level, and doesn't handle dependencies automatically. 

Tools like `yum`, `zypper`, and `dnf` use repository metadata to automatically resolve and install all required dependencies.

> **RPM vs YUM:**
> 
> - `rpm` = low-level, local package operations.
> - `yum` = high-level, repository-aware package manager (handles dependencies automatically).  
> 
>     Use `rpm` for diagnostics or manual installations, and `yum`/`dnf` for everything else.


## Useful Yum Trick
Using `-y` option will automatically choose 'yes/no' prompts. Useful for quick, deliberate installs.

Kinda like `--noconfirm` that I'm familiar with in Pacman.

---

## Zypper
On **OpenSUSE** / **SUSE Enterprise** systems, Zypper is the native frontend for `.rpm`. The downloaded `.rpm` files are stored in a specified folders. 

For example:
```
/var/cache/zypp/packages/download.opensuse.org-oss/x86_64
```

More `zypper` commands & comparison to `dnf` and `yum` can be found on the [cheatsheet](/References/yum-zypper-cheatsheet.md#-comparison-zypper-vs-dnf--yum).

---

## `dnf`
On newer systems like Rocky Linux 10 that I use, `dnf` is a drop-in replacement for `yum`. 
- To maintain backward compatibility (especially for automation scripts and older documentation),  
- Red Hat and its clones **kept the old `yum` command as a symlink** to `dnf`.
	- The `yum` command is just a **wrapper for `dnf`**, meaning they behave the same, though you’ll get a deprecation notice sometimes.

### 📁 Configuration Files Still Exist
Even with `dnf`, the configuration paths remain **inherited from yum**:

| **Purpose**            | **File / Directory**         | **Notes**                                            |
| ---------------------- | ---------------------------- | ---------------------------------------------------- |
| Main config            | `/etc/yum.conf`              | Still read by `dnf`; backward-compatible format.     |
| Repository definitions | `/etc/yum.repos.d/*.repo`    | Still used; you’ll edit these to manage repos.       |
| Cached metadata        | `/var/cache/dnf/` (used now) | Older `/var/cache/yum/` may still exist on upgrades. |
| Log file               | `/var/log/dnf.log`           | Replaced `yum.log`, but older systems still have it. |

### Interesting Update Fail
One day when I try to update with `sudo dnf upgrade`, I encountered an error followed by a suggestion:
```
(try to add '--allowerasing' to command line to replace conflicting packages or '--skip-broken' to skip uninstallable packages or '--nobest' to use not only best candidate packages)
```
…apparently it means **`dnf` couldn’t find a clean dependency path** to upgrade everything safely.  

That’s where these options come in 👇

#### ⚙️ Options Explained

| **Option**       | **Meaning**                                                                         | **When to Use It**                                                                                                  |
| ---------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `--allowerasing` | Allows DNF to **remove conflicting packages** if they block upgrades.               | Use when an obsolete or conflicting package prevents updates. DNF will remove it and continue.                      |
| `--nobest`       | Instructs DNF **not to insist on the latest version** if dependencies can’t be met. | Useful when a repo is missing dependencies for the latest version. Lets DNF choose an older but compatible package. |
| `--skip-broken`  | Tells DNF to **skip packages that can’t be upgraded** due to broken dependencies.   | Handy to let the upgrade continue while ignoring problematic packages (they remain installed as-is).                |

After including these options, the update went rather smoothly. Let's see the history:
```
# dnf history info last
Transaction ID : 4
Begin time     : Sat 18 Oct 2025 03:47:08 PM +07
Begin rpmdb    : 29a4a4769fd4d28b7b7ae94ff8595962a326374d6b483e211f6bd6ade75cf69f
End time       : Sat 18 Oct 2025 03:48:07 PM +07 (59 seconds)
End rpmdb      : 06b110ad908051e8bbab70d6fb28b0e9af95dc829d08c516d07d9393a12aafa1
User           : Warden <warden>
Return-Code    : Success
Releasever     : 10
Command Line   : upgrade --allowerasing --skip-broken --nobest
Comment        : 
Packages Altered:
    Install  kernel-6.12.0-55.39.1.el10_0.x86_64               @baseos
    Install  kernel-core-6.12.0-55.39.1.el10_0.x86_64          @baseos
    Install  kernel-modules-6.12.0-55.39.1.el10_0.x86_64       @baseos
    Install  kernel-modules-core-6.12.0-55.39.1.el10_0.x86_64  @baseos
    Install  kernel-modules-extra-6.12.0-55.39.1.el10_0.x86_64 @baseos
    Upgrade  kernel-tools-6.12.0-55.39.1.el10_0.x86_64         @baseos
    Upgraded kernel-tools-6.12.0-55.37.1.el10_0.x86_64         @@System
    Upgrade  kernel-tools-libs-6.12.0-55.39.1.el10_0.x86_64    @baseos
    Upgraded kernel-tools-libs-6.12.0-55.37.1.el10_0.x86_64    @@System
    Upgrade  libssh-0.11.1-4.el10_0.x86_64                     @baseos
    Upgraded libssh-0.11.1-1.el10.x86_64                       @@System
    Upgrade  libssh-config-0.11.1-4.el10_0.noarch              @baseos
    Upgraded libssh-config-0.11.1-1.el10.noarch                @@System
    Upgrade  vim-filesystem-2:9.1.083-5.el10_0.1.noarch        @baseos
    Upgraded vim-filesystem-2:9.1.083-5.el10.noarch            @@System

```
#### 📦 Packages Altered

| Type        | Package                                            | Repository | Notes                                                                                                          |
| ----------- | -------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------- |
| **Install** | `kernel-6.12.0-55.39.1.el10_0.x86_64` + submodules | `@baseos`  | New kernel and related modules installed — DNF installs new kernels **instead of replacing** old ones (safer). |
| **Upgrade** | `kernel-tools`, `kernel-tools-libs`                | `@baseos`  | These replaced older versions (`55.37.1 → 55.39.1`).                                                           |
| **Upgrade** | `libssh`, `libssh-config`                          | `@baseos`  | Security or compatibility update (minor build change).                                                         |
| **Upgrade** | `vim-filesystem`                                   | `@baseos`  | Small revision bump (`-5.el10` → `-5.el10_0.1`). Likely a maintenance release.                                 |

#### 🧠 Key Observations
1. ✅ **`--allowerasing` didn’t remove anything this time** — no erased packages shown.  
    (If there were conflicts, they’d appear as “Erase … @System” lines.)
2. ✅ **`--skip-broken` didn’t skip anything either**, since all transactions completed normally.
3. ⚙️ **A new kernel was installed**, so you should now have **two (or more)** kernel versions in `/boot` — DNF keeps older ones automatically.