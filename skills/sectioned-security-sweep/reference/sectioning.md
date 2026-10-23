# Sectioning

Cut the tree into five to ten sections. Focus beats breadth: one pass, one
section, full attention.

## How to cut

1. Map top-level packages and entrypoints (HTTP handlers, CLI, workers, jobs).
2. Group by security concern, not by language or folder fashion.
3. Aim for sections a frontier model can hold in one focused review without
   drowning in unrelated modules.
4. Record path globs or directory lists per section. Keep the list short.

## Typical section set

Adapt; drop what the project lacks; split what is huge.

| Section | Look for |
|---------|----------|
| Auth and sessions | Login, password reset, tokens, cookies, SSO, MFA, session fixation |
| Authorization | Role checks, object ownership, admin gates, IDOR |
| Public API / ingress | Unauthenticated routes, parsers, uploads, SSRF, request smuggling |
| Payments / billing | Webhooks, amount trust, replay, idempotency |
| Data store | SQL/NoSQL injection, migrations, backup exposure, tenancy |
| Crypto and secrets | Key handling, RNG, secret logging, hard-coded credentials |
| UI / client | XSS, open redirects, sensitive data in browser storage |
| Supply chain | Lockfile trust, postinstall scripts, vendored binaries |
| Deploy and config | Default secrets, debug endpoints, CORS, TLS termination |

## Rules

- Every in-scope source file belongs to one section, or is listed as deferred.
- Prefer eight sections over two giant ones.
- A monorepo may need one section list per deployable service.
- Generated vendor trees are out of scope unless the pass is explicitly
  supply-chain.

## Example (neutral)

For a small bookstore API:

1. Auth: `src/auth/**`, `src/middleware/session.ts`
2. Catalog API: `src/routes/books/**`
3. Checkout: `src/routes/checkout/**`, `src/payments/**`
4. Admin: `src/routes/admin/**`
5. Data: `src/db/**`, `migrations/**`
6. Config: `deploy/**`, `.env.example`, `docker-compose.yml`
