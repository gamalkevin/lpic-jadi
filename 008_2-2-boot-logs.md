# 008 - 101.2 - Part 2/2 - Boot Process; logs (dmesg, logs, journalctl) & init (systemd&SysV) 📝

## `dmesg`
One great things about Linux system is that everything is extensively logged. So in case something goes wrong, we can go see the log and examine the problems.

Upon boot, all kernel messages (*hardware, drivers, boot events, etc.*) are stored in **kernel ring buffer**; think of it like a tear-off notepad where the kernel stores messages. It is **limited to current buffer** like a ring, so after a while it will get full, and new data/message will overwrite old ones.  

`dmesg` means **display/diagnostic message**; it prints all the messages stored in the kernel ring buffer. It is the kernel's built-it log viewer.
- It also stores information about other devices, like USB. 
	- That's the reason why `dmesg` is used to check whether USB/other peripherals are connected & detected by the system or not.

### `dmesg` vs `journalctl -k`
- `dmesg` shows only what is in the **current kernel ring buffer**, and older messages eventually get overwritten.
- `journalctl -k` (from `systemd`) shows **persistent kernel logs**, saved across reboots, if your system uses `systemd-journald`.