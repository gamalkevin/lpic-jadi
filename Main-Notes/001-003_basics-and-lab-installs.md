# 001-003 - Basics and Lab Installations

I have already watched these videos, and determined that there's no need to write down the notes of these videos.
I'll document my labs here anyway, shall the needs for references arise.

## Virt. Platform 🏗️
I'm using [QEMU-KVM](https://tinypilotkvm.com/blogs/insights/kvm-vs-qemu) for all of my labs virtualization, as its hardware passthrough is vastly superior compared to other virtualization software, enabling near-native GPU and device performance in a VM[^1].

[^1]: [QEMU vs VirtualBox vs VMWare](https://www.diskinternals.com/vmfs-recovery/qemu-vs-virtualbox-vs-vmware/)

---
## 🔬 Current Labs 🧪

You might think that I can't decide which platform I want to have as labs, since I keep deleting and having new ones. Anyways, here's the current labs that I have. *Updated **16 Oct 2025**.*

| **Distribution**        | **Details**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rocky Linux 10**      | [As the lesson goes](Main-Notes/017_rpm-yum-overview.md), the study requires me to have systems with `rpm` package management. **Rocky Linux**, being designed to be 100% bug-for-bug with **Red Hat Enterprise Linux (RHEL)**, naturally has `rpm`. Two birds with one stone, you might say.<br><br>It also has `yum` and `dnf`, perfect for practice.<br><br>I chose **[Rocky Linux 10](https://wiki.rockylinux.org/rocky/version/)** since I prefer to have the latest system available. I might install previous version(s) shall the need for legacy system(s) arise. |
| **OpenSUSE Tumbleweed** | Same as Rocky above in regards of `rpm`, but **OpenSUSE** ships with `zypper` instead of `yum`/`dnf`.<br><br>I choose [**Tumbleweed**](https://get.opensuse.org/tumbleweed/), since it behaves like Arch due to its rolling-release nature. Might change to [Leap](https://get.opensuse.org/leap/16.0/) shall the need arise.                                                                                                                                                                                                                                              |
| **Ubuntu Server 25.10** | I need to have a Debian-based lab too.<br><br>Ubuntu, being the *de facto* leader of Debian derivatives, comes to mind. It's polished, but not too shiny. Perfect for my needs.<br><br>[25.10](https://releases.ubuntu.com/questing/), since I like the systems to be latest. Just a personal preference.                                                                                                                                                                                                                                                                  |

## Notes
As you might notice, I'm not using the industry-practice where stability is paramount; I use rolling-release system(s) and latest versions instead of LTS. 

Since I'm still learning, I reckon it's best for me to use the most up-to-date tool available. Will definitely adapt once I'm in the industry. No worries. 


---

## Deleted Labs. 🚮 😵
### ~~Fedora 🎩~~
~~Specifications:~~ 
- ~~**Fedora 42:** [ISO](https://download.fedoraproject.org/pub/fedora/linux/releases/42/Workstation/x86_64/iso/Fedora-Workstation-Live-42-1.1.x86_64.iso)~~
- ~~Gnome DE *(default)*~~
- ~~4 vCPU~~
- ~~8000 MiB RAM~~
- ~~30 GiB VirtIO Disk 1:~~
	- ~~`/var/lib/libvirt/images/fedora42.qcow2`~~

### ~~Ubuntu 🐦‍⬛~~
~~Specifications:~~
- ~~**Ubuntu 24.04:** [ISO](https://ubuntu.com/download/desktop/thank-you?version=24.04.3&architecture=amd64&lts=true)~~
- ~~Gnome DE *(default)*~~
-  ~~4 vCPU~~
- ~~8000 MiB RAM~~
-  ~~30 GiB VirtIO Disk 1:~~
	- ~~`/var/lib/libvirt/images/ubuntu24.04.Fresh Install`~~
~~Initially, I used Ubuntu 25.04, but because I deleted it and later realized I need it again, I reinstalled. However, as you can see it's now 24.04. The `.Fresh Install` is the snapshot after fresh install. Aptly named.~~


## ~~Slackware 🫟~~
~~After a while, I realized previous labs above to be redundant since I am already running Manjaro. So I decided to delete both.~~ 

~~However, I discovered later on ([video 009](009_1-2-runlevels-targets-concepts.md)) that I needed to have a system with **SysVinit**.~~ 

~~I already have AntiX and MX Linux with `sysvinit`, but both are installed on a USB drive. Since I don't have a spare laptop to run it side-by-side with my study, I decided to make a KVM/QEMU using MX Linux, though it was ultimately failed (ISO not recognized by `virt-manager`; no time to troubleshoot & need to get it ready quickly).~~

~~So I opted to install **Slackware**, which turned out to be as time consuming to install, if not more. *One of the least beginner-friendly distro ever. Dang.*~~ 

~~Anyways, I got it up and running successfully. Here's the details:~~ 
- ~~**Slackware 15:** [ISO](https://iso.ukdw.ac.id/slackware/slackware64-15.0-iso/)~~
- ~~XFCE~~
- ~~4 vCPU~~
- ~~8192 MiB / 8 GiB RAM~~
- ~~25 GiB VirtIO Disk 1:~~
	- ~~`/var/lib/libvirt/images/slackware15.0.qcow2`~~

~~Well, somehow it's bricked after some use. I can't get past LILO to boot the Slack system again. So I end up deleting it.~~