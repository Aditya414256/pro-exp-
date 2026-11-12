# MediFind — Project Memory and Session Handoff

**Repository:** https://github.com/Aditya414256/pro-exp-  
**Branch inspected:** `main`  
**Last repository inspection:** 2026-11-12

This file is the handoff note for future AI coding sessions. Update it after meaningful implementation work. The current contents describe the inspected baseline; they are not a live status feed.

## 1. Project identity

MediFind is an existing full-stack medicine availability and verified pharmacy locator. It connects customers to local pharmacies and supports medicine search, pickup/delivery orders, pharmacy inventory, prescription workflows, notifications, medicine availability requests and admin pharmacy verification.

Do not treat this as a greenfield project. Preserve the current application and improve it incrementally.

## 2. Repository baseline

- Repository: `Aditya414256/pro-exp-`
- Default branch inspected: `main`
- Frontend: HTML5, vanilla CSS3 and vanilla JavaScript (ES modules/scripts as currently structured).
- Backend: Python and Flask.
- Authentication: Flask-Login.
- ORM/database: Flask-SQLAlchemy and SQLAlchemy.
- Migrations: Flask-Migrate / Alembic.
- Local database default: SQLite.
- Production database documented: PostgreSQL via Supabase.
- File storage: private Supabase Storage with local development fallback.
- Tests: pytest.
- Deployment: Vercel configuration is present.

## 3. Important paths

- `backend/app.py` — Flask app factory and blueprint registration.
- `backend/config.py` — environment and database/storage configuration.
- `backend/models.py` — User, Pharmacy, Medicine, PharmacyInventory, Order, Prescription, Notification and MedicineRequest.
- `backend/routes/` — auth, medicine, order, pharmacy, admin, notifications/prescriptions and requests.
- `backend/services/` — business logic and integrations.
- `frontend/HomePage.html` / `HomePage.css` — home page and shared navigation styling.
- `frontend/FindMedi.html` / `FindMedi.css` — medicine search page.
- `frontend/Pharmacies.html` / `Pharmacies.css` — pharmacy directory.
- `frontend/js/api.js` — shared browser API helper.
- `frontend/js/selection.js` — medicine selection and ordering flow.
- `frontend/js/home.js` — home autocomplete.
- `frontend/js/navbar.js` — navigation behaviour.
- `frontend/js/pharmacy-portal.js` — pharmacy portal.
- `frontend/js/admin-portal.js` — admin portal.
- `migrations/` — database migrations.
- `tests/` — automated tests.
- `api/index.py`, `vercel.json` — Vercel entry and routing.
- `run.py` — local development entry point.

## 4. Product and UX constraints to preserve

These are explicit project requirements and must not be casually reversed:

1. Do not add the removed filter sidebar/section to Find Medicine.
2. Do not create a How It Works page unless explicitly requested.
3. Do not add decorative background colors to header/intro sections that were intentionally kept plain.
4. Prefer small, targeted changes over redesigning existing pages.
5. Preserve the MediFind brand and minimal healthcare visual language.
6. Do not implement backend functionality for a frontend-only task.
7. Keep the existing codebase and workflows; do not rebuild from scratch.

## 5. Existing design baseline

- Body/page background: `#f8fafc`
- Main text: `#0f172a`
- Muted text: `#64748b`
- Border: `#e2e8f0`
- Primary blue: `#0284c7`
- Dark blue: `#0369a1`
- Soft blue: `#e0f2fe`
- Success accent: `#059669`
- Verified green: `#047857`
- Verified background: `#d1fae5`
- Fonts: Plus Jakarta Sans and Space Grotesk.
- Existing main content max width on discovery pages: approximately 1100px.
- Existing navigation height: 72px.

Consult `design.md` and inspect actual CSS before changing tokens.

## 6. Critical business invariants

### Orders and stock
- Order creation does not deduct stock.
- ACCEPTED does not deduct stock.
- ACCEPTED → CONFIRMED deducts stock once, atomically.
- Valid cancellation/rejection after deduction restores stock once.
- PENDING/ACCEPTED cancellation does not change stock.
- Customer cancellation is limited to PENDING and ACCEPTED by the current service rules.
- Idempotency prevents duplicate orders on retries.
- The OrderService transition map is the source of truth.

### Prescriptions
- Store pickup: digital upload is not required solely for pickup; pharmacy verifies physical prescription at counter.
- Home delivery for prescription-required medicine: digital prescription upload is required.
- Pharmacy manually reviews uploaded delivery prescriptions.
- Prescription approval and order acceptance are separate actions.
- Prescription files must remain private and protected by server-side authorization.

### Pharmacy verification
- Verification status and active/inactive status are separate fields.
- Only an authorized admin can approve/reject pharmacy verification.
- Do not expose verification documents publicly.

## 7. Current status — known vs unverified

### Verified by repository inspection
- The repository contains frontend, Flask backend, models, services, migrations, tests and Vercel configuration.
- The application factory registers the expected blueprints.
- SQLite is the default local database configuration.
- PostgreSQL URL normalization and Supabase Storage configuration exist.
- Private storage service code and protected upload/download service paths exist.
- The order service contains an explicit state machine and stock deduction/restoration logic.
- The Find Medicine HTML currently has no filter sidebar.
- Pytest test files exist, including main application tests, PostgreSQL concurrency tests and Supabase Storage tests.

### Not verified in this inspection
- Whether the current branch passes all tests in a clean environment.
- Whether every README feature works end-to-end in a browser.
- Whether the current production Vercel deployment is healthy.
- Whether the configured Supabase project credentials/buckets/database are currently active.
- Whether every environment variable is correctly configured in deployment.
- Whether all responsive states are correct on real devices.

Do not convert an unverified item into a confirmed claim. Update this section after executing tests or smoke checks.

## 8. Recent documentation change

The six project guidance files are being created under `test project/`:

- `prd.md`
- `architecture.md`
- `rules.md`
- `design.md`
- `tasks.md`
- `memory.md`

They are intended to guide future experiments and AI-assisted development. They do not themselves change the application runtime.

## 9. Session handoff procedure

At the start of a new coding session:
1. Read this file.
2. Read `rules.md` before editing code.
3. Read the relevant sections of `prd.md`, `architecture.md` and `design.md`.
4. Check `tasks.md` for current priorities.
5. Inspect the actual code and Git status; memory can become stale.

At the end of a coding session:
1. Update task status honestly.
2. Record files changed and key decisions.
3. Record tests run and exact results.
4. Record unresolved bugs, blockers and risks.
5. Update architecture/design docs if the system or visual rules changed.
6. Do not store secrets, private customer data or prescription details here.

## 10. Decision log

| Date | Decision | Reason |
|---|---|---|
| 2026-11-12 | Keep the existing Flask + vanilla frontend architecture as the baseline | Avoid unnecessary rewrite and preserve working code |
| 2026-11-12 | Keep documentation separate in `test project/` | Isolate experiment guidance from application source files |
| 2026-11-12 | Treat code and tests as authority over README claims | Prevent inaccurate implementation assumptions |

Add new decisions with dates. If a decision is reversed, record the new decision and why.
