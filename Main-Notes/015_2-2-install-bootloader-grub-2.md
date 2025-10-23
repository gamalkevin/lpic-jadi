# Part 2 of 2: Install Bootloader - GRUB 2

## **GRUB 2 (GRand Unified Bootloader v2)**
Most widely used boot manager on Linux.

**Configuration files:**
- `/boot/grub/grub.cfg` — auto-generated (do **not** edit manually)
- `/etc/default/grub` — main user configuration; edit here instead
- `/etc/grub.d/` — script fragments to build the GRUB menu

**Generate Configuration:**
`sudo grub-mkconfig -o /boot/grub/grub.cfg`

**Install GRUB (BIOS systems):**
`sudo grub-install /dev/sda`

**Install GRUB (UEFI systems):**
`sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB`

---

## **GRUB Kernel Parameters**
### 🧠 **Basic Boot Options**

|Parameter|Description|
|---|---|
|`ro`|Mounts root filesystem **read-only** (default).|
|`rw`|Mounts root filesystem **read-write**.|
|`root=/dev/sdXn`|Defines which partition to mount as `/`.|
|`init=/bin/bash`|Bypasses init/systemd and launches a shell directly.|
|`quiet`|Suppresses most boot messages (default on desktops).|
|`splash`|Shows splash screen (like Plymouth).|
|`nomodeset`|Disables kernel mode setting; useful for fixing GPU-related boot issues.|

---

### 🛠️ **Troubleshooting / Rescue**

|Parameter|Description|
|---|---|
|`single`|Boots into **single-user mode** (maintenance mode).|
|`systemd.unit=rescue.target`|Similar to single-user mode under systemd.|
|`systemd.unit=multi-user.target`|Boots into text-only multi-user mode.|
|`systemd.unit=graphical.target`|Boots into GUI mode (default desktop).|
|`emergency`|Minimal environment — only root shell, no services.|
|`reboot=bios` / `reboot=acpi`|Forces specific reboot method (useful if normal reboot hangs).|
|`ignore_loglevel`|Shows **all** kernel log messages (for debugging).|

---

### 💻 **Hardware / Device Related**

|Parameter|Description|
|---|---|
|`acpi=off`|Disables ACPI (power management) — helps on buggy BIOSes.|
|`noapic`|Disables I/O APIC; fixes some boot issues on old hardware.|
|`nolapic`|Disables Local APIC.|
|`irqpoll`|Helps with interrupt handling errors.|
|`pci=noacpi`|Prevents ACPI from being used for PCI routing.|
|`usbcore.autosuspend=-1`|Disables USB autosuspend (useful for some peripherals).|
|`pci=nomsi`|Disables PCI Message Signaled Interrupts (troubleshooting).|

---

### 🔋 **Power & Performance**

|Parameter|Description|
|---|---|
|`intel_pstate=disable`|Disables Intel’s power management driver (use `acpi_cpufreq` instead).|
|`cpufreq.default_governor=performance`|Sets default CPU frequency scaling governor.|
|`idle=poll`|Disables CPU idle states — keeps CPU fully active (for testing).|
|`maxcpus=1`|Limit to one CPU core (debugging).|
|`nohz=off`|Disables dynamic tick (can help with timing issues).|

---

### 🧩 **Filesystem / Storage**

|Parameter|Description|
|---|---|
|`rootflags=subvol=@`|Specify subvolume (for Btrfs).|
|`rootfstype=ext4`|Force filesystem type.|
|`fsck.mode=force`|Force filesystem check on boot.|
|`fsck.repair=yes`|Auto-repair filesystem issues if found.|
|`rd.break`|Break into dracut emergency shell (for initramfs debugging).|

---

### 🌐 **Networking (during early boot)**

|Parameter|Description|
|---|---|
|`ip=dhcp`|Obtain network configuration via DHCP during boot.|
|`ip=192.168.1.10::192.168.1.1:255.255.255.0::eth0:none`|Manually set IP/gateway/netmask on boot.|

---

### 🔧 **Example Use Case**

If you want to troubleshoot a non-booting system with possible GPU or filesystem problems:

`linux /vmlinuz root=/dev/sda2 ro nomodeset fsck.mode=force single`

→ Disables GPU mode-setting, forces fsck, and enters single-user mode.

---

### **Application**
**Temporary (at boot):**
1. Highlight desired boot entry.    
2. Press `e` to edit.
3. Modify the `linux` line, e.g.:
    `linux /vmlinuz root=/dev/sda2 ro single`
    → Boots into single-user mode.
    
**Permanent (in config file):**  
Edit `/etc/default/grub`:
`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`
Then regenerate configuration:
`sudo update-grub`

---

### **Backup & Recovery**
If GRUB is damaged (e.g. overwritten MBR/EFI), use a live environment:
`sudo mount /dev/sdXn /mnt sudo grub-install --boot-directory=/mnt/boot /dev/sdX`

---

### **Exam Tips**
- Know GRUB 2 file locations and update commands.
- Understand BIOS vs UEFI installation targets.
- Remember where boot code lives:
    - **BIOS:** Master Boot Record (MBR)
    - **UEFI:** EFI System Partition (ESP)