# 009 - 101.3 - Part 1 of 2 - Runlevels and targets, Concepts and updating 🔄
Continuing with **LPIC Topic 101.3: Change runlevels / boot targets and shutdown or reboot system**.

I accidentally learned about runlevels when reading about **SysVinit vs Systemd** back in video **008**. I even compiled a [cheat-sheet](./References/init-systems.md#systemd-vs-sysvinit-cheat-sheet) in the References folder to differentiate the two, followed by side-by-side [comparison](./References/init-systems.md#notes-on-runlevelstargets) of runlevels and its systemd target equivalent. So I already have some degree of understanding regarding the concept.

However, Jadi put an interesting way to describe runlevels: We think of it like human consciousness. At the first levels, it's like the early infancy phase, even go as far as antenatal. Then after that, comes life with its stages: childhood, adulthood, and end up in old age, where things are 'less alive'.

## System Target
Targets are a combination of services. As previously said, `systemd` can handle super complicated task, like:
- Making services inside targets dependent to each other;
- Making a particular service starts **only after** another particular service; 
- *et cetera*.
