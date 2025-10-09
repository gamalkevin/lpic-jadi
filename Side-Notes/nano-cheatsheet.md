# `nano` cheatsheet

Here’s a practical **cheat sheet** that balances speed, clarity, and real-world use — no fluff, just the essentials.

---

## 🧭 Basic Navigation

|Command|Action|
|---|---|
|`Ctrl + A`|Move to **beginning** of line|
|`Ctrl + E`|Move to **end** of line|
|`Ctrl + Y`|Scroll **up one page**|
|`Ctrl + V`|Scroll **down one page**|
|`Ctrl + _`|**Go to line** number|
|`Ctrl + Space`|Move forward one word|
|`Alt + Space`|Move backward one word|
|`Ctrl + ↑ / ↓`|Move cursor **up/down** paragraph (depends on terminal)|

---

## ✍️ Editing Basics

|Command|Action|
|---|---|
|`Ctrl + O`|**Write Out (Save)**|
|`Ctrl + X`|**Exit** nano|
|`Ctrl + R`|**Read file** into current buffer|
|`Ctrl + K`|**Cut** current line|
|`Ctrl + U`|**Uncut (Paste)** line|
|`Alt + 6`|**Copy** line|
|`Ctrl + J`|**Justify** (reflow paragraph)|
|`Ctrl + T`|**Spell check** (if available)|

---

## 🔍 Searching & Replacing

|Command|Action|
|---|---|
|`Ctrl + W`|**Where Is (Search)**|
|`Alt + W`|**Repeat last search**|
|`Ctrl + \`|**Replace** text|
|`Alt + R`|**Regex search** (toggle)|

---

## ⚙️ File Management

|Command|Action|
|---|---|
|`Ctrl + O` → Enter|Save changes|
|`Ctrl + X`|Exit nano|
|`Ctrl + R`|Insert another file at cursor|
|`Alt + F`|Display **full file path**|

---

## 🌈 Marking & Selecting

|Command|Action|
|---|---|
|`Ctrl + ^`|**Start/End selection**|
|`Ctrl + K`|Cut selected text|
|`Ctrl + U`|Paste at cursor|
|`Alt + 6`|Copy selected text|

---

## 🧩 Indentation & Formatting

|Command|Action|
|---|---|
|`Alt + }`|**Indent** selected lines|
|`Alt + {`|**Unindent** selected lines|
|`Ctrl + J`|**Justify** (reflow paragraph)|
|`Alt + J`|Justify entire buffer|

---

## 🧠 Useful Toggles

|Command|Toggle|
|---|---|
|`Alt + M`|Mouse support|
|`Alt + X`|Help text (shortcut list)|
|`Alt + P`|Whitespace display|
|`Alt + L`|Soft line wrapping|
|`Alt + N`|Line numbers|
|`Alt + D`|DOS/Mac file format view|

---

## 💀 Emergency Recovery

|Command|Action|
|---|---|
|`Ctrl + C`|Show cursor position|
|`Ctrl + G`|Help screen|
|`Ctrl + X`|Exit (prompt if unsaved)|
|`Ctrl + O`, then `Ctrl + C`|Cancel write-out|
|`Ctrl + T`|Run syntax checker or formatter if configured|

---

## ⚡ Pro Tips

- Run `nano -c filename` → shows **line numbers** always.
- Run `nano -m` → **enable mouse** support.
- Run `nano +5 filename` → opens file **at line 5**.
- Use `~/.nanorc` to configure syntax highlighting, tab size, etc.

Example:
`set linenumbers set tabsize 4 set softwrap include "/usr/share/nano/*.nanorc"`

---