# Broken Access Control (OWASP 1 TOP 10 2021)

Access control enforces policy such that users cannot act outside of their intended permissions. Failures tipically lead to unauthorized information disclosure, modification, or destruction of all data or performing a business function outside the user's limits. Common access control vulns include:

- Violation of the principle of least privilege or deny by default, where access should only be granted for particular capabilities, roles, or users, but is abailable to anyone.

- Bypassing access control checks by modifying the URL (parameter tampering or force browsing), internal application state, or the HTML Page, or by using attack tool modifying API requests.

- Permitting viewing or editing someone else's account, by providing its unique identifier (insecure direct object references)

- Accessing API with missing access controls for POST, PUT and DELETE.

- Elevation of privilege. Acting as a user without being logged in or acting as an admin when logged in as user.

- Metadata manipulation, such as replaying or tampering with a JSON Web Token access control token, or a cookie or hidden field manipulated to elevate privileges or abusing JWT invalidation.

- CORS misconfiguration allows API access from unauthorized/untrusted origins.

- Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a stardad user.

## Prevention

- Except for public resources, deny by default.

- Implement access control mechanisms once and re-use them throughout the app, including minimizing Cross-Origin Resource Sharing (CORS) usage.

- Model access controls should enforce record ownership rather than accepting that the user can create, read update, or delete any record.

- Unique application business limit requirements should be enforced by domain models.

- Disable web server directory listing and ensure file metadata and backup files are not present with web roots.

- Log access control failures, alert admins when appropriate (e.g., repeated failures)

- Rate limit API and controller access to minimize the harm from automated attack tooling.

- Stateful session identifiers should be invalidated on the server after logout. Stateless JWT tokens should rather be short-lived so that the windows of opportunity for an attacker is minimized. For longer lived JWTs it's highly recommended to follow the OAuth standards to revoke access.
