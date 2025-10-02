# 011 - 102.1 - Part 1 of 2 - Design Hard Disk Layout - Filesystem Hierarchy Standard
## 102.1: Design hard disk layout

|**Weight**|**2**|
|---|---|
|**Description**|Candidates should be able to design a disk partitioning scheme for a Linux system.|

**Key Knowledge Areas:**
- Allocate filesystems and swap space to separate partitions/disks.
- Tailor the design to the intended use of the system.
- Ensure `/boot` partition conforms to hardware architecture requirements.
- Knowledge of basic **LVM** features.

**Partial list of files, terms, utilities:**  
`/` (root), `/var`, `/home`, `/boot`, EFI System Partition (ESP), swap space, mount points, partitions

---


## Filesystem Hierarchy Standard / FHS
|Directory|Purpose|
|---|---|
|`/` (root)|The **top of everything**. All files and dirs start here.|
|`/bin`|**Essential binaries** (commands like `ls`, `cp`, `mv`, `cat`). Needed even in single-user mode.|
|`/sbin`|**System binaries** (like `fsck`, `ifconfig`, `reboot`). Used by root/admin.|
|`/etc`|**System configuration** files (static text configs). Example: `/etc/passwd`, `/etc/fstab`.|
|`/dev`|**Device files** (e.g. `/dev/sda`, `/dev/null`). Created by `udev`.|
|`/proc`|**Virtual filesystem** with kernel & process info. Example: `/proc/cpuinfo`.|
|`/sys`|**Virtual filesystem** exposing kernel/sysfs info.|
|`/tmp`|Temporary files (wiped on reboot).|
|`/var`|Variable data → logs (`/var/log`), spool files, caches.|
|`/home`|User home directories.|
|`/root`|Root user’s home directory.|
|`/boot`|Boot loader files, kernel, initramfs.|
|`/lib`|Essential shared libraries & kernel modules.|
|`/usr`|**User programs & data**. Has subdirs like `/usr/bin`, `/usr/lib`.|
|`/opt`|Optional add-on software (3rd party apps).|
|`/media`|Mount points for removable media (USBs, DVDs).|
|`/mnt`|Temporary mount point (admin mounting).|
|`/srv`|Service data (e.g. web server files in `/srv/www`).|