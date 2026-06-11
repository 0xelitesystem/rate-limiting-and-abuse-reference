# Why rate limiting matters

A public endpoint with no limit on how often it can be called is exposed to anyone who decides to call it a lot. Without rate limiting, a single user, a buggy client, a scraper, or an attacker can flood your service with requests, exhausting its capacity, running up costs on anything metered, and overwhelming sensitive operations like login. Rate limiting is the control that caps how many requests a caller may make in a window of time, and it is among the most basic protections any service needs.

## The capacity problem

Every service has finite capacity, and unlimited request rates let that capacity be consumed by one source. A flood of requests, whether from a deliberate attack or a misbehaving client stuck in a loop, can slow or take down the service for everyone, because the resources spent handling the flood are not available for legitimate users. Rate limiting protects availability by ensuring no single caller can consume a disproportionate share of capacity. This is true even without malice: a client bug that retries forever can hammer an endpoint as effectively as an attacker, and a limit stops both.

## The cost problem

For any operation that costs money per request, unlimited rates are a direct financial risk. Calls to paid third-party services, sending messages, anything billed by usage, accrues cost with every request, and without a cap, a runaway client or an attacker can run that cost up without bound. The bill arrives regardless of whether the requests were legitimate. This is one of the most concrete reasons to rate limit: an unprotected metered endpoint is an open invitation to a large unexpected bill, and the cap is what bounds the worst case to something you can absorb.

## The abuse problem

Some endpoints are targets for abuse precisely because of what they do. Login endpoints invite attempts to guess credentials, since an attacker can try many passwords if nothing stops them. Signup and contact endpoints invite automated spam. Password reset and verification endpoints can be abused to flood people with messages. Without rate limiting, these endpoints can be hit rapidly and repeatedly, which is how credential-guessing and spam abuse work. Limiting the rate on these endpoints removes the ability to make the many rapid attempts that abuse depends on, turning a fast automated attack into a slow, impractical one.

## Set limits before you are forced to

The recurring lesson is that rate limits should be in place before an incident forces them, not added in a panic after a flood, a huge bill, or a credential-stuffing attack has already happened. The cost of setting reasonable limits up front is small; the cost of not having them when one of these problems arrives can be a downed service, a large bill, or a breach. Because the protection is cheap and the exposure without it is open-ended, rate limiting belongs on every public endpoint from the start, tuned tighter on the endpoints where the consequences of abuse are most severe, which the following pages cover.
