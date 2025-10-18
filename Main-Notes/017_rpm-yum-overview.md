# Overview of RPM and YUM/Zypper/DNF

## 102.5: Use RPM and YUM package management

| **Weight**      | **3**                                                                              |
| --------------- | ---------------------------------------------------------------------------------- |
| **Description** | Candidates should be able to perform package management using RPM, YUM and Zypper. |

**Key Knowledge Areas:**
- Install, re-install, upgrade, and remove packages using RPM, YUM, and Zypper.
- Obtain information on RPM packages such as version, status, dependencies, integrity, and signatures.
- Determine what files a package provides and find which package a specific file comes from.
- Awareness of `dnf`.

**Common Files, Terms, and Utilities:**

- `rpm`
- `rpm2cpio`
- `/etc/yum.conf`
- `/etc/yum.repos.d/`
- `yum`
- `zypper`

---

For some reason, Jadi starts 102.5 (RPM/Yum package management) before 102.4 (Debian package management)

---
## `rpm`, `yum`, `zypper` (and `dnf`)

*Access `yum` and `rpm` cheat-sheet [here](yum-zypper-cheatsheet.md).*

RHEL, Rocky Linux, Fedora, and openSUSE uses `rpm` as its package manager. `rpm` is lower-level, and doesn't handle dependencies automatically. 

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



## Zypper
On **OpenSUSE** / **SUSE Enterprise** systems, Zypper is the native frontend for `.rpm`. The downloaded `.rpm` files are stored in a specified folders. 

For example:
```
/var/cache/zypp/packages/download.opensuse.org-oss/x86_64
```

More `zypper` commands can be found on the cheatsheet.