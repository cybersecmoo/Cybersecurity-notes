# External Recon

## Nmap

- Quick scan: `nmap -v -sC -sS -oA [filename] [host]`
- Full scan: `nmap -v -p- -oA [filename] [host]`

## NSLookup 

- Set DNS server:
  - `server [host]`
- Lookup host
  - `[host]`
  - Works for reverse lookups, too

## LDAPSearch

- `-x` sets simple (non-binding) authentication
- `-h` sets the LDAP server
- `-s` is the scope (`sub`, `base`, `one`, or `children`)
- Anything put at the end will be interpreted as a list of attributes to return

### LDAP Filters of Note

- `&(objectCategory=person)(objectClass=user)(servicePrincipalName=*)` finds human users with SPNs (for Kerberoasting attacks)
- `&(objectCategory=person)(objectClass=user)` returns human users
- `(objectClass=group)` returns groups