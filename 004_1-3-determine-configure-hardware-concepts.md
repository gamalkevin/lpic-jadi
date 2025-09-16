# 004 - 101.1 - Part 1 of 3 - Determine and Configure Hardware, Concepts 📚️

We're starting with: 
- **LPIC Topic 101.1: - Determine and configure hardware settings**.

## Hardware 🖥️
This concept is among the harder topics in LPIC. It is generally easier to do more 'complicated' tasks like setting up web servers, since we are executing commands and expect it to behave accordingly. 

Even a lot of professional Linux Administrators [never configured hardware settings](https://youtu.be/xCPDxgp0zXY?si=IxEF1shxlCiKCLsX&t=58). They work on systems that are (already) working. No need to troubleshoot specific driver(s), or peripherals from years ago.

## BIOS

**Basic Input/Output Software** (BIOS) is a firmware that bridges between software and hardware (motherboard). It was old, and has highly limited capabilities.
- It can only boot the first sector of the harddisk, called **MBR/Master Boot Record**. 
- MBR is very little, so the computer needs **two-step boot procedures**.
- First stage is stored on MBR, and it points to the actual system on the drive and starts the second stage.

## UEFI
**Unified Extensible Firmware Interface** (UEFI), initially EFI in 1998 by Intel, is a next-gen alternative from BIOS. 
- Almost every modern PCs uses UEFI
- Instead of the first sector of the harddisk, it uses a dedicated partition (`FAT` partition type); On Linux, it's `/boot`
- {*kev-note*} = Multi-boot OSes can share the EFI partition, though it's best and less 'problematic' if the OSes are similar (Linux-Linux, not Linux-Windows, etc.)

## Peripherals
In the early days, computers are made as a 'block'. Meaning, everything was assembled without a way to change parts. 

For example, there was no way to upgrade an existing modem in computers. You had to buy a whole new computer for even a single upgrade. This was incredibly expensive and wasteful.

Thus, peripherals are made. We can swap out RAM sticks, processors, etc.

### PCI
**Peripheral Component Interconnect** lets **expansion cards** (GPU, sound card, NIC, RAID Controller, etc.) to connect with the motherboard. 

PCI has since evolved into PCIe (e stands for express): 
- PCIe is serial and sends data in lanes; allowing much higher bandwidth. 
- Compare it to PCI with its limited speeds due to the parallel nature of PCI (all signals sent together)
- PCIe allows devices to communicate directly with the CPU and memory efficiently

### USB
**Universal Serial Bus** is also serial, as its name suggests. Almost everyone will notice it compared to other peripheral connection methods.

### GPIO
**General Purpose Input/Output** is mainly used on SBCs (Single Board Computers) and Embedded Systems.
- As the name suggests, it's general. Think of it as programmable digital pin
- The pins can be controlled/accessed separately (no need to use all the pins at once)
- The pins are **binary**. Basically every pin is either on or off, which can later be configured as far as you want. 

---

First part done, let's head to [part 2 of 3](./005_2-3-determine-configure-hardware-kernel.md) next.