# Windows Recon

## Impacket Scripts

- `GetNPUsers`: Lists users which do not require kerberos pre-auth, and dumps their hashes
- `secretsdump`: dumps hashes (requires something like `DCSync` permissions)

## Built-in Recon Aids

- List listening ports and such: `netstat` 
- List env vars: `set`
- System overview: `systeminfo`
- Get service info: `sc`, or `Get-Service` on PowerShell
  - List all services: `sc queryex type=service state=all`
- List applied patches: `wmic qfe get Caption,Description,HotFixID,InstalledOn` or `Get-HotFix` on powershell
- List installed drivers: `driverquery`
- List user accounts: `wmic useraccount get /ALL /format:csv`

## Volume Shadow Copy Service (VSS) ##

- This service allows you to backup whole volumes (i.e. lettered drives)
- It can be interacted with via an admin command `vssadmin`
- `vssadmin list volumes` shows you the applicable volumes
- By viewing information about a volume, you can see where it is stored (somewhere like `\\GLOBALROOT\...`)
- From there, you can start listing files stored in the shadow volume, and can view the contents of anything you find interesting

## Alternate Streams

- If you find a file that is empty, but you feel like there *should* be data in it, check alternate streams.
  - via `SMB`, you can use `allinfo [filename]` to enumerate the streams (among other things)
  - You can then `get` on a specific stream: `get [filename]:[stream]`