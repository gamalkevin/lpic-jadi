# 005 - Part 2 of 3 - Determine and Configure Hardware, Kernel 🌽
So we just learned about hardware in previous video, now we're diving into Linux system. Before we start on learning from the video, let's learn about...

## 'Kernel vs User' Space
*Jadi doesn't cover this in the video, so I figured I get an info on this matter on my own, so I don't have to look elsewhere for references.*

### Two “worlds” in Linux memory
When the OS runs, memory is split into **two regions**:
1. **Kernel space**
    - Where the Linux **kernel code** runs.
    - Has **full access** to hardware (CPU, RAM, disks, devices).
    - Dangerous if something goes wrong (can crash the whole system).
2. **User space**
    - Where **applications/programs** run (like Firefox, bash, Python).
    - Restricted: can’t directly touch hardware.
    - Must ask the kernel (via **system calls**) if they need hardware access.

**💡 Quick analogy:**
- **Kernel:** The “brain” of the OS, controlling all operations.
- **User programs:** Workers asking the brain for resources.
- **Hardware:** Tools that the workers want to use.

### Kernel Definition
The **kernel** is the **core part of an operating system (OS)**. It acts as a bridge between:
1. **Hardware** (CPU, memory, disk, network, devices)    
2. **Software** (applications and user programs)

**In simple terms:** The kernel is the “manager” that lets software use hardware safely and efficiently.

With this definition, we might confuse it with **firmware**, but these two are not the same. Firmware is **low-level software embedded in hardware** while kernel is part of the **operating system**, not hardware.

Let's see how both of them interact: 
1. **Power on** → firmware (BIOS/UEFI) runs, initializes hardware.
2. **Firmware** → loads the bootloader.
3. **Bootloader** → loads the kernel into memory.
4. **Kernel** → takes over, initializes the OS, exposes devices via sysfs, `/proc`, `/dev`, etc.


## Sysfs
`Sysfs` is a **pseudo, virtual filesystem** in Linux.
- It is **mounted at `/sys`**.
- Purpose: Expose **kernel objects, attributes, and device info** to user space.
- Think of it as a **window into the kernel**, letting us inspect and sometimes configure devices, buses, and drivers.

It might look like a bunch of files, where we can `cd` and browse, but the files are not like common files. Sysfs is _readable and sometimes writable_, but it does **not exist on disk**; the kernel generates it dynamically.

### Analogy
- **Kernel** = the manager who controls all the hardware tools.
- **Sysfs** = a glass window into the manager’s office, with a few buttons we're allowed to press.
- When we press a button (*write to sysfs*), the manager acts — possibly reconfiguring or telling hardware to change behavior.

*A more complete sysfs explanation can be accessed [here](References/sysfs-explained.md).*

## Userspace `/dev` / `udev`
