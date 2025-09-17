# 007 - 101.2 - Part 1 of 2 - The Boot Process; Concepts 📚️

## The Boot Process
We have very little control during the first moments of turning on the computer. Let's understand more what's happening:
1. Motherboard firmware does a PowerOnSelfTest / **POST**
	1. It's checking and testing available hardware, which continues the booting process if everything goes well.
2. All is well, the motherboard loads the bootloader
3. Bootloader loads the kernel and its configs
	1. In GRUB (The current default bootloader for Linux), we can customize the behavior of the Linux distro that we're booting: different kernel versions, safe-mode, terminal only, etc.
	2. It can even detect other installed Linux distros and Windows too. 
4. Kernel loads, prepares the system / '**rootfs**' root filesystem and runs the initialization program / **init**
5. Init then loads system services (networking, GUI, etc.) and other startup programs.
6. System ready to use

## Boot Process Analogies
To understand the boot process, I headed to ChatGPT to obtain some useful analogies. Access them [here](./References/boot-process-analogies.md).

## Linux vs. GNU/Linux  
- **Linux** strictly means the **kernel**, the core program that manages hardware and system resources.  
- By itself, the kernel can’t do much—you wouldn’t even have commands like `ls` or `cd`.  
- When combined with the **GNU tools** (shell, utilities, libraries), it forms a complete operating system.  
- Most distributions are this combination, which is why the technically accurate name is **GNU/Linux**—though in practice, people usually just say **Linux**.  

