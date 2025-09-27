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

## Systemd
`systemd` works by having **units**. 