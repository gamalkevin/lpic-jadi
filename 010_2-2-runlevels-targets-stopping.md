# 010 - 101.3 - Part 2 of 2 - Runlevels and targets, Stopping the System and Informing
 Part 2 of **LPIC Topic 101.3: Change runlevels / boot targets and shutdown or reboot system**.

In the early days, computers are shared and centralized, usually in universities or research centers. Multiple users can log in into the same computer, even remotely, at the same time. 

So what happens when an admin wants to restart or shutdown the computer for maintenance purpose? Surely they can't just pull the plug on the computer. People are still working on the system and can lose their WIPs.

Also shutting down does not mean 'just turn off'. Admins need the system to:
1. Tell users and processes that shutdown is happening.
2. Stop services cleanly (databases, networking, daemons).
3. Unmount filesystems safely.
4. Power off or reboot the hardware.

## 📑 Classic SysV init commands
- **`shutdown`**  
    The “proper” way.
    - `shutdown -h now` → halt & power off immediately.
    - `shutdown -r +5 "System rebooting!"` → reboot in 5 minutes with message.

- **`halt`** → stops the CPU. On modern systems, it’s the same as `poweroff`.    
- **`poweroff`** → turns the machine off.
- **`reboot`** → reboots the machine.

All these ultimately call **`/sbin/init`** to switch to runlevel 0 (halt) or 6 (reboot).

## 📑 systemd equivalents
- **`systemctl poweroff`** → shutdown & power off.
- **`systemctl reboot`** → reboot.
- **`systemctl halt`** → stop all processes, halt CPU (might not power off).
- **`systemctl suspend` / `hibernate`** → sleep/hibernate.
- **`systemctl rescue`** → go to rescue (single-user) mode.

These are more consistent because everything goes through `systemd`.

## 🔄 Runlevel/Target mapping for shutdown
- **Runlevel 0** = shutdown → `poweroff.target`.
- **Runlevel 6** = reboot → `reboot.target`.