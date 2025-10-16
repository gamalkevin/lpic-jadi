# Useful Personal Commands

During my LPIC learning, I came across these useful commands. I'll write them down here, so they will stick with me in the long term.

## 🪙 `cat`
`cat` is primarily used for concatenating and displaying file contents (mainly text/logs). Its name is an abbreviation of "**concatenate.**"

### Common Options
Basic options
- `-n`, `--number`: Number all output lines, starting at 1.
- `-b`, `--number-nonblank`: Number non-blank output lines, starting at 1. Overrides `-n`.
- `-s`, `--squeeze-blank`: Squeeze multiple consecutive blank lines into a single blank line.
- `-u`: Forces unbuffered output, writing each byte as it is read. Most useful when dealing with real-time devices. 

Options for showing special characters:
- `-E`, `--show-ends`: Display a `$` at the end of each line.
- `-T`, `--show-tabs`: Display tab characters as `^I`.
- `-v`, `--show-nonprinting`: Display non-printing characters (except for tabs and newlines) using `^` and `M-` notation.
- `-e`: Equivalent to `-vE`. Displays non-printing characters and a `$` at the end of each line.
- `-t`: Equivalent to `-vT`. Displays non-printing characters and tabs as `^I`.
- `-A`, `--show-all`: Equivalent to `-vET`. Displays all non-printing characters, line endings, and tabs. 

---

## 🪙 `grep`
One of the most useful tool to search text.

### Common options for `grep`
Case and pattern matching
- **`-i`**, `--ignore-case`: Ignore case distinctions, treating "Pattern" and "pattern" as the same.
- **`-w`**, `--word-regexp`: Match the pattern only when it forms a whole word, not a substring.
- **`-x`**, `--line-regexp`: Select matches that are entire lines.
- **`-E`**, `--extended-regexp`: Interpret the pattern as an extended regular expression (ERE), which enables more powerful features like `|` for "OR".
- **`-F`**, `--fixed-strings`: Interpret the pattern as a list of fixed strings, not regular expressions. This is faster for searching literal text.
- **`-f file`**: Obtain patterns from a file, with one pattern per line.
- **`-e pattern`**: Use multiple patterns for searching. 

Output control
- **`-v`**, `--invert-match`: Invert the search to display all lines that **do not** match the pattern.
- **`-n`**, `--line-number`: Prefix each line of output with the corresponding line number from the input file.
- **`-c`**, `--count`: Suppress normal output and instead print a count of the number of matching lines.
- **`-l`**, `--files-with-matches`: Print only the names of files that contain at least one match.
- **`-L`**, `--files-without-match`: Print only the names of files that contain **no** matching lines.
- **`-o`**, `--only-matching`: Print only the matched part of a line, with each match on a new line.
- **`-q`**, `--quiet`, `--silent`: Suppress all output. `grep` will exit with a status code of 0 if a match was found and 1 if not, which is useful for scripting.
- **`-s`**, `--no-messages`: Suppress error messages about non-existent or unreadable files.
- **`--color=auto`**: Highlight the matching text in the output. 

Context control
- **`-A n`**, `--after-context=n`: Print `n` lines of trailing context **after** each match.
- **`-B n`**, `--before-context=n`: Print `n` lines of leading context **before** each match.
- **`-C n`**, `--context=n`: Print `n` lines of context **before and after** each match. 

File and directory search

- **`-r`**, `--recursive`: Search recursively through all files in a directory and its subdirectories.
- **`-R`**, `--dereference-recursive`: Similar to `-r`, but follows all symbolic links.
- **`-h`**, `--no-filename`: Suppress the prefixing of file names on output when searching multiple files.
- **`-H`**, `--with-filename`: Always print the file name for each match.
- **`-I`**: Process a binary file as if it did not contain matching data.

---

## 🪙 Piping `dmesg` with `grep`
```
$ sudo dmesg | grep -i 'amd'
```
While learning about `dmesg` with Jadi, I tested running the command to see what my system has to say.  

Turns out, it brought a long wall of text—duh—making it quite hard to read. 

Suddenly it came up to me to chain the command with `grep`, a tool commonly used for scouring logs. Sure enough, the output was nicely filtered, having only specified to list  `amd`. It even colorized the keyword!

---

## 🪙 `file`

Very useful to see what type of files is being accessed. 

### Case example
I'm looking for a text file to edit, but since the file has no visible file extensions like `.txt` or `.sh`, it is useful to look what type of file it is beforehand:
```
file /etc/default/grub

/etc/default/grub: ASCII text
```


Another example, I want to make sure that I'm looking for a binary executable:
```
file /usr/lib/firefox/firefox

firefox: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 4.4.0, BuildID[sha1]=2400217cbd14b7b71a44599075a3170d4cefdaad, stripped
```

---

