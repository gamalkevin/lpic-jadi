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

For more details about init systems, go [here](References/init-systems.md).

### Systemd
The new, flashy guy. Lots of debate. 

It is monolithic, meaning it behaves more like a corporation with united vision rather than a group of independent volunteers.

It is super complicated (supports hierarchy, timers, etc.), and for many people, that is the very argument against using it.

`systemd` works by having [units](References/init-systems.md#systemd-units). To work with units, we use `systemctl` (systemd control?).

Let's try viewing my default mode:
```
$ systemctl get-default
graphical.target
[kev@Randrake system]$ systemctl cat graphical.target
# /usr/lib/systemd/system/graphical.target
#  SPDX-License-Identifier: LGPL-2.1-or-later
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Graphical Interface
Documentation=man:systemd.special(7)
Requires=multi-user.target
Wants=display-manager.service
Conflicts=rescue.service rescue.target
After=multi-user.target rescue.service rescue.target display-manager.service
AllowIsolate=yes

```

### SysVinit

