# XSS:
## Reflected XSS:
In simple terms, Reflected XSS means the input is directly *reflected* onto the screen.
Suppose a website has a search function which receives the user-supplied search term in a URL parameter:
`https://insecure-website.com/search?term=gift`
The application echoes the supplied search term in the response to this URL:
`<p>You searched for: gift</p>`
So, if no proper input sanitization occurs, a user may be able to inject a payload and do some bad things.

Reflected XSS isn't normally that severe.

### Resources:
https://portswigger.net/web-security/cross-site-scripting/reflected