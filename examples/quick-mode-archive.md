# Archive: Migrating Authentication from JWT to Session Tokens

| Field | Value |
|-------|-------|
| Date | 2025-03-15 |
| Tags | architecture, authentication, security |
| Type | decision-making |

## Summary

> Walked through the trade-offs of JWT vs session tokens for our user auth system. Decided to migrate to session tokens after realizing our "stateless" JWTs had grown into a stateful mess anyway — we were checking a blocklist on every request, which defeated the whole point. The migration plan is incremental: new endpoints use sessions, old ones keep JWTs until their refresh cycles expire.

## Key Outputs

- Migration plan: session tokens for new endpoints first, JWTs phase out over 6 weeks
- Redis session store config draft
- List of 4 endpoints that need immediate migration (password reset, email change, account delete, admin panel)

## Action Items

- [ ] Set up Redis session store in staging
- [ ] Write migration guide for the team
- [ ] Audit which endpoints currently rely on JWT claims for authorization logic
- [ ] Schedule team sync to walk through the plan

## Insights & Learnings

### Concepts & Terminology
- **Stateless theater**: When a "stateless" architecture accumulates so many server-side checks (blocklists, rate limits, feature flags) that it's effectively stateful but without the benefits of proper session management.

### Pitfalls & Warnings
- JWT refresh token rotation can silently break mobile clients that retry requests — need to handle race conditions in the token refresh endpoint
- Don't migrate the admin panel last. It's the highest-risk surface and should move to sessions early

## Conversation Record

**User**: I've been going back and forth on whether to switch from JWTs to session tokens. Every article I read makes the opposite case from the last one.

**Assistant**: What's driving the reconsideration? Is it a specific problem you're hitting, or more of a general unease?

**User**: Both, I guess. We added a token blocklist six months ago for instant revocation, and now every request hits Redis anyway. My CTO pointed out we're basically doing sessions with extra steps.

**Assistant**: That's a really common pattern — I'd call it "stateless theater." You started with JWTs for their stateless benefits, but as security requirements grew...

[conversation continues]
