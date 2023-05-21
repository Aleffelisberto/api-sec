# Lack of resources and Rate limiting (OWASP 4 TOP 10)

Lack of resource and rate limiting occurs when an API fails to sufficiently protect itself from overuse or abuse which negatively impacts availability. This issue is usually thought to be caused by malicious actors purposefully trying to consume all the resources of an API. However, less nefarious instances of this can also occur. Consider an API used by multiple businesses in a business to business situation. One of the businesses using the API may make an excessive, though not malicious, number of requests and cause other businesses to receive slow or no responses to their API calls. In either the malicious or mistaken case, properly configured resource and rate limiting can keep an API available to its clients.

Continuing the online retailer example, an attacker notices that the profile page for customers includes an optional avatar to personalize the customer experience. The profile page also includes the functionality to upload an image for your avatar. The attacker makes an API call including an excessively large image file which consumes all the resources of the API as it attempts to resize the image to the proper dimensions. As resources are consumed by this attack, the API responses get increasingly slow until the API becomes unavailable. With a simple large file upload, the attacker has taken the API offline. This is an application-level denial of service (DOS) attack.

## Importance

An API is created to allow for the exchange of data between programs in an automated and performant fashion. By failing to establish resource and rate limiting controls, APIs are at risk of violating one of the 3 central principles of information security:

### Availability

Considering the fact that the API was created to exchange data for a business reason, downtime or degraded performance of an API represents a risk to the business. If the API is directly related to revenue generating activities, downtime can mean loss of revenue, violations of contractual obligations and potential legal action.

## Rate limits

There are several places where resource and rate limits should be considered.

### Networking Layer

request size should be limited to maximum size. For our online retailer example, a simple network control on the maximum size of any HTTPS request could prevent the DOS attack

### Single-Client Request

Such protections can prevent a single client from 'hogging' the API at the expense of other clients. These limits may be imposed at a load balancer, an API gateway, a WAF, a reverse proxy or other infrastructure which handles the APIs requests.

### API itself

Also, the API itself needs to protect itself from attack by imposing limits on submission of data as well as providing data to clients. For example, an API which does not paginate data sent to the client may allow an attacker to create so many data items that getting a list of them will consume all the APIs available resources. Yet another way to cause a denial of service (DOS) attack.

It is important to note that while denial of service attacks are generally conducted by malicious actors, a poorly programmed client may innocently create enough traffic to degrade performance or availability of an API. So, even APIs that are between trusted partners are not safe from these risks.

## Is the API vulnerable?

An API is vulnerable if at least one of the following limits is missing or set inappropriately (e.g., too low/high):

- Execution timeouts
- Max allocable memory
- Number of file descriptors
- Number of processes
- Request payload size (e.g., uploads)
- Number of requests per client/resource
- Number of records per page to return in a single request response

## Testing

Many automated dynamic API scanners (DAST tools) will conduct a mild version of fuzzing as the test for potential injection vulnerabilities and may discover a lack of resource or rate limits. Quality testing, especially performance testing, is likely to find areas which need limiting as well as provide information on where to set limits to adequately protect an API. Specialized HTTP/API fuzzing tools also exist to find APIs which are susceptible to DOS attack or particularly resource intensive.

## Prevention

- Docker makes it easy to limit memory, CPU, number of restarts, file descriptors, and processes.
- Implement a limit on how often a client can call the API within a defined timeframe.
- Notify the client when the limit is exceeded by providing the limit number and the time at which the limit will be reset.
- Add proper server-side validation for query string and request body parameters, specifically the one that controls the number of records to be returned in the response.
- Define and enforce maximum size of data on all incoming parameters and payloads such as maximum length for strings and maximum number of elements in arrays.
