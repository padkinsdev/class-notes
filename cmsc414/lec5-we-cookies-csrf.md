# Web Intro, Cookies, Cross Site Request Forgery

### Intro
- GET requests generally include relevant information in the URL, whereas POST requests contain pertinent data in the request body
- Cookies are named after the term "magic cookie", which is a packet of data that a program receives and sends back unchanged
- Cookies include an expiration field, a path field, and a domain field to designate when the cookie should expire, what paths on a website the cookie is applicable to, and what domains (or subdomains) the cookie should be used on
- Session cookies (or tokens) keep users logged in across multiple requests

### Cross site request forgery (CSRF/XSRF)
- Each time a request is made, the user's browser automatically attaches relevant cookies. This can be taken advantage of by tricking the user into making unintended requests
- Essentially, stealing a user's session token (cookie) can allow an attacker to impersonate that user
- Getting a user to click a malicious link is an easy way to take advantage of their "logged-in" status to perform actions, e.g. if a user goes to the attacker's website and the website sends a fetch request to a legitimate domain, cookies for that legitimate domain will be automatically attached in the fetch request and used to authenticate the user
- Embedding JavaScript into, say, an ad, can allow for execution of the code to make a request to a different website

#### CSRF defenses
- These are implemented by the server, not the browser

##### CSRF tokens
- The idea: Requests from a page are not accepted unless some additional secret is attached
- A CSRF token is a secret value provided by the server to the user. The user must attach the same value in the request for the server to accept the request
- CSRF tokens cannot be sent to the server in a cookie
- Usually these are only valid for one or two requests in order to be ephemeral
- The server must maintain a mapping of CSRF tokens to sessions
- Essentially the session cookie is paired with the CSRF token to establish request legitimacy

##### Referer validation
- The idea is to track where requests are coming from, using the referer field of the request
- Rejects any requests with untrusted or suspicious referer headers
- One common issue is the removal of referer fields by operating systems and browsers for privacy reasons

### Cross site scripting (XSS)
- Injecting malicious JavaScript into a webpage
- Protecting against this involves enforcing a `Same-Origin` policy, preventing one domain from interfering with another's activities
- To satisfy `Same-Origin`, the domain, port, and protocol must match
- `Same-Origin` is enforced by the browser, preventing requests from one domain from being sent to a different domain
- JavaScript runs with the origin of the page that loaded it, presenting a limitation to this
- Websites can fetch and display images from other origins, but can only know the saze and dimensions of the image, not manipulate it
- CORS (Cross-Origin Resource Sharing) allows limited sharing of information between origins

#### XSS Injection
- XSS consists of stored XSS and reflected XSS
- Stored XSS consists of JS being stored in the content of a page, and is thus executed when the page is loaded (e.g. putting JS in your Facebook page)
- **Stored XSS is a server-side vulnerability!**
- Reflected XSS involves a victim inputting JS into a request, and the content is reflected in the server response
- An example of reflected XSS implementation is getting a victim to click a link that has injected JS in the URL, and upon the server response being displayed, the JS is executed
- Another implementation (reflected XSS) is the embedding of an iframe (1 px by 1 px) which sends the malicious request

***Reflected XSS causes an effect on the client's side, whereas CSRF causes an effect on the server side***

### Server-side defenses
- Protection against XSS and CSRF involves sanitizing user input and/or adding escapes to all potentially dangerous characters. This is done with trusted libraries.
- The bottom line is to ensure that user input is treated as data, not HTML