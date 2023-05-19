# Broken User Authentication (OWASP-2 TOP 10)

Broken User Authentication occurs when weakenesses in how an API authenticates users allow attackers to compromise accounts and make API requests as if they were a legitimate user.

**Autenthication**: Someone proving their identity to a computer system is called authentication. Authentication can happen in many ways but the traditional way is providing a username and password to prove identity digitally. New method of providing stronger authentication such as 2FA or MFA authentication have been created to provide more confidence in the identity of a computer user. Depending on the device used and the software running on that device, additional mechanisms may be available to validate identity claims.

If an API has authentication weaknesses, attackers can hijack existing user accounts and take any actions available to that API user. Since APIs are generally created for pragmmatic interaction e.g. not by humans, many of the protections that exist for mobile or browser interactions are not available, Additionally, if the weakness is global to the API, all user data is at risk.

## How to test

Attackers look for broken authentication vulns first by looking for APIs that allow 'login' or lack authentication completely. APIs that lack authenticatino can typically be found behind mobile clients and other uses where developers incorrectly believe only their client will call the API or that the API is somehow hidden.

For APIs which allow 'login' by accepting a username and password combination to provide an authentication token, an attacker only has to automate the attack and have a list of usernames and passwords.

For APIs which require an authenticatino token, there are multiple known cryptographic and configuration weaknesses for API token mechanisms such as JWTs. Simply using an authentication token and removing 'login' from an API does not mean the API is not vulnerable to broken user authentication.

## Remediation

For APIs with logins, credential stuffing and passowrd spraying attacks generate a large volume of traffic and will create a spike in failed login attempts. For defenders who monitor traffic and have telemetry data on failed login attempts, these attacks can be spotted easily. Additionaly, the existing infrastructure can be utilized to block, de-authenticate or otherwise take action against the attacker. A common response to discovering an automated attack is to block the offending IP address for a period of time.

For concerns around the strength of API authentication token, implementing secure software development practices including developer training can help prevent the creatino of weak tokens.

- Where possible, using 2FA or MF2 authentication to prevent bruce force
- Do not ship or deploy with any default credentials, particularly for admin users.
- Implement weak-password checks, such as testing new or changed passwords against a list of the [top 10000 worst passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords)
- Align password length, complexity and rotation policies with [NIST 800-3 B's guidelines in section 5.1.1 for Memorized Secrets](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecret) or other modern, evidence based password policies.
- Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes.
- Limit or increasingly delay failed login attempts. Log all failures and alert admins when credential stuffing, brute force, or other attacks are detected.
- Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

[JSON Web Token Best Current Practices RFC](https://datatracker.ietf.org/doc/rfc8725/)
