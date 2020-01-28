# UNIX Hints and Tips #

## Covering Tracks ##

- `unset HISTFILE` will tell Bash that there is no history file
- Generally (if `$HISTCONTROL` includes `ignoreboth` or `ignorespace`), a command starting with a space (e.g. ` whoami`) will not be logged to the history

## Privilege Escalation ##

- After enumerating the OS version and distro, it's worth looking for known privesc vulnerabilities