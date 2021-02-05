# Cracking

- `hashcat --stdout [basic password list] -r [ruleset] -r [ruleset 2] ...` will enrich a basic wordlist by applying the given rules. Good for making a small list with variations on common passwords, for use in online attacks.
- `crackmapexec smb --pas-pol` can enumerate password policies via SMB; useful for further refining your wordlists (e.g. restricting passwords in the list to those with a length that is valid in the password policy)
  - you may need to append `-u '' -p ''` for a null authentication attack, if the anonymous authentication did not work
  - Both will fail if the server is properly configured though (i.e. prevents null- and anonymous-authentication)
  - Similar things can be done via `rpcclient`, too.