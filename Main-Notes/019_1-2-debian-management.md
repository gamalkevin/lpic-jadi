# Part 1 of 2: Debian Package Management 

## 102.4: Use Debian package management

| **Weight**              | **3**                                                                                                                                                                                                                                                                       |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Description**         | Candidates should be able to perform package management using the Debian package tools.                                                                                                                                                                                     |
| **Key Knowledge Areas** | - Install, upgrade, and uninstall Debian binary packages.  <br>- Find packages containing specific files or libraries (installed or not).  <br>- Obtain package info such as version, content, dependencies, integrity, and installation status.  <br>- Awareness of `apt`. |
| **Terms and Utilities** | - `/etc/apt/sources.list` <br>- `dpkg`<br>- `dpkg-reconfigure`<br>- `apt-get`<br>- `apt-cache`                                                                                                                                                                              |

---

# Debian Package Manager
Debian systems use the **dpkg** tool as the low-level package manager.  

`apt-get`, `apt-cache`, and `apt` sit on top of it to **resolve dependencies**, **fetch from repos**, and **automate installs**.

If we were to compare it to previous chapter, `dpkg` is the Debian equivalent to RHEL's `rpm`, while `apt-get` / `apt` are to `yum` / `dnf`.

## Checking `sources.list`
**Modern Ubuntu (22.04+, and Debian 12+)** has moved from the old `sources.list` format to the **new `.sources` YAML-style format** under `/etc/apt/sources.list.d/`. 

The old `sources.list` being listed on the overview is no longer in-use. Checking it will yield the following:
```
# cat /etc/apt/sources.list
# Ubuntu sources have moved to /etc/apt/sources.list.d/ubuntu.sources
```

Sure enough, after adjusting to the new location, it shows the `sources` just fine:
```
# cat /etc/apt/sources.list.d/ubuntu.sources
Types: deb
URIs: http://th.archive.ubuntu.com/ubuntu/
Suites: questing questing-updates questing-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: questing-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

## 🧭 Current Commands

| Command                               | What It Does                                                     |
| ------------------------------------- | ---------------------------------------------------------------- |
| `sudo apt update`                     | Refreshes your package list (checks what’s available in repos)   |
| `sudo apt install package-name`       | Installs a package from your configured repos                    |
| `sudo apt upgrade`                    | Upgrades installed packages to newer versions available in repos |
| `sudo add-apt-repository ppa:xyz/ppa` | Adds a new repo (like a third-party app store)                   |

Notice that this table lists `apt` instead of `apt-get`. We should still be aware that LPI keeps its objectives **cross-compatible across older Debian releases** and other distributions (like Devuan) where `apt-get` is still primary.

Scripts still utilize `apt-get` as standard too.