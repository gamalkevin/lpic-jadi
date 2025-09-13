# 005 - Part 2 of 3 - Determine and Configure Hardware, Kernel 🌽
So we just learned about hardware in previous video, now we're diving into Linux system. Before we start on learning from the video, let's learn about...

## Linux Kernel
*Jadi doesn't cover this in the video, so I figured I get an info on this matter on my own.*

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

###  Core Concepts / Terms in sysfs
#### **a) kobject (Kernel Object)**
- **Definition:** The basic unit in `sysfs`.
- Every kernel object that the sysfs exposes is a `kobject`.
- Contains:
    - Name
    - Type
    - Attributes (`kobj_attribute`)
    - Relationships (parent/child)
- **Example:** A device on USB bus is represented as a kobject in `/sys/bus/usb/devices/`.

#### **b) Attributes**
- Each `kobject` can have **attributes**.
- These are **files in sysfs**, usually readable (sometimes writable) for user space.
- Attributes expose:
    - Device properties
    - Driver settings
    - Status information
        
**Example:**
`$ cat /sys/class/net/eth0/address # Returns MAC address of eth0`

Here, `address` is a **kobject attribute file**.

#### **c) Classes and Class Devices**
- **Class:** Groups of devices with similar functionality.
    - Example: `block` (disks), `net` (network interfaces), `input` (keyboard, mouse)
- **Class devices:** Specific instances of that class.
    - Example: `eth0` in `net` class, `/sys/class/net/eth0/`

So classes → devices → attributes.

#### **d) Bus**
- **Definition:** Represents how devices connect to the system (physical/logical).
- Examples: `usb`, `pci`, `platform`    
- Each bus has its **directory in sysfs**, e.g., `/sys/bus/usb/devices/`.

**Why useful?** Lets you see all devices attached to a particular bus.

#### **e) Device**
- Represents a **hardware or virtual device**.
- Connected to a bus and has **attributes**.
- Usually a subdirectory under `/sys/bus/<busname>/devices/` or `/sys/class/<class>/<device>/`

**Example:**
`/sys/class/net/eth0/ /sys/class/block/sda/`

#### **f) Driver**
- Represents a **kernel driver bound to a device**
- You can see which driver manages which device    
- Often writable: you can bind/unbind devices to drivers via sysfs

**Example:**
`/sys/bus/pci/drivers/`

#### **g) Linking structure**
- Devices and drivers are linked:
    - **Device → Driver** (`driver` file points to the driver)
    - **Driver → Device** (`bind/unbind` files)
- The kernel maintains these **relationships dynamically**.

### **3️⃣ How sysfs is used**

### **Inspecting hardware**

`ls /sys/class/net/        # List network interfaces cat /sys/class/net/eth0/speed   # Get link speed`

### **Tuning kernel parameters**

Some attributes are writable:

`echo 1 > /sys/class/leds/input3::capslock/brightness`

This turns on the Caps Lock LED.

### **Device management**

`echo -n "0000:00:1f.2" > /sys/bus/pci/drivers/ahci/unbind`

This detaches a device from its driver.

---

## **4️⃣ Quick Map of /sys**

`/sys  ├─ /class      → group devices by functionality (network, block, input)  ├─ /bus        → group devices by bus type (pci, usb, platform)  ├─ /devices    → device tree hierarchy  ├─ /block      → block devices (disks)  ├─ /firmware   → firmware interfaces  └─ ... (many others)`

**Tip:** Always think: **Classes = type**, **Bus = connection**, **Device = instance**, **Driver = handler**.

---

## **5️⃣ Key Takeaways**

1. sysfs is **not on disk**, it’s a **live interface** generated by kernel.
    
2. Allows **inspection and limited control** of kernel objects.
    
3. Core entities:
    
    - **kobject** → basic object
        
    - **attribute** → file exposing a property
        
    - **class** → group of similar devices
        
    - **bus** → physical/logical connection type
        
    - **device** → hardware instance
        
    - **driver** → software handler
        
4. Mostly **read-only**, some files are writable for control/tuning.