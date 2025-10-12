### 🧾 **LPIC-1 102.1 Partition Cheat Sheet**

|Mount|Typical Size|Notes / Recommended Filesystem|Reason / Purpose|
|---|---|---|---|
|`/boot`|500 MB – 1 GB|ext4 (sometimes ext2)|Holds kernel and initramfs; separate to avoid BIOS/UEFI limitations.|
|`/` (root)|20–50 GB|ext4|OS files, system binaries, default directories.|
|`/home`|Remainder of disk / user data|ext4|User data; separate for OS reinstall without losing files.|
|`/var`|10–50 GB or more (depending on server)|ext4, xfs|Logs, databases, spool files; prevents filling `/`.|
|`/tmp`|2–10 GB|ext4|Temporary files; can mount with `noexec` for security.|
|`swap`|Equal to RAM for hibernation; half RAM if not hibernating|Swap partition or swap file|Virtual memory; needed for hibernation and memory overflow.|
|LVM|Flexible|N/A|Allows dynamic resizing of partitions; useful for servers or changing workloads.|
|RAID|Varies|ext4, xfs|Redundancy/performance; `/boot` on RAID1 recommended for reliability.|

---