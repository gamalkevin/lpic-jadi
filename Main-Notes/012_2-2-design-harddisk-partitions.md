# Part 2 of 2: Design Hard Disk Layout - Partitions
Continuing with **Partitions**.

**Note**: I've been using Linux for years now. I have installed multiple distros throughout the years, and most of them using **manual partitioning** option. It is fair to say that I am already quite experienced in this particular topic. 

So instead of writing notes for this video, I'll summarize my knowledge here.

Also, I made a partitioning cheatsheet [here](/References/partition-cheat-sheet.md).

---

## Knowledge Recap: Disk Partitioning 🧠

**Concept summary:**
- The disk is divided into partitions that the OS uses as separate filesystems.
- The partition table defines how the disk is structured (MBR vs GPT).
- Linux sees everything as a file, including `/dev/sda`, `/dev/sda1`, etc.

**Tools I use regularly:** `lsblk`, `fdisk`, `parted`/`gparted` (GUI), `blkid`, `df`, `mount`, `umount`
- **Notes**: I prefer `gparted` for making partitions, as it is almost as quick as making partitions through raw text commands. 
	- Also, the GUI acts like a personal safety-net for checking and rechecking what I'm doing to the partitions. 
	- This is especially important when preparing partitions for system installations.
- Previously I used **Gnome Disk** through Ubuntu Live Systems for making `.img` disk backups.
	- However, the resulting image is a raw sector-by-sector copy; it includes **all empty space** (and even deleted files), so it’s bigger than necessary.
	- Now I use **[Rescuezilla](https://rescuezilla.com/)**[^1] for better space-saving measures:
		- It only copies _used_ sectors, skipping unused ones
		- Backup images end up smaller and can also be **compressed** (gzip/lz4/zstd)
		- For example, I backed up a 128 GB disk (around 100 GB of files) and the backed up files end up taking **less than 60 GB**. That's not even using the 'best' encryption method.
    
**My workflow when partitioning:**
1. Identify the target drive with `lsblk`.
2. Use `gparted` to create partitions.

As some of you might have pointed out, this is a bad exercise.

## Manual Partitioning Commands
Since LPIC requires comfortable level of usage with the CLI/underlying tools beneath `gparted`, I will also write my workflow to manually create partitions.

This is also a major requirement, since LPIC exams are text-based, and we’re tested conceptually and practically on command-line usage — not GUIs. We’ll see questions about `fdisk`, `parted`, `lsblk`, `mkfs`, `mount`, and `/etc/fstab`.  
> GUI tools like `gparted` aren’t covered because LPIC assumes a minimal system (like a server) where you may not have a desktop environment.

I used these tools when trying to install Slackware, so I have actually tried and tested this.

**'Manual' Partitioning Workflow:**
- Identify the target drive with `lsblk`.
- Use `fdisk /dev/sdX` → `p` to print, `n` to create, `w` to write.
- Format with `mkfs.ext4 /dev/sdXn`.
- Mount manually or update `/etc/fstab` if permanent.

**What I’d tell a beginner:**  
Don’t rush into commands; visualize the disk layout first. Draw it if necessary. 

We **can** use a GUI tool for initial learning and familiarization, but we still need to get our hands on the actual tools used by the GUI program.

[^1]: - An **easy-to-use fork** of Clonezilla, designed to be beginner-friendly.
	- You boot it from a live USB (like Clonezilla).
	- Instead of the text-only ncurses menus Clonezilla uses, Rescuezilla gives you a **point-and-click graphical interface**.
