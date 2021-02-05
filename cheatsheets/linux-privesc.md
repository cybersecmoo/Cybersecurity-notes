# Linux PrivEsc 

- After enumerating the OS version and distro, it's worth looking for known privesc vulnerabilities
- `less` has a command `:e` which can be used to examine a new file. If you're using a tool such as `journalctl`, that uses `less` as a pager, and get `journalctl` to read a line which is longer than the terminal width (i.e. gets paged) then you get popped into `less`. If you were running `journalctl` as `root` (via misconfigured `sudo` permissions) then this `less` instance will also be running as `root`, and can use `:e` to view files as `root`.
  - Equally in this scenario you could just run `!/bin/bash` from `journalctl` to open a root shell. The key here is that the pager must be open.
- [GTFOBins](https://gtfobins.github.io) has a great list of ways in which to use various common linux binaries to privesc or escape a restricted shell.
- 