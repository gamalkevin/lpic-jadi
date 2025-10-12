# 010 - 101.3 - Part 2 of 2 - Runlevels and targets, Stopping the System and Informing
 Part 2 of **LPIC Topic 101.3: Change runlevels / boot targets and shutdown or reboot system**.

*Find shutdown & reboot commands cheat-sheet [here](/References/shutdown-commands.md).*

---

In the early days, computers are shared and centralized, usually in universities or research centers. Multiple users can log in into the same computer, even remotely, at the same time. 

So what happens when an admin wants to restart or shutdown the computer for maintenance purpose? Surely they can't just pull the plug on the computer. People are still working on the system and can lose their WIPs.

Also shutting down does not mean 'just turn off'. Admins need the system to:
1. Tell users and processes that shutdown is happening.
2. Stop services cleanly (databases, networking, daemons).
3. Unmount filesystems safely.
4. Power off or reboot the hardware.

## Classic SysV init commands 📑
- **`shutdown`**  
    The “proper” way.
    - `shutdown -h now` → halt & power off immediately.
    - `shutdown -r +5 "System rebooting!"` → reboot in 5 minutes with message.

- **`halt`** → stops the CPU. On modern systems, it’s the same as `poweroff`.    
- **`poweroff`** → turns the machine off.
- **`reboot`** → reboots the machine.

All these ultimately call **`/sbin/init`** to switch to runlevel 0 (halt) or 6 (reboot).

## systemd equivalents 📑
- **`systemctl poweroff`** → shutdown & power off.
- **`systemctl reboot`** → reboot.
- **`systemctl halt`** → stop all processes, halt CPU (might not power off).
- **`systemctl suspend` / `hibernate`** → sleep/hibernate.
- **`systemctl rescue`** → go to rescue (single-user) mode.
- 

These are more consistent because everything goes through `systemd`.

## Runlevel/Target mapping for shutdown 🔄
- **Runlevel 0** = shutdown → `poweroff.target`.
- **Runlevel 6** = reboot → `reboot.target`.

## Delayed shutdown with `systemd`
As you might have noticed above, systemd doesn't seem to have the same delayed shutdown the way SysVinit does. 

However, `systemd` ships a **compatibility wrapper** / symbolic links: 
- It will recognize classic `shutdown` command with its options, 
- e.g.: `shutdown -h +10` (Shutdown in 10 minutes). 
- It is run under `systemd` instead of `init`.

But does systemd has this ability built-in? The answer is **yes**.

- The modern systemd way:
```
systemctl poweroff --when="now +10 minutes"
systemctl reboot --when="17:30"
```    
→ `--when` lets user specify relative or absolute times. 

*This feature was added in newer systemd versions, so on some older distros you might not see it.*

## Notifying users 📣
101.3 isn’t just about the mechanics of `shutdown` and `reboot`, but also about:
- **Notifying users** before a shutdown/reboot.
- **Managing logins** (so new users don’t get surprised or lose work).
- Understanding the **tools to communicate with users** on a multi-user system.

So these tools Jadi mentioned are useful to notify users of what admins going to do with the system:
- **`wall` (write all)**  
    Sends a broadcast message to all logged-in users.
    `echo "System will reboot in 5 minutes" | wall`
    → Everyone sees it on their terminal.
- **`mesg`**  
    Controls whether other users can send messages to your terminal.
    - `mesg n` → deny messages.
    - `mesg y` → allow messages.  
        → Affects tools like `write` or `wall`.
- **`notd`**  
    This is less common, but on some systems it’s a notification daemon (like desktop pop-ups).  
    LPIC mostly focuses on **text-based TTY notifications** (`wall`, `mesg`, `write`).