# 009 - 101.3 - Part 1 of 2 - Runlevels and targets, Concepts and updating 🔄
Continuing with **LPIC Topic 101.3: Change runlevels / boot targets and shutdown or reboot system**.

I accidentally learned about runlevels when reading about **SysVinit vs Systemd** back in video **008**. I even compiled a [cheat-sheet](./References/init-systems.md#systemd-vs-sysvinit-cheat-sheet) in the References folder to differentiate the two, followed by side-by-side [comparison](./References/init-systems.md#notes-on-runlevelstargets) of runlevels and its systemd target equivalent. So I already have some degree of understanding regarding the concept.

However, Jadi put an interesting way to describe runlevels: We think of it like human consciousness. At the first levels, it's like the early infancy phase, even go as far as antenatal. Then after that, comes life with its stages: childhood, adulthood, and end up in old age, where things are 'less alive'.

## System Target
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

## Systemctl Isolate
Doing `cat` to a target like the one above, we can notice the part where it says `AllowIsolate=yes`.

According to the `man` [page](https://www.freedesktop.org/software/systemd/man/latest/systemctl.html#isolate%20UNIT): 
> **isolate _`UNIT`_**
> 
> Start the unit specified on the command line and its dependencies and stop all others, unless they have `IgnoreOnIsolate=yes`

So if we were to execute `systemctl isolate multi-user`, it will get us out of `graphical.target` and into the defined `multi-user.target`. 

`multi-user.target` doesn't have GUI, so it will bring us to the command-line interface/CLI of the system.