# Injections


## SQL Injection 

### SQLMap

- Useful if you don't have to worry about monitoring

### Useful test strings

- `'username--` - this can help you work out if the server is vulnerable (if it throws an error about the query, then it might be SQLi-vulnerable). Note the double-dash, which will comment out the remainder of the query into which this is injected.

### Useful Injection Strings

#### Retrieving data

- `' OR 1=1--` can return all contents of the queried table

#### Bypassing Logic

- `administrator'--` provided as a username (with "administrator" replaced with the appropriate account username) could bypass a password-check (if the check is applied directly against the database).

#### Gathering Info on the Database Itself

- You can run something like `SELECT @@version` (for MySQL; similar for other DBMSes) to get the database version.
- You can list the tables and columns in the database:
  - Non-Oracle: `information_schema.tables`, `information_schema.columns`
  - Oracle: `all_tables`, `all_tab_columns`

### Useful Concepts

#### UNION

 - `UNION` merges the output of two `SELECT` statements
   - Note that Oracle requires a valid table to be specified for every `SELECT` query; you can use the built-in `DUAL` table for this.
 - If the server is grabbing (unhashed) passwords from the DB, then you might be able to use `UNION` to merge the output from the original `SELECT` (i.e. the real password) with the output of a `SELECT` statement that you can craft. If you craft your `SELECT` such that it returns, say, "password123" as the `password` field, that might be included in the equality-check when the server checks to see if your provided password equals that of the given user. You then just have to enter "password123" as the password, and you're in.
   - You will need the passwords to be stored unhashed, or know what the hashing algorithm is (in which case they need to be unsalted). Otherwise you won't be able to guarantee what your given password will hash to.
   - You may need to know the username of the user (and include it in your payload), otherwise the server might chuck a wobbly. This is because it would be logging you in as a user which doesn't exist. 
 - If you `UNION SELECT table_schema, table_name from information_schema.table`, you may be returned a list of table names.
 - Make sure the `SELECT`s either side of the `UNION` have the same number of columns. You may need to add columns (e.g. `SELECT [...], 1`). 
 - If you want to get, say, two columns, but the target response only has one column, then you can concatenate the contents of multiple columns.
 - The overall process is:
   - Identify the injection point
   - Identify the number of columns in the target response; can do this either via `' UNION SELECT NULL,NULL,...--` or via `' ORDER BY 1--`, `' ORDER BY 2--`, ...
   - Find which column returns a string data type: `' UNION SELECT 'a',NULL,...`
   - Construct the exploit payload and Bob is your uncle.


## NoSQL Injection

### Useful Test Strings

- `username[$ne]=bob`: Simple check that a given username in any document is not equal to "bob". Handy just to check that the target is vulnerable.
- 