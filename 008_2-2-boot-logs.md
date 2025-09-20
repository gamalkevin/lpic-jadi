# 008 - 101.2 - Part 2/2 - Boot Process; logs (dmesg, logs, journalctl) & init (systemd&SysV) 📝

## `dmesg`
One great things about Linux system is that everything is extensively logged. So in case something goes wrong, we can go see the log and examine the problems.

Upon boot, all kernel messages (*hardware, drivers, boot events, etc.*) are stored in **kernel ring buffer**; think of it like a tear-off notepad where the kernel stores messages. It is **limited to current buffer** like a ring, so after a while it will get full, and new data/message will overwrite old ones.  

`dmesg` means **display/diagnostic message**; it prints all the messages stored in the kernel ring buffer. It is the kernel's built-it log viewer.