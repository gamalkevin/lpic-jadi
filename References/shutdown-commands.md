# 🔌 Shutdown & Reboot Commands in Linux

| **Action**                  | **SysV (old)**                | **systemd (modern)**                   | **Notes**                          |
| --------------------------- | ----------------------------- | -------------------------------------- | ---------------------------------- |
| Shutdown (immediate)        | `shutdown -h now`             | `systemctl poweroff`                   | `-h` = halt & power off            |
| Shutdown (in X minutes)     | `shutdown -h +10 "Msg"`       | `systemctl poweroff --message="Msg"`   | SysV version lets you delay easily |
| Reboot (immediate)          | `shutdown -r now` OR `reboot` | `systemctl reboot`                     | `-r` = reboot                      |
| Reboot (in X minutes)       | `shutdown -r +5 "Msg"`        | `systemctl reboot --message="Msg"`     | Useful to warn users               |
| Halt system (CPU stop)      | `halt`                        | `systemctl halt`                       | May not power off hardware         |
| Power off system            | `poweroff`                    | `systemctl poweroff`                   | Explicit “power off”               |
| Single-user / rescue mode   | `init 1`                      | `systemctl rescue`                     | Minimal services for recovery      |
| Cancel a scheduled shutdown | `shutdown -c`                 | N/A (just don’t schedule with systemd) | Sends warning to logged-in users   |

---

## 🧠 LPIC Tips
- SysV `shutdown` can **delay shutdowns** → often tested.
- `halt`, `poweroff`, `reboot` are **shortcuts** to `shutdown`.
- systemd is now default, but LPIC wants you to know **both styles**.
- Always remember: **Runlevel 0 = poweroff**, **Runlevel 6 = reboot**.