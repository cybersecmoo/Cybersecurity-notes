# Linux Recon

- Kernel/Distro Versions: `lsb_release -a` and `uname -a`
  - You can then look up on Google for known privilege escalation paths in these versions.
- Find SUID files: `find / -perm -u=s -type f 2>/dev/null`
- Find items owned by a group: `find / -group <group>`
- Find items owned by a user: `find / -user <user>`
- List current user's sudo permissions: `sudo -l`
  - Especially be on the lookout for applications which can execute user-provided code/commands (e.g. Vi). You could then use those to run commands as root.