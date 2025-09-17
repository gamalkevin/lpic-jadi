# 007 - 101.2 - Part 1 of 2 - The Boot Process; Concepts 📚️

## The Boot Process
We have very little control during the first moments of turning on the computer. Let's understand more what's happening:
1. Motherboard firmware does a PowerOnSelfTest / **POST**
	1. It's checking whether the hardware are adequate to run the bootloader, which continues the booting process.
2. All is well, the motherboard loads the bootloader
3. Bootloader loads the kernel and its configs
	1. In GRUB (The current default bootloader for Linux), we can customize the behavior of the Linux distro that we're booting: different kernel versions, safe-mode, terminal only, etc.
	2. It can even detect other installed Linux distros and Windows too. 
4. Kernel loads, prepares the system / '**rootfs**' root filesystem and runs the initialization program / **init**
5. Init then loads system services (networking, GUI, etc.) and other startup programs.
6. System ready to use

## Boot Process Analogies
To understand the boot process, I headed to ChatGPT to obtain some useful analogies.

### First, the **bootloader**
The bootloader is like the smart guy at the bar who knows exactly who you should talk to in order to get what you need.

### BIOS vs. UEFI
- **BIOS** is like the _old, grumpy bouncer at the bar_.    
    - He’s been around forever.
    - He doesn’t give you many details, just checks your ID, then pushes you inside.
    - He can only handle small, simple IDs (like old 16-bit code and MBR partitions).
    - If the person you need isn’t right there at the door, tough luck—you’re on your own.
        
- **UEFI** is like the _modern event coordinator at a fancy lounge_.
    - She has a guest list, a microphone, and even a map of the place.
    - She can handle big, complicated IDs (64-bit, GPT disks).
    - She can give you menus, point you to multiple areas, and even run little programs before you get to your contact.
    - Much more flexible and future-proof, but more complex.

### GRUB
- **GRUB** is like the _host who brings you a menu every time you walk in_.
    - You can browse the options (multiple kernels, recovery mode, etc.).
    - If one drink is out of stock, you can pick another without leaving the bar.
    - More powerful than its predecessor (LILO), but also a bit heavier.

### 🔹 initramfs/initrd
This is like the _wingman_ your smart guy (bootloader) introduces you to first.
- He doesn’t run the whole show, but he helps you get settled.
- He gathers the basics you’ll need—like finding your coat check ticket (drivers/modules for disks, filesystems).
- Without him, you might not even be able to reach the person you’re here for (the kernel can’t mount the real root filesystem).

### 🔹 Kernel
This is the _VIP you actually came to see_.
- He’s the one running the whole party.
- Once you’re introduced, he takes charge—managing the place, setting the rules, and making sure everyone behaves (process scheduling, memory management, device handling).
- From here on, he doesn’t need the bootloader or initramfs anymore—he’s in control.

