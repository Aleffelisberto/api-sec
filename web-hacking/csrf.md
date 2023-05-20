# Cross Site Request Forgery (CSRF)

Cross-Site Request Forgery (CSRF) is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker’s choosing. If the victim is a normal user, a successful CSRF attack can force the user to perform state changing requests like transferring funds, changing their email address, and so forth. If the victim is an administrative account, CSRF can compromise the entire web application.

An attacker can use CSRF to obtain the victim’s private data via a special form of the attack, known as login CSRF. The attacker forces a non-authenticated user to log in to an account the attacker controls. If the victim does not realize this, they may add personal data—such as credit card information—to the account. The attacker can then log back into the account to view this data, along with the victim’s activity history on the web application.

It’s sometimes possible to store the CSRF attack on the vulnerable site itself. Such vulnerabilities are called “stored CSRF flaws”. This can be accomplished by simply storing an IMG or IFRAME tag in a field that accepts HTML, or by a more complex cross-site scripting attack. If the attack can store a CSRF attack in the site, the severity of the attack is amplified. In particular, the likelihood is increased because the victim is more likely to view the page containing the attack than some random page on the Internet. The likelihood is also increased because the victim is sure to be authenticated to the site already.

## Remediations that do not work

### Using a secret cookie

Remember that all cookies, even the secret ones, will be submitted with every request. All authentication tokens will be submitted regardless of whether or not the end-user was tricked into submitting the request. Furthermore, session identifiers are simply used by the application container to associate the request with a specific session object. The session identifier does not verify that the end-user intended to submit the request.

### Only accepting POST requests

Applications can be developed to only accept POST requests for the execution of business logic. The misconception is that since the attacker cannot construct a malicious link, a CSRF attack cannot be executed. Unfortunately, this logic is incorrect. There are numerous methods in which an attacker can trick a victim into submitting a forged POST request, such as a simple form hosted in an attacker’s Website with hidden values. This form can be triggered automatically by JavaScript or can be triggered by the victim who thinks the form will do something else.

### Multi-Step Transactions

Multi-Step transactions are not an adequate prevention of CSRF. As long as an attacker can predict or deduce each step of the completed transaction, then CSRF is possible.

### URL Rewriting

This might be seen as a useful CSRF prevention technique as the attacker cannot guess the victim’s session ID. However, the user’s session ID is exposed in the URL. We don’t recommend fixing one security flaw by introducing another.

### HTTPS

HTTPS by itself does nothing to defend against CSRF.

However, HTTPS should be considered a prerequisite for any preventative measures to be trustworthy.

## Remediations that work

- Check if your framework has built-in CSRF protection and use it.
  - If framework does not have built-in CSRF protection, add CSRF tokens to all state changing requests (requests that cause actions on the site) and validate them on the backend
- For stateful software use the synchronizer token pattern
- For stateless software use double submit cookies
- Implement at least one migition from the list:
  - Consider SameSite Cookie Attribute for session cookies but be careful to NOT set a cookie specifically for a domain as that would introduce a security vulnerability that all subdomains of that domain share the cookie. This is particularly an issue when a subdomain has a CNAME to domains not in your control.
  - Consider implementing user interaction based protection for highly sensitive operations
  - Consider the use of custom request headers
  - Consider verifying the origin with standard headers
- Remember that any Cross-Site Scripting (XSS) can be used to defeat all CSRF mitigation techniques!
- Do not use GET requests for state changing operations.
  - If for any reason you do it, protect those resources against CSRF
