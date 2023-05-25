# Broken Function Level Authorization (OWASP 5 TOP 10)

Exploitation requires the attacker to send legitimate API calls to the API endpoint that they should not have access to. These endpoints might be exposed to anonymous users or regular, non-privileged users. It’s easier to discover these flaws in APIs since APIs are more structured, and the way to access certain functions is more predictable (e.g., replacing the HTTP method from GET to PUT, or changing the “users” string in the URL to "admins").

## Is the API Vulnerable?

The best way to find broken function level authorization issues is to perform deep analysis of the authorization mechanism, while keeping in mind the user hierarchy, different roles or groups in the application, and asking the following questions:

- Can a regular user access administrative endpoints?
- Can a user perform sensitive actions (e.g., creation, modification, or erasure) that they should not have access to by simply changing the HTTP method (e.g., from GET to DELETE)?
- Can a user from group X access a function that should be exposed only to users from group Y, by simply guessing the endpoint URL and parameters (e.g., /api/v1/users/export_all)?

Don’t assume that an API endpoint is regular or administrative only based on the URL path.

While developers might choose to expose most of the administrative endpoints under a specific relative path, like api/admins, it’s very common to find these administrative endpoints under other relative paths together with regular endpoints, like api/users.

## Prevention

- The enforcement mechanism(s) should deny all access by default, requiring explicit grants to specific roles for access to every function.
- Review your API endpoints against function level authorization flaws, while keeping in mind the business logic of the application and groups hierarchy.
- Make sure that all of your administrative controllers inherit from an administrative abstract controller that implements authorization checks based on the user’s group/role.
- Make sure that administrative functions inside a regular controller implements authorization checks based on the user’s group and role.
