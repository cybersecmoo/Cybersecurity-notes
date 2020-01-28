# Persistence #

## Windows ##

- You can gain persistence by replacing the accessibility executables (e.g. `sethc.exe`)
  - These are available from secure desktops (i.e. login screens, UAC popups, etc.)
  - These are run as SYSTEM (hooray!)
- Image File Execution Options (IFEO) are essentially Registry keys which can be set to allow a given executable to be run during/after another executable
  - The intention is to facilitate attaching debuggers to processes
  - However, as an attacker we can set our malware as the "debugger"
  - Attaching our malware in this way, to run when an automatically-started service (e.g. userinit.exe) starts, we can gain persistence