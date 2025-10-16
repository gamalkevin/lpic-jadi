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

# Understanding Libraries
In the early days, programs were written entirely from scratch. Nowadays, the same practice would take developers unreasonably long time to finish their programs, especially given how complex and massive the software development world has become.

Instead, devs can 'call' **libraries** to serve the basic/underlying features of their program. This will cut **development time**, as well as increasing reliability and predictability. When debugging, Devs can easily pinpoint which part of their program doesn't behave correctly. 

## 📦 1. What Are Libraries?

A **library** is a collection of precompiled code used by programs.  
They come in two types:

| Type               | Extension             | Loaded              | Description                                                                   |
| ------------------ | --------------------- | ------------------- | ----------------------------------------------------------------------------- |
| **Static library** | `.a`                  | At **compile time** | Code copied into the program binary. Bigger size, but self-contained.         |
| **Shared library** | `.so` (Shared Object) | At **runtime**      | Code loaded dynamically when program runs. Saves memory, smaller executables. |

👉 Example:

```
/lib/x86_64-linux-gnu/libc.so.6
```

This is the **GNU C library** (glibc), used by almost every Linux program.

📝 Note:
- Static library increases dependability, since everything is baked in a single binary. Kinda like laptops where they don't need separate peripherals. 
- Shared/dynamic library is externally loaded. Programs cannot run until the library is resolved. 

---

## ⚙️ 2. How Dynamic Linking Works

When you run a program:

1. The **executable** lists which shared libraries it depends on.
2. The **dynamic linker/loader** (`ld-linux.so`) loads them into memory.
3. The program starts running with those libraries available.

Let's try checking libraries required for `firefox`:

```
ldd /usr/lib/firefox/firefox
```

Example output:

```
linux-vdso.so.1 (0x00007fdbd5691000)
libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0x00007fdbd5200000)
libm.so.6 => /usr/lib/libm.so.6 (0x00007fdbd50f2000)
libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x00007fdbd5574000)
libc.so.6 => /usr/lib/libc.so.6 (0x00007fdbd4e00000)
/lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x00007fdbd5693000)
```

---

## 🧩 3. Library Search Paths

The system looks for shared libraries in these locations, in order:

1. Directories in **`/etc/ld.so.conf`**
2. Any `.conf` files inside **`/etc/ld.so.conf.d/`**
3. Default paths:
    ```
    /lib /usr/lib
    ```
4. Any paths defined in the **`LD_LIBRARY_PATH`** environment variable (temporary).

---

### 🔍 Example: Add a New Library Path

Suppose you installed a custom library in `/usr/local/lib`.

You can make it permanent:
```
echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/local.conf
sudo ldconfig
```

`ldconfig` updates the **library cache** file:
```
/etc/ld.so.cache
```

This cache speeds up lookup for dynamic linker.

---

## 🧰 4. Key Commands

| Command           | Purpose                                     |
| ----------------- | ------------------------------------------- |
| `ldd <binary>`    | Show which shared libraries a program needs |
| `ldconfig`        | Update the library cache and symbolic links |
| `ldconfig -p`     | Show all known libraries from cache         |
| `ldconfig -v`     | Verbose mode — see what it’s doing          |
| `LD_LIBRARY_PATH` | Temporary override of library search path   |

---

### Example:

```
export LD_LIBRARY_PATH=/opt/mylibs ./myprogram
```

This tells the system to also look for `.so` files inside `/opt/mylibs` **just for that session**.

---

## 🧪 5. Symbolic Links and Versions

Libraries are versioned to allow compatibility.

Example:
```
/lib/libm.so.6 -> libm-2.31.so
```

The symbolic link points to the actual library version.  
`ldconfig` helps maintain these links automatically.

---

## 🧠 **In Short**

| Concept                 | Description                            |
| ----------------------- | -------------------------------------- |
| **Static (.a)**         | Linked at compile time                 |
| **Shared (.so)**        | Linked at runtime                      |
| **ldd**                 | Lists dependencies                     |
| **ldconfig**            | Updates cache and symlinks             |
| **/etc/ld.so.conf(.d)** | Defines system-wide library paths      |
| **LD_LIBRARY_PATH**     | Temporary per-session path override    |
| **/etc/ld.so.cache**    | Compiled list of available shared libs |