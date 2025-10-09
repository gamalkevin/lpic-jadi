# 016 - 102.3 - Managing Shared Libraries

## 102.3: Manage shared libraries

|**Weight**|**1**|
|---|---|
|**Description**|Candidates should be able to determine the shared libraries that executable programs depend on and install them when necessary.|

**Key Knowledge Areas:**
- Identify shared libraries.
- Identify the typical locations of system libraries.
- Load shared libraries.

**Partial list of files, terms, utilities:**  
`ldd`, `ldconfig`, `/etc/ld.so.conf`, `LD_LIBRARY_PATH`

---

## Understanding Libraries
### **📘 Terms & Commands**
#### 1. **Shared Library Basics**
- Shared libraries are files that contain code and data that **multiple programs can use simultaneously**, reducing redundancy.
- Common extensions:
    - `.so` → shared object (e.g., `libm.so`)
    - `.so.6` → versioned shared object
    - `.a` → static archive library (not shared at runtime)

Location examples:
`/lib /usr/lib /usr/local/lib`

#### 2. **View Linked Libraries**
Use `ldd` to list shared libraries an executable depends on:
`ldd /bin/ls`

Output example:
`linux-vdso.so.1 =>  (0x00007fffa2dfe000) libselinux.so.1 => /lib64/libselinux.so.1 (0x00007f38e8c23000) libc.so.6 => /lib64/libc.so.6 (0x00007f38e8860000)`

> ⚠️ **Be careful:** Don’t run `ldd` on untrusted binaries — it executes code internally.

---

#### 3. **Library Search Paths**

The system looks for libraries in several places:

1. **Default paths:** `/lib`, `/usr/lib`
    
2. **Configured paths:** `/etc/ld.so.conf` and `/etc/ld.so.conf.d/`
    
3. **Environment variable:** `LD_LIBRARY_PATH`
    

Example:

`export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH`

---

#### 4. **Update the Cache**

After installing new libraries manually:

`sudo ldconfig`

This command updates the cache file `/etc/ld.so.cache`.

To see current entries:

`ldconfig -p | less`

---

#### 5. **Check Shared Library Dependencies of Running Process**

You can use:

`cat /proc/<PID>/maps`

or

`lsof -p <PID> | grep '\.so'`

---

#### 6. **Static vs Dynamic Linking**

|Type|Description|File Example|
|---|---|---|
|Static|Code is copied into each executable. Bigger size.|`.a`|
|Dynamic (Shared)|Linked at runtime, smaller executables.|`.so`|

---

### **🧩 Files & Directories**

|Path|Purpose|
|---|---|
|`/lib`, `/usr/lib`, `/usr/local/lib`|Default library directories|
|`/etc/ld.so.conf`|Main config file for library paths|
|`/etc/ld.so.conf.d/`|Directory for additional configs|
|`/etc/ld.so.cache`|Cache built by `ldconfig`|