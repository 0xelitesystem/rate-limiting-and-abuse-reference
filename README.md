# Rate Limiting and Abuse Reference

Rate limiting caps how many requests a caller can make in a given time, so that no single user, script, or attacker can overwhelm your service, run up your costs, or hammer a sensitive endpoint. It is a basic, necessary defense for any public endpoint, and it matters most on the things that cost money or guard access: login, signup, password reset, and anything that calls a paid API. The principle is to set limits before someone forces you to, tightest where abuse hurts most.

## What is inside

- [01-why-rate-limiting-matters.md](01-why-rate-limiting-matters.md) what unlimited requests expose you to
- [02-per-ip-and-per-user-limits.md](02-per-ip-and-per-user-limits.md) who you limit and why both matter
- [03-protecting-costly-endpoints.md](03-protecting-costly-endpoints.md) the tightest limits go on what costs money
- [04-strategies-and-responses.md](04-strategies-and-responses.md) how limits are enforced and what to return
- [05-beyond-rate-limits.md](05-beyond-rate-limits.md) the abuse controls that rate limiting alone misses

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright (c) 2026 RBL Industries LLC.
