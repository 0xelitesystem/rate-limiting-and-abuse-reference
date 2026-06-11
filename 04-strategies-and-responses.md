# Strategies and responses

Beyond deciding who and what to limit, rate limiting involves how the limit is enforced and what the service does when a caller hits it. The enforcement strategy shapes how the limit feels to legitimate users and how well it stops abuse, and the response shapes whether a blocked caller backs off gracefully or keeps hammering. Both deserve thought, because a poorly chosen strategy or response can either frustrate real users or fail to deter abuse.

## Common enforcement approaches

There are several ways to enforce a rate limit, and they differ in how they count requests over time. Some approaches count requests in fixed time windows, resetting each window. Others track a rolling window for smoother behavior. Others model the limit as a budget that refills over time, allowing short bursts while capping the sustained rate. The differences matter mainly at the edges: a simple fixed window can allow a burst at the boundary between windows, while a budget-based approach handles bursts more gracefully. For most purposes any reasonable approach works; the point is to have one, and to pick one whose burst behavior fits your endpoint, allowing legitimate short bursts where they are normal and preventing them where they signal abuse.

## Allow legitimate bursts, cap sustained abuse

A good rate limit distinguishes between a brief legitimate burst and sustained abuse. Real usage is often bursty: a user loads a page that makes several requests at once, then is idle. A limit that blocks normal bursts frustrates real users, while one that only caps the sustained rate over time allows the normal bursts and still stops the continuous flood that abuse requires. Choosing an enforcement approach that permits reasonable short bursts while capping the sustained rate fits how legitimate clients actually behave, which is part of setting limits that protect without inconveniencing the people you are protecting.

## Respond clearly when the limit is hit

When a caller exceeds the limit, the service should respond with a clear signal that the limit was hit, rather than a generic error or silence, so that a well-behaved client knows to slow down and a user understands what happened. There is a standard status code meaning too many requests, and using it tells clients explicitly that they are rate limited. Including information about when they may try again helps well-behaved clients back off appropriately rather than retrying immediately. A clear response turns the limit into something clients can respond to correctly, where an ambiguous one leaves them retrying blindly, which wastes effort on both sides.

## Make blocked callers back off, not retry harder

The response should encourage a blocked caller to wait rather than retry immediately, because a client that retries the instant it is blocked keeps the load high and defeats the limit's purpose. Telling the client when it can retry, and having well-behaved clients honor that, produces a graceful back-off where the load drops until the window clears. Abusive callers will ignore this, but the limit still holds against them because the requests are rejected regardless; the back-off guidance mainly helps the legitimate clients that got caught behave correctly. The combination of rejecting excess requests and signaling when to retry handles both honest and dishonest callers appropriately.

## Lean on the platform where you can

Implementing solid rate limiting from scratch is work, and much of it is already provided by the infrastructure many sites run behind. Services that sit in front of an origin commonly offer rate limiting, abuse protection, and traffic filtering as features, so before building your own, it is worth checking whether the platform you already use covers it. This fits the stack-check discipline: the protection you need may be configuration on a service you already have rather than new code to write and maintain. Where the platform provides it, configuring it well is faster and more reliable than reimplementing it; where it does not, the strategies here are what you build, focused tightest on the costly and access-guarding endpoints.
