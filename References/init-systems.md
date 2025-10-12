# Init systems

## Systemd
- **System and Service Manager** for Linux.
- It’s the **first process** the kernel starts after boot (`PID 1`).
- Its job: **bring the system from “just kernel” → to a usable OS**.

### What it does
1. **Init replacement**
    - In older Linux, `init` (SysVinit) handled startup.
    - Today, `systemd` is the modern replacement.
    - Cheat sheets below.
2. **Service management**
    - Starts, stops, restarts background services (daemons).
    - Example: `systemctl start sshd`
3. **Parallel booting**
    - Old SysVinit started services one by one (slow).
    - Systemd starts services **in parallel** if possible → faster boot.
4. **Unit files**
    - Instead of messy shell scripts, systemd uses **unit files** to define services, targets, sockets, etc.
    - Example: `/etc/systemd/system/sshd.service`
    - Complete list of the units below.
5. **Other tools** (all under systemd umbrella):
    - `journald` → logging system
    - `logind` → user sessions and power management
    - `timedated`, `networkd`, etc. → modular helpers

### `systemctl` cheatsheet

#### **1️⃣ Service management**
| Command                    | Description                                                       |
| -------------------------- | ----------------------------------------------------------------- |
| `systemctl start <unit>`   | Start a service immediately.                                      |
| `systemctl stop <unit>`    | Stop a service immediately.                                       |
| `systemctl restart <unit>` | Stop and start a service (reload config if needed).               |
| `systemctl reload <unit>`  | Ask the service to reload config without stopping (if supported). |
| `systemctl status <unit>`  | Show current status, active/inactive, logs.                       |

#### **2️⃣ Enable/disable services at boot**
| Command                       | Description                          |
| ----------------------------- | ------------------------------------ |
| `systemctl enable <unit>`     | Start automatically at boot.         |
| `systemctl disable <unit>`    | Do not start at boot.                |
| `systemctl is-enabled <unit>` | Check if a unit is enabled/disabled. |


#### **3️⃣ Check units / targets**
| Command                               | Description                                               |
| ------------------------------------- | --------------------------------------------------------- |
| `systemctl list-units`                | List **active/running** units.                            |
| `systemctl list-unit-files`           | List **all unit files** and their enabled/disabled state. |
| `systemctl list-units --type=service` | List running services only.                               |
| `systemctl list-units --type=target`  | List active targets.                                      |

#### **4️⃣ Targets / runlevels**
| Command                          | Description                                                 |
| -------------------------------- | ----------------------------------------------------------- |
| `systemctl get-default`          | Show the default target (runlevel).                         |
| `systemctl set-default <target>` | Set the default target for boot (e.g., `graphical.target`). |
| `systemctl isolate <target>`     | Switch to a target immediately (like changing runlevel).    |

#### **5️⃣ Systemd internals**
| Command                   | Description                                       |
| ------------------------- | ------------------------------------------------- |
| `systemctl daemon-reload` | Re-read all unit files (after editing or adding). |
| `systemctl mask <unit>`   | Block a unit from starting at all.                |
| `systemctl unmask <unit>` | Remove the block.                                 |
| `systemctl show <unit>`   | Show detailed info about a unit.                  |

### ✅ LPIC Takeaway

- **Start/Stop/Restart/Reload** → control running services
- **Enable/Disable** → control boot behavior
- **List units** → see what’s running or available
- **Daemon-reload** → after editing unit files
- **Targets** → modern replacement for runlevels

### ✅ LPIC-friendly summary
- **Systemd = modern init system**
- Manages: **boot sequence + services + logging + user sessions**
- Uses **unit files** instead of old SysVinit scripts
- Controlled with **`systemctl`**

### Systemd Units
| **Unit Type** | **Extension** | **What it controls**                                                                 |
| ------------- | ------------- | ------------------------------------------------------------------------------------ |
| **Service**   | `.service`    | A background program (e.g. `sshd.service`, `nginx.service`)                          |
| **Target**    | `.target`     | A group of units (like old runlevels) — e.g. `multi-user.target`, `graphical.target` |
| **Device**    | `.device`     | Hardware devices recognized by the kernel (via udev/sysfs)                           |
| **Mount**     | `.mount`      | A filesystem mount point (e.g. `/home`, `/mnt/usb`)                                  |
| **Automount** | `.automount`  | Auto-mounted filesystem (mount on first access)                                      |
| **Socket**    | `.socket`     | Communication sockets (used to start services on demand)                             |
| **Timer**     | `.timer`      | Like cron jobs, scheduled events (e.g. `logrotate.timer`)                            |
| **Path**      | `.path`       | Watches files/directories and triggers services when they change                     |
| **Slice**     | `.slice`      | Resource control (CPU, memory) via cgroups                                           |
| **Scope**     | `.scope`      | External processes not started by systemd (e.g. user sessions)                       |

## Systemd vs SysVinit Cheat Sheet

| Feature / Concept      | **SysVinit** (old)                     | **Systemd** (modern)                                                     |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------------------ |
| **Role**               | Traditional init system                | Modern init & service manager                                            |
| **Config location**    | `/etc/inittab`, `/etc/rc*.d/` scripts  | Unit files in `/etc/systemd/system/`                                     |
| **Service definition** | Shell scripts (`/etc/init.d/`)         | Declarative unit files (`.service`)                                      |
| **Boot process**       | Sequential (one after another)         | Parallel (faster boot)                                                   |
| **Dependencies**       | Basic runlevels, no explicit deps      | Units with defined dependencies                                          |
| **Runlevels/Targets**  | Numeric runlevels (0–6)                | Targets (e.g. `multi-user.target`)                                       |
| **Service control**    | `service <name> start/stop/status`     | `systemctl start/stop/status <name>`                                     |
| **Enable at boot**     | `chkconfig` or symlinks in `rc.d` dirs | `systemctl enable <service>`                                             |
| **Logging**            | Syslog only                            | Integrated `journald` (plus syslog)                                      |
| **Extensibility**      | Limited                                | Includes timers, sockets, cgroups, etc.                                  |
| **Adoption**           | Legacy (still on some minimal distros) | Default on most major distros (Debian, Ubuntu, Fedora, RHEL, Arch, etc.) |

### Notes on Runlevels/Targets
Think of **runlevels** as **modes of operation** for Linux. Kinda like **Windows power/user modes** (Safe Mode, Normal Mode, etc.), but a bit broader. They’re not about priority or hierarchy — they’re about _what kind of system state_ Linux should boot into.

| **Runlevel** | **Meaning (SysV)**                                                          | **systemd Target Equivalent**     |
| ------------ | --------------------------------------------------------------------------- | --------------------------------- |
| 0            | Halt / Shutdown                                                             | `poweroff.target`                 |
| 1            | Single-user mode (rescue/maintenance mode, no networking, just root access) | `rescue.target`                   |
| 2            | Multi-user (no network, rarely used)                                        | `multi-user.target` (Debian-like) |
| 3            | Multi-user with networking (text/console mode)                              | `multi-user.target`               |
| 4            | Undefined / Can be customized                                               | `multi-user.target` (or custom)   |
| 5            | Multi-user with GUI (graphical login)                                       | `graphical.target`                |
| 6            | Reboot                                                                      | `reboot.target`                   |

So:
- They’re not _priorities_.
- They’re not _hierarchies_.
- They’re simply **system states**, telling Linux what services to start and what kind of environment to give the user.

👉 For example:  
If you boot into **runlevel 3**, you’ll get a terminal login (no desktop).  
If you boot into **runlevel 5**, you’ll get the graphical login screen.