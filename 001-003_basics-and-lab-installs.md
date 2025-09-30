# 001-003 - Basics and Lab Installations

I have already watched these videos, and determined that there's no need to write down the notes of these videos.
I'll document my labs here anyway, shall the needs for references arise.

## Virt. Platform 🏗️
I'm using [QEMU-KVM](https://tinypilotkvm.com/blogs/insights/kvm-vs-qemu) for all of my labs virtualization, as its hardware passthrough is vastly superior compared to other virtualization software, enabling near-native GPU and device performance in a VM[^1].

[^1]: [QEMU vs VirtualBox vs VMWare](https://www.diskinternals.com/vmfs-recovery/qemu-vs-virtualbox-vs-vmware/)

---

It seems like I can't decide which platform I want to have as labs, since I keep deleting and having new ones. Anyways, here's the history.

## Initial Labs; deleted, but restored. 🚮 😵
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
- **Ubuntu 24.04:** [ISO](https://ubuntu.com/download/desktop/thank-you?version=24.04.3&architecture=amd64&lts=true)
- Gnome DE *(default)*
-  4 vCPU
- 8000 MiB RAM
-  30 GiB VirtIO Disk 1:
	- `/var/lib/libvirt/images/ubuntu24.04.Fresh Install`
Initially, I used Ubuntu 25.04, but because I deleted it and later realized I need it again, I reinstalled. However, as you can see it's now 24.04. The `.Fresh Install` is the snapshot after fresh install. Aptly named.


## Another deleted lab - Slackware 🫟
After a while, I realized both labs above to be redundant since I am already running Manjaro. So I decided to delete both. 

However, I discovered later on ([video 009](./009_1-2-runlevels-targets-concepts.md)) that I needed to have a system with **SysVinit**. 

I already have AntiX and MX Linux with `sysvinit`, but both are installed on a USB drive. Since I don't have a spare laptop to run it side-by-side with my study, I decided to make a KVM/QEMU using MX Linux, though it was ultimately failed (ISO not recognized by `virt-manager`; no time to troubleshoot & need to get it ready quickly).

So I opted to install **Slackware**, which turned out to be as time consuming to install, if not more. *One of the least beginner-friendly distro ever. Dang.* 

Anyways, I got it up and running successfully. Here's the details: 
- **Slackware 15:** [ISO](https://iso.ukdw.ac.id/slackware/slackware64-15.0-iso/)
- XFCE
- 4 vCPU
- 8192 MiB / 8 GiB RAM
- 25 GiB VirtIO Disk 1:
	- `/var/lib/libvirt/images/slackware15.0.qcow2`

Well, somehow it's bricked after some use. I can't get past LILO to boot the Slack system again. So I end up deleting it.