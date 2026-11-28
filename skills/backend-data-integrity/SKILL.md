---
name: backend-data-integrity
description: Backend rules for business invariants, transactions, numeric limits and their tests (locking order, column-size caps, SQLite template DB built from migrations, concurrency tests), plus the dev-only demo-account login prefill. Use when writing or reviewing transactional business logic or its tests.
---

# Backend data integrity

## Transactions
- Take the lock (sequence increment or locked row) BEFORE checking an invariant that depends on it; checking first leaves a race window.
- Cap amounts to the column size (e.g. 32-bit Int) even if application validation accepts larger values: overflow must be a clean validation error, not a database failure.

## Testing business rules
- Build a template SQLite database from the migration files (split trigger bodies on `BEGIN ... END`, not on `;`) and copy it per test file: real schema and triggers, no shared state.
- Test concurrency with `Promise.allSettled` on parallel calls and assert the invariant on the final state, not on individual results.

## Dev-only demo account prefill
- The seed writes the demo credentials as `VITE_DEMO_*` into a gitignored `.env.development.local`; the front end reads them only under `import.meta.env.DEV`.
- Verify the production build contains no trace of them (grep the bundle).
