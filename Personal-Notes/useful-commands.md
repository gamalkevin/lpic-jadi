# Useful Personal Commands

While learning, I came across these useful commands. I'll write them down here, so they will stick with me in the long term.

## Piping `dmesg` with `grep`

```
$ sudo dmesg | grep -i 'amd'
```
While learning about `dmesg` with Jadi, I tested running the command to see what my system has to say.  

Turns out, it brought a long wall of text—duh—, making it quite hard to read. 

Suddenly it came up to me to chain the command with `grep`, a plain-data searcher 