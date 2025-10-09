# 014 - 102.2 - Part 1 of 2 - Install Bootloader - GRUB Legacy
## 102.2: Install a boot manager

|**Weight**|**2**|
|---|---|
|**Description**|Candidates should be able to select, install and configure a boot manager.|

**Key Knowledge Areas:**
- Provide alternative boot locations and backup boot options.
- Install and configure a boot loader such as GRUB Legacy.
- Perform basic configuration changes for GRUB 2.
- Interact with the boot loader.

**Partial list of files, terms, utilities:**  
`menu.lst`, `grub.cfg`, `grub.conf`, `grub-install`, `grub-mkconfig`, MBR

---

## **Boot Process Overview**
1. **Firmware Stage (BIOS or UEFI)**
    - Initializes hardware.
    - Searches for a bootable device according to configured boot order.
    - Loads the **boot sector** (BIOS) or **EFI binary** (UEFI).
        
2. **Boot Loader Stage**
    - Loads the operating system kernel into memory.
    - Provides a menu to select different OSes or kernel versions.
    - Common bootloaders: **GRUB 2**, **LILO (legacy)**, **systemd-boot**, **rEFInd**.
        
3. **Kernel Stage**
    - The kernel takes control and mounts the root filesystem.
    - Starts `init` (or `systemd`) → continues normal boot.

---

## **Common Boot Loaders**

### **LILO (Legacy Bootloader)**

- Older alternative to GRUB.
- Config file: `/etc/lilo.conf`.
- Must run `sudo lilo` after editing configuration.
- Rarely used today, but still covered in exams.

### **GRUB 2 (GRand Unified Bootloader v2)**
Most widely used boot manager on Linux. More in the [next file](./Main-Notes/015_install-bootloader-grub-2.md).