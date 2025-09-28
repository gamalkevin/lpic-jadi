# 001-003 - Basics and Lab Installations

I have already watched these videos, and determined that there's no need to write down the notes of these videos.

I'll document my labs here anyway, shall the needs of references arise.

## Virt. Platform 🏗️
I'm using [QEMU-KVM](https://tinypilotkvm.com/blogs/insights/kvm-vs-qemu) for all of my labs virtualization, as its hardware passthrough is vastly superior compared to other virtualization software, enabling near-native GPU and device performance in a VM[^1].

[^1]: [QEMU vs VirtualBox vs VMWare](https://www.diskinternals.com/vmfs-recovery/qemu-vs-virtualbox-vs-vmware/)

## Initial Labs; deleted.
```
## Lab 1 - Fedora 🎩
Specifications: 
- **Fedora 42:** [ISO](https://download.fedoraproject.org/pub/fedora/linux/releases/42/Workstation/x86_64/iso/Fedora-Workstation-Live-42-1.1.x86_64.iso)
- Gnome DE *(default)*
- 4 vCPU
- 8000 MiB RAM
- 30 GiB VirtIO Disk 1:
	- `/var/lib/libvirt/images/fedora42.qcow2`

## Lab 2 - Ubuntu 🐦‍⬛
Specifications:
- **Ubuntu 25.04:** [ISO](https://ubuntu.com/download/desktop/thank-you?version=24.04.3&architecture=amd64&lts=true)
- Gnome DE *(default)*
-  4 vCPU
- 8000 MiB RAM
-  25 GiB VirtIO Disk 1:
	- `/var/lib/libvirt/images/ubuntu25.04.qcow2`
```

## Current Lab - Slackware
After a while, I realized both labs above to be redundant since I am already running Manjaro. However, I discovered later on ([video 009](./009_1-2-runlevels-targets-concepts.md)) that I needed to have a system with `sysvinit`. 

I already have AntiX and MX Linux with `sysvinit`, but both are installed on a USB drive. Since I don

Specifications: 
- **Slackware 15:** [ISO](https://iso.ukdw.ac.id/slackware/slackware64-15.0-iso/)
- XFCE
- 4 vCPU
- 8192 MiB / 8 GiB RAM
- 25 GiB VirtIO Disk 1:
	- `/var/lib/libvirt/images/slackware15.0.qcow2`