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
Lists PCI devices 

###
