# Protecting costly endpoints

Not all endpoints carry equal risk, and the tightest rate limits belong on the ones where abuse is most damaging: the endpoints that cost money per request and the endpoints that guard access. A general limit across the whole service is a baseline, but the endpoints that can run up a bill or be hammered for credentials need stricter, dedicated limits, because the cost of leaving them loosely protected is the highest.

## Money-spending endpoints need the tightest limits

Any endpoint that incurs a cost per call, calls to paid external services, sending messages, anything metered, is a direct financial exposure if left loosely limited, because every excess request is real money. These endpoints deserve the strictest limits, set from what legitimate use actually requires, plus a hard ceiling beyond which the caller is cut off entirely rather than merely slowed. The reasoning is that the worst case on a metered endpoint is unbounded cost, and only a hard cap bounds it. A per-request rate limit slows the spend; a hard ceiling on total spend per user or per period stops it, which is the protection that actually limits the financial damage.

## A hard ceiling, not just a rate

For costly endpoints, a rate limit alone is insufficient, because a rate that is acceptable per minute can still accumulate to an unacceptable total over a day. The stronger protection is a hard ceiling on total usage or cost per user over a period, with a firm cutoff when it is reached, on top of the per-request rate limit. This means even a caller staying under the rate limit cannot exceed the total cap, so the maximum possible spend per user is bounded regardless of how they pace their requests. For anything that costs money, this combination, a rate limit plus a hard total ceiling with a real cutoff, is what turns an open-ended financial risk into a known, capped one.

## Authentication endpoints need strict limits

The endpoints that guard access, login, password reset, signup, verification, are targets for abuse and need limits strict enough to make automated attacks impractical. Login is the classic case: without a tight limit, an attacker can try many passwords rapidly, which is how credential-guessing works, so login needs limits that allow a real person their few attempts while stopping the rapid automated guessing. Password reset and verification endpoints can be abused to flood people with messages, so they need limits that prevent a caller from triggering many messages quickly. Signup needs limits to prevent automated mass account creation. Each of these guards something valuable and is abused by making many rapid attempts, which strict rate limits directly prevent.

## Match the strictness to the consequence

The principle tying these together is that the strictness of the limit should match the consequence of abuse on that endpoint. A read-only public endpoint that costs little and guards nothing can have a generous limit, because abusing it does limited harm. A metered endpoint or an access-guarding endpoint can do serious harm when abused, so it gets a strict limit and, for metered ones, a hard ceiling. Setting a single uniform limit across all endpoints either over-restricts the harmless ones or under-protects the dangerous ones; calibrating each endpoint's limit to its risk is what gives both good usability on the safe endpoints and strong protection on the costly ones.

## Know which endpoints carry the risk

Applying this requires knowing which of your endpoints cost money and which guard access, which is worth mapping deliberately rather than assuming. Every call to a paid service, every message sent, every operation billed by usage is a money endpoint. Every login, signup, reset, and verification is an access endpoint. These are the ones that need the strict, dedicated limits and, for the metered ones, the hard ceilings. Identifying them up front and protecting them specifically, rather than relying on a blanket limit, ensures that the endpoints where abuse is most costly are the ones most tightly guarded, which is where rate limiting earns its keep.
