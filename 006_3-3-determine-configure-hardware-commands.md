# 005 - Part 2 of 3 - Determine and Configure Hardware, Commands 📣
After learning about hardware, let's take a look into the 'basic' commands that touch them.

## **lsusb, lspci, lsblk, lshw**
If the command `ls` lists files, these commands list their respective hardwares. 

### lsusb
This command lists all USB devices connected to the system. Let's see mine:
```
$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 25a7:fa55 Areson Technology Corp Fantech Helios Pro Wireless XD3 
Bus 001 Device 003: ID 04ca:3015 Lite-On Technology Corp. Qualcomm Atheros QCA9377 Bluetooth
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 0408:a031 Quanta Computer, Inc. VGA WebCam
Bus 003 Device 003: ID 0bda:8179 Realtek Semiconductor Corp. RTL8188EUS 802.11n Wireless Network Adapter
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```

### lspci
- Lists hardware connected through the PCI/PCIe bus (common connection method inside computers).
- *Examples: graphics card, network card, sound card, USB controllers.*
```
$ lspci
00:00.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Root Complex
00:00.2 IOMMU: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 IOMMU
00:01.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:01.6 PCI bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 PCIe GPP Bridge [6:0]
00:01.7 PCI bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 PCIe GPP Bridge [6:0]
00:08.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:08.1 PCI bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus A
00:08.2 PCI bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus B
00:14.0 SMBus: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller (rev 61)
00:14.3 ISA bridge: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge (rev 51)
00:18.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 0
00:18.1 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 1
00:18.2 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 2
00:18.3 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 3
00:18.4 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 4
00:18.5 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 5
00:18.6 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 6
00:18.7 Host bridge: Advanced Micro Devices, Inc. [AMD] Raven/Raven2 Device 24: Function 7
01:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTL8411B PCI Express Card Reader (rev 01)
01:00.1 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8211/8411 PCI Express Gigabit Ethernet Controller (rev 12)
02:00.0 Network controller: Qualcomm Atheros QCA9377 802.11ac Wireless Network Adapter (rev 31)
03:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Raven Ridge [Radeon Vega Series / Radeon Vega Mobile Series] (rev c4)
03:00.1 Audio device: Advanced Micro Devices, Inc. [AMD/ATI] Raven/Raven2/Fenghuang HDMI/DP Audio Controller
03:00.2 Encryption controller: Advanced Micro Devices, Inc. [AMD] Raven/Raven2/FireFlight/Renoir/Cezanne Platform Security Processor
03:00.3 USB controller: Advanced Micro Devices, Inc. [AMD] Raven USB 3.1
03:00.4 USB controller: Advanced Micro Devices, Inc. [AMD] Raven USB 3.1
03:00.6 Audio device: Advanced Micro Devices, Inc. [AMD] Family 17h/19h/1ah HD Audio Controller
04:00.0 SATA controller: Advanced Micro Devices, Inc. [AMD] FCH SATA Controller [AHCI mode] (rev 61)
```
*Quite a list, huh? Wait 'till you see `lshw`.*

### lsblk
This is almost like a personal favorite; I use it a lot whenever doing partitioning or tinkering with USB bootables.

`lsblk` means **list block devices**. Block devices are devices that read/write in fixed-size blocks (e.g., hard drives, SSDs, USB sticks, partitions, even **loop devices** or **RAM disks**).
```
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 931.5G  0 disk 
├─sda1   8:1    0   250G  0 part 
├─sda2   8:2    0   500M  0 part /boot
├─sda3   8:3    0   215G  0 part /
├─sda4   8:4    0   300M  0 part /boot/efi
├─sda5   8:5    0 372.6G  0 part 
├─sda6   8:6    0   732M  0 part 
├─sda7   8:7    0  30.7G  0 part 
└─sda8   8:8    0  61.7G  0 part 
sdb      8:16   0 119.2G  0 disk 
├─sdb1   8:17   0   499M  0 part 
├─sdb2   8:18   0   100M  0 part 
├─sdb3   8:19   0    16M  0 part 
├─sdb4   8:20   0 117.9G  0 part 
└─sdb5   8:21   0   741M  0 part 
```

#### Column meanings
**`MAJ:MIN` (Major:Minor number)**
- These are **device numbers** that the kernel uses internally.
- **Major**: identifies the driver for the device type (e.g., all SCSI/SATA disks share major number `8`).
- **Minor**: identifies the specific device or partition (e.g., `sda` is `8:0`, `sdb` is `8:16`, partitions increment from there).
- Users usually don’t need to care, but it’s how `/dev/sda`, `/dev/sdb1`, etc. are mapped.

**`RM` (Removable)**
- Shows whether the device is **removable** (`1`) or not (`0`).
- Example:
    - `sda` (internal HDD/SSD) → `0`
    - `sdb` (USB stick) → `1`

**`RO` (Read-Only)**
- Shows whether the device is **read-only** (`1`) or not (`0`).
- Example:
    - Normal drives → `0`
    - Mounted ISO file or write-protected USB → `1`

### lshw
- Stands for **List Hardware**.
- Shows **detailed information** about the system's hardware, and goes much deeper than `lsblk`, `lspci`, or `lsusb`.
- *Covers: CPU, RAM, motherboard, firmware/BIOS, storage, network cards, etc.*

Running `lshw` by itself, you will be suggested to run it with elevated commands, since it will yield more accurate results. I won't post my full `lshw` here, since it's incredibly long. 
- Even using `-short` option, it will still list all classes of hardware installed in your system. 
- You might want to run it using `-class` option, so it's easier to read through.