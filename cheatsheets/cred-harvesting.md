# Credential Harvesting Methods

## Windows

- By creating a new PasswordFilter, you can access plaintext credentials whenever a user tries to change their password
  - These are DLLs
  - They expose three functions: `InitializeChangeNotify`, `PasswordFilter`, `PasswordChangeNotify`
  - Place the DLL into System32
  - Add the DLL name (without the .dll extension) to the HKLM\SYSTEM\CurrentControlSet\Control\Lsa\Notification Packages registry key
  - Restart the machine
  - You can do a bunch of stuff (including uploading creds to a server, etc.) from within these DLLs.