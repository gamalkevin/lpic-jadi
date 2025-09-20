# Useful Personal Commands

During my LPIC learning, I came across these useful commands. I'll write them down here, so they will stick with me in the long term.

## 🪙 `cat`
`cat` is primarily used for concatenating and displaying file contents (mainly text/logs). Its name is an abbreviation of "**concatenate.**"

## 🪙 Piping `dmesg` with `grep`

```
$ sudo dmesg | grep -i 'amd'
```
While learning about `dmesg` with Jadi, I tested running the command to see what my system has to say.  

Turns out, it brought a long wall of text—duh—making it quite hard to read. 

Suddenly it came up to me to chain the command with `grep`, a tool commonly used for scouring logs. Sure enough, the output was nicely filtered, having only specified to list  `amd`. It even colorized the keyword!

By the way, `-i` option means case-insensitive search, so it's searching, for example, `AMD` and `amd`. 


