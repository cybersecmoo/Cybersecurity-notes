# Kerberos Things #

## How Kerberos Works ##

- Client sends a `KRB_AS_REQ` to the Key Distribution Centre (KDC), which in a domain is the Domain Controller
  - Request contains a timestamp encrypted with the user's password hash
- If the timestamp is decryptable by the KDC (i.e. the password hash used by the client was correct), the KDC sends a `KRB_AS_REP` response containing the Ticket-Granting Ticket (TGT)
- When the client wishes to access a service, they send a `KRB_TGS_REQ` request for that particular service to the KDC, containing the service SPN and the TGT.
- The KDC checks the TGT, and responds with a `KRB_TGS_REP`, containing a TGS ticket (itself containing info about the user's groups etc.)
- Client then sends a `KRB_AP_REQ` request to the service, containing the TGS.
- Service checks that the user is authorised to access it, using the info contained in the TGS, and responds with a `KRB_AP_REP`.

## How NTLM Works ##



## Attacks ##

### Kerberoasting ###

- The `KRB_TGS_REP` response comprises two items:
  - The TGS, encrypted with the secret of the requested service
  - The session key, encrypted with the user's secret.
- The KDC does *not* check access rights.
  - Therefore, any user/client may request for *any* service via `KRB_AS_REQ`
- An attacker can make a `KRB_AS_REQ` request using the SPN (Service Principal Name) of the service in question.
  - If that SPN is found in the domain, the KDC will respond appropriately.
- So an attacker can request an arbitrary SPN, and use the response to try to brute-force the password for the service account.
- Normally, services run under special machine accounts, with long pseudorandom passwords.
  - So it would normally be inviable to attack these accounts in this way.
- However, sometimes human users are given SPNs
  - So if an attacker requests the SPN for a human user, they will receive the usual `KRB_AS_REP` response, and can use that to try to bruteforce the human's password.
  - Human passwords are normally less complex than a machine account's would be.
- This attack can be mitigated by avoiding giving SPNs to human users
  - i.e. run all services under specialised machine accounts
  - Potentially, use the Managed Service Accounts feature, which automatically sets complex, rotating passwords.

#### Attack Notes ####

- The `GetUserSPNs.py` Impacket script can be used to identify users with SPNs
- Similarly, you can use the following LDAP filter: `&(objectCategory=person)(objectClass=user)(servicePrincipalName=*)`

### AS_REP-Roasting ###

- By default, the client has to authenticate with the KDC when sending the KRB_AS_REQ.
  - The aforementioned encrypted timestamp stuff
- But it is possible to turn this off (on a per-user basis)
- An attacker can then send a `KRB_AS_REQ` request to the KDC for that user, and receive a TGT without needing the target user's password.
- Note, however, that if the DC requires you to authenticate in order to make requests, i.e. it does not permit unauthenticated LDAP requests, then it can be difficult to get the information about which users are vulnerable.
  - In this case, OSINT and bruteforcing may help.
  - OSINT to produce a list of possible user names, and then bruteforcing through those to see if you can AS_REP-roast any of them.
- Once you have found a user without pre-auth, you can request a TGT for them.
- `GetNPUsers.py` in the Impacket suite can conduct this attack.
- The TGT can then be passed to John or Hashcat for offline cracking.

### Silver Tickets ###

- TGS tickets are encrypted using the hash of the service in question.
- If, therefore, we have the hash (or, indeed, password) for a given service that we would not normall have access to, then wqe can forge a TGS that *will* provide us access
- This TGS is called the Silver Ticket.
- To generate and use a Silver Ticket, you can use `ticketer.py` from Impacket's scripts: 
  - `ticketer.py -nthash [NT Hash] -domain-sid [Domain SID] -domain [Domain] -spn [Service SPN] [user]`
  - `export KRB5CCNAME=/path/to/user.ccache`
  - Pass the service name to other Impacket scripts using `-k`, e.g. `-k DESKTOP1.lab.local` 

### Golden Tickets ###

- If we have the hash for the `krbtgt` account (the hash which is used to encrypt TGTs), then we can forge a TGT
- For example, we could forge a TGT that says we are Domain Admins.
- This TGT is called the Golden Ticket.
- To generate and use a Golden Ticket, you can use `ticketer.py` from Impacket's scripts: 
  - `ticketer.py -nthash [NT Hash] -domain-sid [Domain SID] -domain [Domain] [user]`
  - `export KRB5CCNAME=/path/to/user.ccache`
  - Pass the DC domain name to other Impacket scripts using `-k`, e.g. `-k DC1.lab.local` 

### Pass-the-Hash ###

- 