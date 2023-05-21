# Excessive Data Exposure (OWASP 3 TOP 10)

It occurs when an API responds with additional data which is expected to be filtered or ignored by the client. This is particularly bad when the extra data includes elements that are sensitive or covered by a regulatory requirement.

## How does this happen?

Two unfortunate assumptions can be made during API software development. First, a developer might assume that only the company's clients will talk to the API and believe the client will ignore the extra data. Regrettably, attackers won't confine themselves to only the official client and will choose to use the extra data. Secondly, developers can use programming functions to turn internal API data into API responses automatically. While this provides a productivity boost to the dev teams, it can also lead to extra data being sent in a response.

## Importance

It's never a good look to expose sensitive data, even more so when there are privacy or regulatory requirements for that data to be protected. Fundamentally, you have to know how your APIs are responding to secure them appropriately. Even if the data isn't considered sensitive today, it may be tomorrow. Plus, why send data that isn't truly needed? By removing all extraneous data from responses, you can drive down the data transferred and make both the API and its clients more efficient.

While a single request with sensitive data may not be an existential problem, the same assumptions that lead to that single vulnerable response were likely used throughout the API and attackers know this. Once an instance of excessive data exposure is found, more will be looked for across all the available methods of an API. Like chumming the water, a smaller issue can lead to complete data compromise.

## Remediations

Unfortunately for defenders, excessive data exposure can be hard to detect, especially on an already deployed API since the extra data is part of a normal request. Reviewing source code for use of functions which automatically convert internal API data to API responses can help but is language and framework dependent. Additionally, not all SAST tools will be able to alert when sensitive data is included in an API response.

A strong secure SDLC program which includes API security specific training in an important method to reduce the instances of excessive data exposure in APIs. By informing developers of the importance of only sending the required data in responses and the false assumption of client filtering, the vulnerability may be avoided altogether. While a proactive approach is preferred, having confidence by measuring the effectiveness of such training is vital.

One way to measure the effectiveness of training is to test the running API prior to deploying to production. Robust API Security testing tools can spot sensitive data in responses and flag those API endpoints for review. By adding API security testing to your CI/CD pipeline, gaps in training can be found early and often.

Additionally, while a single request of a vulnerable API endpoint will appear to be 'normal' traffic, automated attacks will have multiple requests within a short period of time typically from a single client. Such data scraping will be visible to API-specific runtime monitoring. If the runtime monitoring includes deep enough inspection, sensitive data in API responses will be detected and provide alerts for corrective action.
