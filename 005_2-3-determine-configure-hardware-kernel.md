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

- **`udev`** is the **user-space manager** for `/dev/`.  
- `/dev/` is where Linux keeps **device files** (like `/dev/sda` for disks, `/dev/ttyUSB0` for serial adapters).
- Without device files, programs can’t talk to hardware.

### Why `udev` is needed?
- Old Linux systems had a **static `/dev/`**: thousands of files for devices we didn’t even have.
- `udev` makes `/dev/` **dynamic**: 
    - When we plug in a device → `udev` creates the right file.
    - When we remove it → `udev` deletes it.

### How it works
1. We plug in hardware (say, USB drive).
2. **Kernel detects** it and sends an event.
3. **`udev` listens** to these events.
4. `udev` uses **rules** to decide:
    - Create device file (`/dev/sdb`).
    - Give it the right permissions.
    - Maybe add a symlink (`/dev/disk/by-uuid/...`).
5. Now programs can use it safely.

### Quick Example
Plug in a USB stick → `/dev/sdb` appears.  
Unplug it → `/dev/sdb` disappears.

That’s **udev in action**.

✅ **Summary in one line:** `udev` is the **userspace service that manages `/dev/` dynamically**, so device files appear/disappear automatically when hardware changes.

## D-Bus
Think of it like Linux’s **internal messaging system**.

### What is D-Bus?

- **D-Bus** = **Desktop Bus / message bus**.
- It’s a **communication system** that lets **programs talk to each other**.
- Runs in **user space**.
- Think of it as a **mailroom or post office inside Linux**.

### Why we need it
- Programs often need to **coordinate or share info**:
    - Example: `NetworkManager` tells the system a Wi-Fi connection is active.
    - Example: A media player asks the sound system to play music.
- Without D-Bus, programs would have to directly connect to each other, which is messy.

### How it works
- **Programs (clients)** send messages to the **D-Bus daemon**.
- D-Bus daemon delivers messages to the **right program (service)**.

**Example:**
1. You plug in a USB drive.
2. Kernel triggers `udev` → device node created in `/dev/`.
3. D-Bus notifies your **file manager** that a new device is available.
4. File manager shows a popup for the USB stick.

### Types of D-Bus
1. **System bus** → for system-wide messages (hardware events, power, network).
    - Runs as root, everyone can listen with permission.
2. **Session bus** → per user session (desktop apps talking to each other).

### Super-simple analogy

- **D-Bus** = Linux’s **internal post office**.
- Programs drop messages into the bus → bus delivers to the correct recipient.
- Makes Linux programs **cooperative** without hard-wiring them together.

✅ **In one line:**  D-Bus is a **user-space messaging system** that lets programs and services on Linux **talk to each other safely and efficiently**.

---

Now, on to the [last part](./006_3-3-determine-configure-hardware-commands.md) on this topic.