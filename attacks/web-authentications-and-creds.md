# Web Authentication and Credentials

This document lists a bunch of attacks and vulnerabilities that can exist in authentication methods and credential types.

## Credentials

### JSON Web Tokens (JWT)

You can spot these by looking for cookies/params with a format of `eyXXXXXXXXXX.XXXXXXXX.XXXXXXXXXX`. The sections are Base64- and URL-encoded.

#### `none` Signature Algorithm

 - When specifying the algorithm with which to generate the signature of the JWT, `none` is a valid option (per the spec).
 - In some broken implementations, this may not be restricted.
 - Therefore, you can change the token, such that maybe it says your username is that of someone else. Then, you change the `alg` type to `none`, and remove the signature section.
 - Send the new token, and (if the server is vulnerable) you will now be "logged in as" the other user.
 - Servers can restrict access to the `none` type, to mitigate against this attack.