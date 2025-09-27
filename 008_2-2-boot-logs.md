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

$ systemctl cat graphical.target
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
The way the unit is described reminds me of how `pacui` lists packages: Note how the `Requires` and `Wants` of systemd has the same spirit with pacman `must-have` and `optional dependency`. I know it's definitely the other way around, but I learned it that way.

Now let's try viewing a service status:
```
$ systemctl status NetworkManager --no-pager -l
● NetworkManager.service - Network Manager
     Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; preset: disabled)
     Active: active (running) since Sat 2025-09-27 09:13:11 +07; 3h 41min ago
 Invocation: 21b3bf43876e4b50bfe06a7fd3961655
       Docs: man:NetworkManager(8)
   Main PID: 576 (NetworkManager)
      Tasks: 4 (limit: 22563)
     Memory: 22.4M (peak: 23.5M)
        CPU: 8.924s
     CGroup: /system.slice/NetworkManager.service
             └─576 /usr/bin/NetworkManager --no-daemon

Sep 27 12:31:48 Randrake NetworkManager[576]: <info>  [1758951108.5453] device (wlp3s0f3u2): supplicant interface state: interface_disabled -> inactive
Sep 27 12:38:41 Randrake NetworkManager[576]: <info>  [1758951521.5337] device (wlp3s0f3u2): set-hw-addr: set MAC address to 16:98:97:CC:78:5B (scanning)
Sep 27 12:38:41 Randrake NetworkManager[576]: <info>  [1758951521.5525] device (wlp3s0f3u2): supplicant interface state: inactive -> interface_disabled
Sep 27 12:38:41 Randrake NetworkManager[576]: <info>  [1758951521.5531] device (wlp3s0f3u2): supplicant interface state: interface_disabled -> inactive
Sep 27 12:45:34 Randrake NetworkManager[576]: <info>  [1758951934.5331] device (wlp3s0f3u2): set-hw-addr: set MAC address to 9A:EF:AB:F8:8C:12 (scanning)
Sep 27 12:45:34 Randrake NetworkManager[576]: <info>  [1758951934.5520] device (wlp3s0f3u2): supplicant interface state: inactive -> interface_disabled
Sep 27 12:45:34 Randrake NetworkManager[576]: <info>  [1758951934.5527] device (wlp3s0f3u2): supplicant interface state: interface_disabled -> inactive
Sep 27 12:52:27 Randrake NetworkManager[576]: <info>  [1758952347.5281] device (wlp3s0f3u2): set-hw-addr: set MAC address to 7A:BF:EB:C5:2B:CA (scanning)
Sep 27 12:52:27 Randrake NetworkManager[576]: <info>  [1758952347.5469] device (wlp3s0f3u2): supplicant interface state: inactive -> interface_disabled
Sep 27 12:52:27 Randrake NetworkManager[576]: <info>  [1758952347.5474] device (wlp3s0f3u2): supplicant interface state: interface_disabled -> inactive
```
*I used the `--no-pager` along with `-l` for full output without pager, so I could copy it here.*

Note that you can visually divide the neat part above and the long wall of text at the bottom: top one is the info, and bottom one is the log.

When running `systemctl list-unit-files`, it will show the state and (default) preset of each unit files.
- There are three states: active, 

### SysVinit

