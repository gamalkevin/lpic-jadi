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

### First, the **bootloader**
The bootloader is like the smart guy at the bar who knows exactly who you should talk to in order to get what you need.

### 🔹 BIOS vs. UEFI
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

### 🔹 GRUB vs. LILO

- **LILO** is like the _bartender who memorized your favorite drink years ago_.
    
    - You have to tell him ahead of time what you want.
        
    - If you change your drink (kernel), you _must_ re-teach him (reinstall the config).
        
    - Simple, fast, but inflexible.
        
- **GRUB** is like the _host who brings you a menu every time you walk in_.
    
    - You can browse the options (multiple kernels, recovery mode, etc.).
        
    - If one drink is out of stock, you can pick another without leaving the bar.
        
    - More powerful, but also a bit heavier.
