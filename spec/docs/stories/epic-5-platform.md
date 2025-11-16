# Epic 5 — Platform (Authentication, Billing, Quotas)

Goal: Provide authentication, entitlements, quotas with rollover, access control, and secure artifact delivery.

## Story 5.1 — Sign in and profile
As a user, I want to sign in and view my profile so that my content and entitlements are tied to my identity.

Acceptance criteria:
- Supports sign in via Google and GitHub OAuth, plus email magic-link fallback.
- On first sign in, a user record is created with display name and roles defaulted.
- Profile page shows: email, display name, roles, current subscription status, remaining simulation runs.
- Sign out clears tokens/cookies; protected pages redirect to sign in.

## Story 5.2 — Manage subscriptions (entitlements)
As a customer, I want my subscription to unlock features so that I can access subscriber-only capabilities.

Acceptance criteria:
- Subscription records created/updated via payment provider webhook (user_subscriptions).
- Application Programming Interface endpoint /me/entitlements returns flags (analytics_access, quick_test_access, private_worlds).
- Entitlements are applied server-side to gate endpoints and client-side to hide locked UI.
- When subscription expires, flags are revoked immediately.

## Story 5.3 — Purchase and apply usage packs
As a user, I want to buy usage packs so that I can exceed my monthly allocation when needed.

Acceptance criteria:
- Usage pack catalog (actors, world_seed, simulation_runs, private_world).
- Purchases recorded in user_purchases with quantities and timestamps.
- Application Programming Interface applies pack balances to quotas on enqueue; balances decrement atomically.
- Purchase history visible in profile/billing view.

## Story 5.4 — Enforce simulation quotas with rollover
As a user, I want clear simulation run limits with rollover so that I can plan my runs.

Acceptance criteria:
- Quota = monthly_simulation_runs + rollover (capped at configured multiplier) − used.
- On run enqueue: quota is checked and decremented atomically; on failure, user receives clear error and call to action.
- Rollover is applied at month boundary up to cap; unused runs beyond cap are dropped.
- UI meter shows remaining runs and rollover cap; Application Programming Interface exposes /usage.

## Story 5.5 — Access control for private content
As a creator, I want private worlds and strategies so that only I (and later, my team) can access them.

Acceptance criteria:
- World/strategy visibility = public | private; private requires owner identity.
- Application Programming Interface enforces resource-level authorization; attempts by non-owners return 404 (not found) to avoid info leaks.
- Listings filter out private resources for non-owners; owners see their private items.
- Future: organization/team membership placeholder included in schema (no UI yet).

## Story 5.6 — Secure artifact delivery and audit trail
As a user, I want secure links to run artifacts so that my data is protected.

Acceptance criteria:
- Artifacts (manifest, summary, pairwise, traces) are retrieved via time‑limited signed URLs.
- Run manifest includes seed, parameters, artifact checksums, and signature.
- Access to artifact URLs is only issued to authorized users for the run’s world.
- Read access is logged with timestamp and user id for audit.

## Story 5.7 — Admin: webhooks and reconciliation
As an operator, I want robust webhook handling so that subscriptions and packs stay consistent.

Acceptance criteria:
- Idempotent webhook endpoints with signature verification and replay protection.
- Dead-letter queue for failed webhook processing; retries with backoff.
- Nightly reconciliation job compares provider state to user_subscriptions/user_purchases and fixes drifts; emits report.



