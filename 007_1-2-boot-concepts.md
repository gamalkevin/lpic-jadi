# 007 - 101.2 - Part 1 of 2 - The Boot Process; Concepts 📚️

# The Boot Process
We have very little control during the first moments of turning on the computer. Let's understand more what's happening:
1. Motherboard firmware does a PowerOnSelfTest / **POST**
	1. It's checking whether the hardware are adequate to run the bootloader, which continues the booting process.
2. All is well, the motherboard loads the bootloader
3. Bootloader loads the kernel and its configs
4. Kernel loads, prepares the system / '**rootfs**' root filesystem and runs the initialization program / **init**
5. Init then loads system services (networking, GUI, etc.) and other startup programs.
6. System ready to use

Bootloader is the smart guy 