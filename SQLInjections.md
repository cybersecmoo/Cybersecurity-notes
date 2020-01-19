# SQL Injections

## SQLMap

- Useful if you don't have to worry about monitoring

## Useful test strings

- `'username` - this can help you work out if the server is vulnerable (if it throws an error about the query, then it might be SQLi-vulnerable)

## Useful Concepts

### UNION

 - `UNION` merges the output of two `SELECT` statements
 - If the server is grabbing (unhashed) passwords from the DB, then you might be able to use `UNION` to merge the output from the original `SELECT` (i.e. the real password) with the output of a `SELECT` statement that you can craft. If you craft your `SELECT` such that it returns, say, "password123" as the `password` field, that might be included in the equality-check when the server checks to see if your provided password equals that of the given user. You then just have to enter "password123" as the password, and you're in.
   - You will need the passwords to be stored unhashed, or know what the hashing algorithm is (in which case they need to be unsalted). Otherwise you won't be able to guarantee what your given password will hash to.
   - You will may need to know the username of the user (and include it in your payload), otherwise the server might chuck a wobbly. This is because it would be logging you in as a user which doesn't exist. 
 - If you `UNION SELECT table_schema, table_name from information_schema.table`, you may be returned a list of table names.
 - Make sure the `SELECT`s either side of the `UNION` have the same number of columns. You may need to add columns (e.g. `SELECT [...], 1`).    