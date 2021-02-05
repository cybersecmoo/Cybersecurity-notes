# Obfuscation/AV-Evasion

- Some AVs may not check mounted SMB shares etc. or at least will not quarantine files on such shares.
  - Presumably because they don't "belong", as it were, to the client.
- 

## Covering Tracks

### Linux

- `unset HISTFILE` will tell Bash that there is no history file
- Generally (if `$HISTCONTROL` includes `ignoreboth` or `ignorespace`), a command starting with a space (e.g. ` whoami`) will not be logged to the history