# 008 - 101.2 - Part 2/2 - Boot Process; logs (dmesg, logs, journalctl) & init (systemd&SysV) 📝
We continue with last part of 101.2.

## `dmesg` 📜
One great things about Linux system is that everything is extensively logged. So in case something goes wrong, we can go see the log and examine the problems.

Upon boot, all kernel messages (*hardware, drivers, boot events, etc.*) are stored in **kernel ring buffer**; think of it like a whiteboard where the kernel stores messages. It is **limited to current buffer** like a ring, so after a while it will get full, and new data/message will overwrite old ones.  

`dmesg` means **display/diagnostic message**; it prints all the messages stored in the kernel ring buffer. It is the kernel's built-it log viewer.
- It also stores information about other devices, like USB. 
	- That's the reason why `dmesg` is used to check whether USB/other peripherals are connected & detected by the system or not.

### `dmesg` vs `journalctl -k`
- `dmesg` shows only what is in the **current kernel ring buffer**, and older messages eventually get overwritten.
- `journalctl -k` (from `systemd`) shows **persistent kernel logs**, saved across reboots, if the system uses `systemd-journald`.

## `init` 🌄
After the kernel finishes loading, the system needs to start other programs. That’s where `init` comes in—it’s the first process that launches and manages system programs (*like webserver, ntp/network time protocol, ssh, etc.*) to get the system fully running. 

Some `init` system examples:
- **SysVinit**, based on Unix System V. The old guard. No longer used widely due to it having limited capabilities.
- **systemd**, the current, widely used init. Can start services in parallel; many useful and fancy features.
- **runit** a fast, simple init system that can start services in parallel. 
- **upstart** event-based, initiated by Canonical, the company behind Ubuntu. Started in 2014, discontinued in 2015. 

### Systemd
The new, flashy guy. Lots of debate. 

It is monolithic, meaning it behaves more like a corporation with united vision rather than a group of independent volunteers.

`systemd` works by having **units**. 

#### What it is
- **System and Service Manager** for Linux.
- It’s the **first process** the kernel starts after boot (`PID 1`).
- Its job: **bring the system from “just kernel” → to a usable OS**.

#### What it does
1. **Init replacement**
    - In older Linux, `init` (SysVinit) handled startup.
    - Today, `systemd` is the modern replacement.
2. **Service management**
    - Starts, stops, restarts background services (daemons).
    - Example: `systemctl start sshd`
3. **Parallel booting**
    - Old SysVinit started services one by one (slow).
    - Systemd starts services **in parallel** if possible → faster boot.
4. **Unit files**
    - Instead of messy shell scripts, systemd uses **unit files** to define services, targets, sockets, etc.
    - Example: `/etc/systemd/system/sshd.service`
5. **Other tools** (all under systemd umbrella):
    - `journald` → logging system
    - `logind` → user sessions and power management
    - `timedated`, `networkd`, etc. → modular helpers

#### Basic commands (LPIC must-know)

```
systemctl status sshd       # check if SSH is running
systemctl start sshd        # start SSH service
systemctl stop sshd         # stop it
systemctl enable sshd       # start on boot
systemctl disable sshd      # don’t start on boot
```

#### ✅ LPIC-friendly summary

- **Systemd = modern init system**
- Manages: **boot sequence + services + logging + user sessions**
- Uses **unit files** instead of old SysVinit scripts
- Controlled with **`systemctl`**

### Note
