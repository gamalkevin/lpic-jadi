# 009 - 101.3 - Part 1 of 2 - Runlevels and targets, Concepts and updating 🔄
## 101.3: Change runlevels / boot targets and shutdown or reboot system

|**Weight**|**3**|
|---|---|
|**Description**|Candidates should be able to manage the SysVinit runlevel or systemd boot target of the system. This includes changing to single user mode, shutdown/reboot, alerting users, and properly terminating processes. Also includes setting the default runlevel/boot target. Awareness of Upstart as an alternative.|

**Key Knowledge Areas:**

- Set the default runlevel or boot target.
    
- Change between runlevels / boot targets including single user mode.
    
- Shutdown and reboot from the command line.
    
- Alert users before switching runlevels / boot targets or other major system events.
    
- Properly terminate processes.
    
- Awareness of `acpid`.
    

**Partial list of files, terms, utilities:**  
`/etc/inittab`, `shutdown`, `init`, `/etc/init.d/`, `telinit`, `systemd`, `systemctl`, `/etc/systemd/`, `/usr/lib/systemd/`, `wall`

---

I accidentally learned about runlevels when reading about **SysVinit vs Systemd** back in video **008**. I even compiled a [cheat-sheet](/References/init-systems.md#systemd-vs-sysvinit-cheat-sheet) in the References folder to differentiate the two, followed by side-by-side [comparison](/References/init-systems.md#notes-on-runlevelstargets) of runlevels and its systemd target equivalent. So I already have some degree of understanding regarding the concept.

However, Jadi put an interesting way to describe runlevels: Try to think of it like human consciousness. At the first levels, it's like the early infancy phase, even go as far as antenatal. Then after that, comes life with its stages: childhood, adulthood, and end up in old age, where things are 'less alive'.

## System Target 🎯
Targets are a combination of services. As previously said, `systemd` can handle super complicated task, like:
- Making targets dependent to each other;
- Making a particular target starts **only after** another; 
- *et cetera*.

As I said before, since I am already using Linux (Manjaro) as a daily driver, I already know how package details are written in Pacman. So viewing targets using `systemctl` yields a familiar description:
```
# systemctl cat multi-user.target
# /usr/lib/systemd/system/multi-user.target
#  SPDX-License-Identifier: LGPL-2.1-or-later
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify>
#  under the terms of the GNU Lesser General Public License as pub>
#  the Free Software Foundation; either version 2.1 of the License>
#  (at your option) any later version.

[Unit]
Description=Multi-User System
Documentation=man:systemd.special(7)
Requires=basic.target
Conflicts=rescue.service rescue.target
After=basic.target rescue.service rescue.target
AllowIsolate=yes
```

## Systemctl Isolate 🏥
Doing `cat` to a target like the one above, we can notice the part where it says `AllowIsolate=yes`.

According to the `man` [page](https://www.freedesktop.org/software/systemd/man/latest/systemctl.html#isolate%20UNIT): 
> **isolate _`UNIT`_**
> 
> Start the unit specified on the command line and its dependencies and stop all others, unless they have `IgnoreOnIsolate=yes`

So if we were to execute `systemctl isolate multi-user`, it will get us out of `graphical.target` and into the defined `multi-user.target`. 

`multi-user.target` doesn't have GUI, so it will bring us to the command-line interface/CLI of the system. See below:
![isolate multi-user](/References/Files/009_isolate-multi-user.gif)

Likewise, isolating into `rescue` will get you into the `rescue.target`. Unlike multi-user, root password is not required in rescue mode. Physical access is enough.

An even less 'alive' condition is the `emergency.target`:
- Running `systemctl is-system-running` while in `rescue` and `emergency` modes will yield `maintenance`.

**Note:**
Trying to isolate into `rescue` seemingly sent my Fedora VM into a coma. I had to mash `alt + F2` to manually get into the  console. However, it brought an error:
![Failed Isolate Rescue](/References/Files/009_isolate-rescue.gif)
*`Cannot open access to console, the root account is locked`*

Turns out, On many modern Linux distros (including Ubuntu, Manjaro, Fedora), the root account is disabled for security reasons, and users are expected to use **normal user + sudo** instead.

To get into recovery/rescue mode, we can do so by choosing the appropriate kernel through GRUB.

## Runlevel Compatibility in Ubuntu 💾
As Jadi has shown, Ubuntu (and apparently its derivatives) still maintain backwards compatibility for `runlevel`.  I tested with Xubuntu and confirmed that it's working.

### 📂 `/etc/init.d/` 
This directory contains old-style init (SysVinit) scripts. 
- Each file inside (`/etc/init.d/ssh`, `/etc/init.d/networking`, etc.) is a **shell script** that can:
    - Start a service  
    - Stop a service
    - Restart a service
    - Check its status

Example (old way): 
```
sudo /etc/init.d/ssh start 
sudo /etc/init.d/ssh stop
```

**So what happens when running services the old way?**
- When running something like:  
    `sudo service ssh start`
    → `systemd` actually **redirects the command** to the proper systemd unit (`sshd.service`).
- If there’s no systemd unit, `systemd` can still **execute the old SysV script** from `/etc/init.d/`.

### 📂 `/etc/rcN.d` 
The `N` after `rc` corresponds to a `runlevel`:

|Runlevel|Meaning|
|---|---|
|`0`|Halt (shutdown)|
|`1`|Single-user mode (rescue/maintenance)|
|`2`|Multi-user mode (without networking, on some distros)|
|`3`|Multi-user mode with networking (text-only)|
|`4`|Undefined/custom (rarely used)|
|`5`|Multi-user mode with networking + graphical login|
|`6`|Reboot|
> In **Debian/Ubuntu**, `2`, `3`, `4`, `5` are usually the same (multi-user with networking).

#### Directory structure 🌳
- Example: `/etc/rc2.d/`
    - Contains **symlinks** to scripts in `/etc/init.d/`
    - Names follow a convention:
        - `SNNname` → Start script at sequence number `NN`
        - `KNNname` → Kill (stop) script at sequence number `NN`
For example:
```
/etc/rc2.d/
 ├── S01dbus -> ../init.d/dbus
 ├── S02networking -> ../init.d/networking
 ├── K01bluetooth -> ../init.d/bluetooth
```
Here:
- `S01dbus` starts the **D-Bus** service very early in runlevel 2.
- `K01bluetooth` stops **Bluetooth** when leaving this runlevel.

#### How it worked ⚡
- When changing runlevels (`init 3`, `init 5`, etc.), the system:
    1. Executed all `K` scripts in the target directory (to stop unneeded services).
        
    2. Executed all `S` scripts in the target directory (to start required services).
        
    3. Order was defined by the number (e.g., `S01` runs before `S20`).
        

### On modern Ubuntu / `systemd` era  🛠
- `/etc/rcN.d/` still exists for **compatibility**.
- Under the hood, `systemd` translates these SysV scripts into native service units.
- Instead of `init` + runlevels, Ubuntu now uses **targets**:
    - `graphical.target` ≈ runlevel 5
    - `multi-user.target` ≈ runlevel 3
    - `rescue.target` ≈ runlevel 1
    - `poweroff.target` ≈ runlevel 0
    - `reboot.target` ≈ runlevel 6
