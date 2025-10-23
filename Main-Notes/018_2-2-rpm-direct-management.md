# Part 2 of 2: RPM Package Management 

## Using RPM to directly manage packages

*Access `yum` and `rpm` cheat-sheet [here](yum-zypper-cheatsheet.md).*

- Instead of the *GNU-Style* longform options (which sound natural and easy to remember) like `apt`'s `install` or `upgrade`, POSIX-Style shortform options are used in `rpm`:
	- ```
	  # rpm [-afgpqlsiv?]
	  ```
	  - Longforms still exist too.
- This is similar to `pacman`'s  commands *(think `sudo pacman -Syu` for updating the system)*.

## 🛄 Verifying Package ☑️
> **Verifying a package** checks the installed files against the information saved in the RPM database. It compares things like the file’s size, checksum, permissions, type, owner, and group.  
> If something doesn’t match, RPM will show the differences.  
> Files that weren’t installed (for example, skipped docs using `--excludedocs`) are ignored silently.


### 1️⃣ Using `rpm -V`
A sysadmin might check whether a package has been tampered or not with the command `# rpm -Vv {package name}`.

For example:
```
# rpm -Vv tmux
.M.......  g /run/tmux
.........    /usr/bin/tmux
.........    /usr/lib/tmpfiles.d/tmux.conf
.........    /usr/share/bash-completion/completions/tmux
.........    /usr/share/doc/packages/tmux
.........  d /usr/share/doc/packages/tmux/CHANGES
.........    /usr/share/licenses/tmux
.........  l /usr/share/licenses/tmux/COPYING
.........  d /usr/share/man/man1/tmux.1.gz
```
**Note:**
- `-V` (capitalized) means **verify**
- `-v` (smallcase) means verbose

You can see the installed files are listed with **periods preceding them**. A single period means **the test passed**.

Let's try modifying `/usr/bin/tmux` with a simple string and check again:
```
# echo "warden was compromised" > /usr/bin/tmux

# rpm -Vv tmux
.M.......  g /run/tmux
S.5....T.    /usr/bin/tmux
.........    /usr/lib/tmpfiles.d/tmux.conf
.........    /usr/share/bash-completion/completions/tmux
.........    /usr/share/doc/packages/tmux
.........  d /usr/share/doc/packages/tmux/CHANGES
.........    /usr/share/licenses/tmux
.........  l /usr/share/licenses/tmux/COPYING
.........  d /usr/share/man/man1/tmux.1.gz
```
You can see that the dots have changed. This is very useful to make sure the apps have been installed properly without any tampering activities.

Here's a complete explanation of the test failure codes[^1]:

| Code  | Meaning               | Description                                                         |
| ----- | --------------------- | ------------------------------------------------------------------- |
| **S** | Size differs          | The file size has changed.                                          |
| **M** | Mode differs          | File type or permissions differ.                                    |
| **5** | Digest differs        | The file checksum (formerly MD5) differs.                           |
| **D** | Device mismatch       | Device major/minor numbers don’t match.                             |
| **L** | Symlink path mismatch | [`readlink(2)`](https://linux.die.net/man/2/readlink) path differs. |
| **U** | User differs          | File ownership by user differs.                                     |
| **G** | Group differs         | File ownership by group differs.                                    |
| **T** | Time differs          | Modification time (`mtime`) differs.                                |
| **P** | Capabilities differ   | File capabilities differ.                                           |

### 2️⃣ Using `rpm -K`
This one checks the associated `.rpm` file as opposed to the installed files. Useful before installing.

Let's try it with `tmux`:
```
# rpm -Kv tmux-3.5a-2.2.x86_64.rpm
tmux-3.5a-2.2.x86_64.rpm:
    Header V3 RSA/SHA512 Signature, key ID 29b700a4: OK
    Header SHA256 digest: OK
    Header SHA1 digest: OK
    Payload SHA256 digest: OK
    V3 RSA/SHA512 Signature, key ID 29b700a4: OK
    MD5 digest: OK
```

## 🫗Extracting RPM files
### `rpm2cpio`

The `.cpio` is an archive format like `.zip`, `.rar`, or `.tar`. 
- We can use `rpm2cpio` to convert the `.rpm` files.
- The resulting `.cpio` files can then be extracted.

```
# rpm2cpio tmux-3.5a-2.2.x86_64.rpm > tmux.cpio

# cpio -idv < tmux.cpio
./usr/bin/tmux
./usr/lib/tmpfiles.d/tmux.conf
./usr/share/bash-completion/completions/tmux
./usr/share/doc/packages/tmux
./usr/share/doc/packages/tmux/CHANGES
./usr/share/licenses/tmux
./usr/share/licenses/tmux/COPYING
./usr/share/man/man1/tmux.1.gz
2555 blocks
```

[^1]: `rpm` [man page](https://linux.die.net/man/8/rpm#:~:text=S%20file%20Size%20differs%0AM%20Mode%20differs%20(includes%20permissions%20and%20file%20type)%0A5%20digest%20(formerly%20MD5%20sum)%20differs%0AD%20Device%20major/minor%20number%20mismatch%0AL%20readlink(2)%20path%20mismatch%0AU%20User%20ownership%20differs%0AG%20Group%20ownership%20differs%0AT%20mTime%20differs%0AP%20caPabilities%20differ).
