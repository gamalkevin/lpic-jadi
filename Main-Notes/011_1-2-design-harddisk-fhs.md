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
Linux uses a single, tree-like filesystem structure, starting from the root (`/`), unlike Windows which separates drives into compartments like `C:` or `D:`.


### FHS Cheatsheet
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
|`/opt`|Optional add-on software (3rd party apps); to avoid mixing with main files in `/bin`.|
|`/media`|Mount points for removable media (USBs, DVDs).|
|`/mnt`|Temporary mount point (admin mounting).|
|`/srv`|Service data (e.g. web server files in `/srv/www`).|

## `/` vs `/root`
|Feature|`/`|`/root`|
|---|---|---|
|Purpose|Root of entire filesystem|Home folder of the root (admin) user|
|Access|All users (read-only for most)|Only root user|
|Type|System-level|User-level (for root)|
|Analogy|“C:\”|“C:\Users\Administrator”|
|Mount Point|Main root partition|Inside root filesystem (or sometimes separate partition)|

## `/bin` vs `/sbin`
|Aspect|`/bin`|`/sbin`|
|---|---|---|
|**User**|All users|Root/admin|
|**Command type**|Regular commands|System admin commands|
|**Boot importance**|Yes, essential|Yes, essential for maintenance|
|**Example commands**|`ls`, `cp`, `cat`|`shutdown`, `mount`, `fsck`|

### What about `/usr/bin`? 🤔
Modern Linux systems merge `/bin` into `/usr/bin` because:
- In the past, `/bin` held essential tools needed before `/usr` was mounted.
- Now `/usr` is always available early in boot, so two folders aren’t needed.
- To simplify things, `/bin` is just a [^2]**symlink** to `/usr/bin`.

Basically:
> **All executables live in `/usr/bin`, and `/bin` exists only for backward compatibility.**

## Removable Disks
Since Linux uses a tree-like filesystem, any removable device we plug in is mounted and become part of that **single hierarchy** — *unlike Windows*[^1], where each drive acts as a separate compartment. You could say Windows behaves like modular sections of the International Space Station, while Linux works more like a unified organization.

For example, a USB disk labeled as `MyDisk` will be automatically mounted to `/media`, or manually to `/mnt`. 

#### What's the difference?
|Aspect|`/mnt`|`/media`|
|---|---|---|
|**Mount type**|Manual|Automatic|
|**User**|Sysadmin|Desktop system|
|**Structure**|Single mount point|Subfolders per device|
|**Persistence**|Optional via `/etc/fstab`|Dynamic (auto-managed)|
|**Typical use**|Maintenance, testing, manual access|Everyday user removable media|

[^1]: Windows assigns separate drive letters (like `C:` or `D:`) to each storage device, creating independent filesystem roots rather than one unified hierarchy.

[^2]: A **symlink** (short for **symbolic link**) is like a _shortcut_ or _pointer_ to another file or folder.
	
	When you open or run a symlink, the system automatically follows it to the real target.
