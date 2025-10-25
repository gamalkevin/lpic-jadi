# Linux Virtualization

## 102.6: Linux as a virtualization guest

|                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Weight**                                               | 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Description**                                          | Candidates should understand the implications of virtualization and cloud computing on a Linux guest system.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Key Knowledge Areas:**                                 | - Understand the general concept of virtual machines and containers<br>- Understand common elements virtual machines in an IaaS cloud, such as computing instances, block storage and networking<br>- Understand unique properties of a Linux system which have to changed when a system is cloned or used as a template<br>- Understand how system images are used to deploy virtual machines, cloud instances and containers<br>- Understand Linux extensions which integrate Linux with a virtualization product<br>- Awareness of cloud-init |
| **Partial list of the used files, terms and utilities:** | - Virtual machine<br>- Linux container<br>- Application container<br>- Guest drivers<br>- SSH host keys<br>- D-Bus machine id                                                                                                                                                                                                                                                                                                                                                                                                                    |

---

## Virtualization and Hypervisor
### 🧩 1️⃣ What is **Virtualization**?

**Virtualization** is the process of creating a **virtual (software-based)** version of something that behaves like the real (physical) version.

In our context:
> 🧠 **Virtualization = making one physical computer act like many computers.**

Each “virtual” computer is called a **Virtual Machine (VM)** — and it has:
- Its own **CPU**, **RAM**, **disk**, **network card** (all virtual)
- Its own **operating system** (guest)
- Runs isolated from the host and other guests

💡 The main idea:  
You can run **multiple OSes simultaneously** on one physical machine, without them interfering with each other.

### 🏗️ 2️⃣ What is a **Hypervisor**?

A **hypervisor** (sometimes called a **Virtual Machine Monitor** or **VMM**) is the software (or firmware) that creates, runs, and manages virtual machines.

It’s the “manager” that:
- Allocates CPU, memory, and devices to each VM
- Keeps them isolated
- Handles I/O between the host and guests

Without a hypervisor, virtualization can’t exist.

---

## Type I vs Type II Hypervisor

| Type                    | Description                                            | Example                                                                   | Analogy                                                                                         |
| ----------------------- | ------------------------------------------------------ | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Type 1 (Bare-metal)** | Runs **directly on the hardware**, no host OS beneath. | VMware ESXi, Microsoft Hyper-V (Server Core), Xen, KVM (with some debate) | Like a dedicated driver who controls the car directly — no middleman.                           |
| **Type 2 (Hosted)**     | Runs **on top of a host OS** as an application.        | VirtualBox, VMware Workstation, QEMU (without KVM)                        | Like driving a remote-controlled car through another system — easier to start, slightly slower. |

A hypervisor **enables** virtualization,  
but virtualization is the **overall technique** or outcome.

### 🧠 Visual breakdown
```
Type 1 (Bare-metal):
 ---------------------------
| Hardware (CPU, RAM, Disk) |
|---------------------------|
| Hypervisor                |
|---------------------------|
| Guest OS 1 | Guest OS 2   |
 ---------------------------

Type 2 (Hosted):
 ---------------------------
| Hardware (CPU, RAM, Disk) |
|---------------------------|
| Host OS (e.g. Linux)      |
|---------------------------|
| Hypervisor App (VirtualBox)|
|---------------------------|
| Guest OS 1 | Guest OS 2   |
 ---------------------------

```

## 🧱 Virtual Machines (VMs) vs 🧩 Containers

| Feature               | **Virtual Machine (VM)**                                                                      | **Container**                                                                      |
| --------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Definition**        | A _virtualized_ instance of an entire operating system (including its own kernel).            | A _lightweight, isolated_ process running on the same OS kernel.                   |
| **Isolation level**   | Full hardware-level isolation — each VM runs its own OS and kernel.                           | Process-level isolation — all containers share the same host kernel.               |
| **Boot time**         | Slower (tens of seconds to minutes).                                                          | Fast (milliseconds to seconds).                                                    |
| **Resource usage**    | Heavy — includes OS overhead (RAM, storage).                                                  | Light — no extra OS per container.                                                 |
| **Typical use cases** | Running multiple OSes on one machine, legacy app isolation, complete environment replication. | Microservices, scalable applications, CI/CD pipelines, and cloud-native workloads. |
| **Management tool**   | Managed by a **hypervisor** (e.g., KVM, VMware, VirtualBox).                                  | Managed by a **container engine** (e.g., Docker, Podman, containerd).              |
| **Portability**       | Harder — VM images are large and OS-specific.                                                 | Easier — container images are small and standardized across environments.          |
| **Security boundary** | Stronger isolation — full OS boundary.                                                        | Weaker isolation — relies on kernel namespaces & cgroups.                          |
| **Performance**       | Slightly lower due to virtualization overhead.                                                | Near-native performance.                                                           |

### 🧩 Example Analogy

Imagine your computer is a **building**:

- Each **VM** is like a _separate apartment_ with its own plumbing, walls, and power — heavy but private.
- Each **container** is like a _room_ in the same apartment — lighter, faster, but sharing the same foundation.

### 🖥️ Real-world Usage

| Purpose             | VMs                                          | Containers                                |
| ------------------- | -------------------------------------------- | ----------------------------------------- |
| **Cloud providers** | AWS EC2, Azure VMs, Google Compute Engine    | AWS ECS, Kubernetes, Docker Swarm         |
| **Developers**      | Test multiple OSes (Ubuntu, Fedora, Windows) | Develop microservices quickly             |
| **Enterprises**     | Legacy app support, strong isolation         | Cloud-native apps, auto-scaling workloads |
| **CI/CD Pipelines** | Often use nested VMs for secure builds       | Commonly use containers for speed         |

### ⚙️ Can they work together?

Yes — in fact, **they often do**:

- Cloud servers run on **VMs**, and _inside_ them, you may run **containers**.
- This “VM → container” layering offers flexibility, isolation, and scalability.

---

## Iaas (Infrastructure as a Service)