# 🧠 What “Swap” Means

Swap space is disk (or memory-compressed) space used as **virtual memory** when your system’s RAM is full.

When physical RAM is exhausted, Linux temporarily moves inactive memory pages to swap — allowing the system to keep running instead of killing processes.

---

## ⚙️ 1. **Swap Partition**
**Oldest and most traditional method.**
### 🧩 Description:
A dedicated disk partition, formatted specifically for swap usage.

### 🖥️ Setup:
`sudo mkswap /dev/sda2 sudo swapon /dev/sda2`

To make it permanent:
`echo '/dev/sda2 none swap sw 0 0' | sudo tee -a /etc/fstab`

### ✅ Pros:
- Fast (no filesystem overhead).
- Reliable — doesn’t depend on any filesystem being mounted.
- Recommended for servers or systems with predictable partitioning.

### ❌ Cons:
- Inflexible: size cannot easily be changed without repartitioning.
- Consumes fixed space, even if unused.

---

## 📄 2. **Swap File**
**Modern and flexible alternative.**

### 🧩 Description:
A regular file inside a mounted filesystem, marked for swap usage.

### 🖥️ Setup:
`sudo fallocate -l 2G /swapfile sudo chmod 600 /swapfile sudo mkswap /swapfile sudo swapon /swapfile`

Make it permanent:
`echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab`

### ✅ Pros:
- Very flexible — easy to resize or delete.
- Ideal for virtual machines or laptops.
- Doesn’t require a separate partition.

### ❌ Cons:
- Slightly slower due to filesystem overhead.
- Depends on the filesystem — can’t be used before root (`/`) is mounted.

---

## 🌀 3. **ZRam (Compressed RAM Swap)**
**A newer, RAM-based approach using compression instead of disk I/O.**

### 🧩 Description:
Creates a **compressed block device in RAM** and uses it as swap.  So instead of swapping to disk, data is **compressed in memory**, giving you more “effective” RAM.

### 🖥️ Setup (temporary):
`sudo modprobe zram echo lz4 > /sys/block/zram0/comp_algorithm echo 2G > /sys/block/zram0/disksize mkswap /dev/zram0 swapon /dev/zram0`

Most modern distros (like Ubuntu, Fedora) use `zram-generator` or `systemd-zram` for automatic management.

### ✅ Pros:
- Extremely fast (no disk access).
- Great for low-memory systems or VMs.
- Reduces wear on SSDs since it avoids disk swapping.

### ❌ Cons:
- Uses part of RAM for compressed data (less total physical RAM available).
- If RAM pressure is too high, can still lead to OOM (Out of Memory).

---

## 📊 Summary Table
|Type|Storage Medium|Speed|Flexibility|Common Use|
|---|---|---|---|---|
|**Swap Partition**|Disk/SSD|⚙️ Medium|❌ Low|Servers, legacy systems|
|**Swap File**|Disk/SSD|⚙️ Medium|✅ High|Desktops, laptops, VMs|
|**ZRam Swap**|RAM (compressed)|🚀 Very Fast|✅ Moderate|Low-RAM systems, mobile, live OS|

---

## 🧩 Bonus: Hybrid Swap Setup
Modern systems often combine methods:
- **ZRam** for short-term memory bursts.
- **Disk swap file/partition** as a fallback.

For example:
`swapon --show`
might display both `/dev/zram0` and `/swapfile`.

---

## 📚 LPIC Context

Understanding the **difference between swap types**, their **configuration**, and **use cases** is key for:

- LPIC-1 Objective 102.5
- Real-world server tuning
- Memory troubleshooting (`vmstat`, `free`, `top`)