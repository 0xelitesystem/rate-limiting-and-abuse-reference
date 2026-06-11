# Per-IP and per-user limits

Rate limiting needs to decide who a limit applies to, and the two common answers are per source address and per user account. Each catches different abuse and misses different abuse, so strong protection often uses both. Limiting only one leaves a gap the other would have covered, which is why understanding what each does is part of setting effective limits.

## Per-IP limits

A per-IP limit caps requests from a single network address in a window of time, regardless of whether the caller is logged in. This is the natural limit for endpoints that anyone can hit without an account, like login, signup, and public pages, because there is no user identity to limit on yet. It catches a single source making many requests, which covers a lot of automated abuse and runaway clients. Its weakness is that an attacker with many addresses can spread requests across them to stay under the per-IP limit on each, and that many legitimate users can share one address, so a too-tight per-IP limit can catch innocent users behind a shared connection.

## Per-user limits

A per-user limit caps requests tied to a specific account, regardless of which address they come from. This is the natural limit for authenticated endpoints, where there is a logged-in identity to attribute requests to. It catches a single account making excessive requests even if that account spreads them across many addresses, which is exactly the case per-IP limits miss. Its weakness is that it only applies once a user is identified, so it does not help on the endpoints anyone can hit anonymously, and an attacker creating many accounts can spread requests across them, which is why account creation itself needs its own limits and abuse controls.

## Why both are needed

Because each limit covers what the other misses, serious protection usually applies both. The per-IP limit guards the anonymous, public endpoints and catches single-source floods; the per-user limit guards authenticated endpoints and catches single-account abuse spread across addresses. An attacker who evades one often runs into the other: spreading across addresses to beat the per-IP limit still hits the per-user limit if the requests are tied to an account, and spreading across accounts still hits the per-IP limit if the requests come from few addresses. Layering the two closes gaps that either alone would leave open.

## Tune limits to legitimate use

Whatever the limit applies to, it has to be set high enough that normal use is never blocked and low enough that abuse is. This balance depends on how the endpoint is genuinely used: a limit that catches an attacker but also catches a normal user during ordinary activity is too tight and will generate complaints, while one set so high that it never inconveniences an attacker is too loose. Setting limits from an understanding of legitimate usage patterns, and watching whether real users ever hit them, is how you find the level that stops abuse without harming the people you are protecting. The shared-address case for per-IP limits especially needs care, since one address can host many real users.

## Identify the caller reliably

A limit is only as good as its ability to attribute a request to the right caller, so both kinds depend on identifying the source reliably. Per-user limits depend on trustworthy authentication, and per-IP limits depend on reading the real source address correctly, which can be complicated when requests pass through intermediaries that change or add address information. Getting the attribution right matters because a limit that can be evaded by spoofing the identity it keys on provides little protection. Ensuring the caller identity the limit relies on is the genuine one, not something the caller can forge, is part of making the limit effective rather than nominal.
