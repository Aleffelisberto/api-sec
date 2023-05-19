# Broken Object Level Authorization (OWASP-1 TOP 10)

APIs tend to expose endpoints that handle object id's, creating a wide attack surface Level Access Control issue. Object-level authorization checks should be considered in every function that accesses a data source using input from the user.

## What does that mean?

If a user finds an endpoint, say, a user number in a browser URL, and changes that number to another and the API returns informatino that the users should not have access to, then we are dealing with a broken object-level authorization. So this vulnerability occurs when an API gives a user access to something they shouldn't have access to ordinarily.

Broken object-level authorizations (BOLA) is a specific type of an Insecure Direct Object Reference (IDOR).

IDOR PoC: https://youtu.be/TRDyvgkBcUs

The reason for the vulnerability existing as a result of simply changing an ID parameter is that the API does not check whether a user owns a resource before that user can do anything to it.

## Remediation

This can be mitigated by ensuring that an identifier is combined with a check that any user has permission to access this resource. This can be as straightforward as an if statement, or using a middleware. Obfuscating and randomizing user id's is helpful, where possible; implementing a user authorization mechanism that rigorously checks for user access to a resource at every point where user input is possible. Using unit tests to ensure that code is automatically tested before being deployed.

## Well-known events

Back in 2010, AT&T suffered by an IDOR that exposed the details of at least over 100.000 owners. It exposed the email address of the owners, as well as the ICC-IDs. By sending a request to AT&T together with an ICC-ID, the server would respond with the corresponding email address. As the ICC-IDs can be enumarated by looking at just a few IDs, this attack could be fully automatised allowing a considerable data leak.
