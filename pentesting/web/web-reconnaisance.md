# Web Reconnaisance

Recon of web apps and websites.

## Spidering

- It is worth doing a mixture of manual and (guided) automated spidering
- You may lean particularly heavily towards manual if you suspect monitoring/throttling is in place on requests. Also, if you suspect the site may be using a lot of quite niche/unique naming schemes.
- You'd lean more heavily towards automated if there is no monitoring in place, or you're really pushed for time.
- When spidering, you're looking for things like:
  - admin interfaces/consoles
  - error/debug pages
  - login/authentication flows
  - Forgot/reset password flows
  - Naming conventions (e.g. `/users/{id}/posts/{post_id}/{view|edit|delete}`)
  - File extensions (`.asp`, `.aspx`, `.php`, etc.)

## Parameters and Cookies

You might look for:

- Parameter naming schemes (and what they betray about the workings of the server code).
- Parameter value formats/types. Are they numbers (IDs, maybe)? Do they look like file paths (if so, can you do a path traversal)?
- Cookies
  - Are they HTTP-only? If not, can you exploit that?
  - Are they secure?
  - Are sessions/tokens being stored in cookies? If so, can anything be gleaned from those?

## Headers

- Does the server reveal anything in the `Server` header?
  - Software versions etc.
  - These can be faked, but it's worth a look

## HTTP Methods

- Try using different request methods on pages
  - e.g. try doing a `POST` on a path that the website would normally `GET` on.
  - This might reveal debug functionality
  - Or it might return a default error message/page (if the devs weren't expecting this kind of thing, for instance).
  - This can be done with cURL, or by sending a request from Burp's Proxy, to its Repeater tab, and then repeating it, varying the request method each time.
  - You can see what methods are possible against a route by running an HTTP `OPTIONS` request against it.

## Enumerating Directories and Files ##

- You can use Dirbuster to automatically scrape directories and files, using a word list.
- If you need to have a reduced footprint, it can be worth reducing the number of threads dirbuster uses, and creating a custom wordlist tailored to the app you're targeting.