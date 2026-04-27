# 001 — Authentication

**Jira Ref**: SKL-001
**Status**: 🟡 In Progress
**Implementation issues**: [LMS-001](../05-issue/LMS-001.md)

## Problem Statement

GRC platform users today have no way to identify themselves to the application. Without authentication, every feature that depends on per-user state (course enrollments, audit trails, role-based access to compliance evidence) is blocked. Users need a low-friction sign-up that doesn't force them to depend on a third-party identity provider on day one — many of our enterprise prospects evaluate the platform from networks where Google or LinkedIn OAuth is unreliable.

## Solution

Ship local email + password authentication first, with sessions delivered as HttpOnly cookies. OAuth providers (Google, Apple, LinkedIn) ship as a follow-up slice once the core auth surface is stable.

## User Stories

1. As a new user, I want to sign up with email and password, so that I can start using the platform without a third-party account.
2. As a returning user, I want to log in with email and password, so that I can resume my work.
3. As any signed-in user, I want to log out, so that someone else using the same machine can't access my data.
4. As any signed-in user, I want my session to persist across tabs and across reasonable time, so that I don't have to log in repeatedly during a workday.
5. As a user who mistypes credentials, I want a clear but generic error, so that I can correct my mistake without learning whether an email is registered.
6. As a security-conscious admin, I want failed login attempts to be rate-limited, so that brute-force attacks are mitigated.
7. As a compliance reviewer, I want signup, login (success + failure), and logout to produce audit log entries, so that we can answer "who logged in when" during an audit.
8. As a user with a weak password instinct, I want the system to reject very weak passwords, so that I'm protected by default.
9. As a user on slice 1.0.1, I want to sign in with Google, Apple, or LinkedIn, so that I don't have to manage another password. _(Deferred — separate PRD slice)_

## Implementation Decisions

- **Modules**: a single `auth` module owns sign-up, login, logout, and current-user lookup. A separate `users` module owns user records.
- **Session model**: server-side opaque tokens, hashed at rest. Cookies carry the plaintext token, marked HttpOnly + Secure + SameSite=Lax. 30-day rolling expiry.
- **Password storage**: bcrypt at cost 12. Minimum 12 chars, maximum 128 chars. Reject from a common-password list at sign-up time.
- **Generic auth errors**: same response body for "unknown email" and "wrong password" to prevent enumeration.
- **Rate limiting**: 10 requests/minute per IP on `/auth/login` and `/auth/signup`.
- **Audit trail**: signup, login (success + failure), and logout each produce an audit log entry.
- **Out-of-band concerns** like email verification, password reset, and MFA are intentionally deferred and tracked as separate PRDs.

## Testing Decisions

- A good auth test verifies behavior over the HTTP boundary, not internals — the cookie's flags matter, the cookie's exact bytes do not.
- Unit tests cover the password hasher (bcrypt round-trip), the session token issuance, and the auth service policy with mocked repositories.
- Integration tests cover each endpoint end-to-end through `supertest` against a real test database, exercised by `/api-testing`.
- E2E tests cover signup → login → me → logout happy path plus wrong-password and expired-session failures, exercised by `/e2e-testing`.
- Prior art: none yet — this is the first feature. Patterns established here become the template for later features.

## Out of Scope

- OAuth providers (Google, Apple, LinkedIn) — deferred to slice 1.0.1
- Password reset flow — separate PRD
- Email verification flow — separate PRD slice
- Multi-factor authentication — future epic
- Admin impersonation — future epic
- SSO / SAML — future epic, enterprise tier

## Further Notes

- Versioning history for this PRD lives in [legacy.md](legacy.md)
- Engineering system design: [LMS-001-system-design](../../../engineering-grc-platform/docs/03-system%20design/LMS-001-system-design.md)
