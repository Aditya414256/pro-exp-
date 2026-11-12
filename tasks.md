# MediFind — Development Tasks and Roadmap

**Purpose:** Track work against the existing application. This is an initial audit-driven backlog, not a claim that every listed item is currently broken.

## Status legend

- [ ] Not started / needs verification
- [x] Verified complete
- [~] In progress
- [!] Blocked or needs a decision

Only mark an item complete after inspecting implementation and running the relevant checks.

## Phase 0 — Baseline and safety

- [x] Confirm repository and default branch.
- [x] Inspect top-level repository structure.
- [x] Identify frontend, backend, models, services, tests and deployment entry points.
- [x] Record core product workflows and critical order invariants.
- [ ] Run the existing test suite and record a fresh baseline result.
- [ ] Run the app locally and verify primary routes.
- [ ] Review current Git working tree before making application changes.
- [ ] Confirm production environment variables are configured securely without exposing their values.

## Phase 1 — Customer experience audit

- [ ] Verify customer registration, login, logout and session restoration.
- [ ] Verify Home page medicine autocomplete, keyboard navigation and search submission.
- [ ] Verify Find Medicine search, medicine details and result states.
- [ ] Confirm the Find Medicine page has no filter sidebar/section.
- [ ] Verify pharmacy directory visibility and service capability labels.
- [ ] Verify store pickup end-to-end, including pharmacy selection.
- [ ] Verify home delivery end-to-end, including delivery address and prescription requirement.
- [ ] Verify order history, status updates and cancellation rules.
- [ ] Verify notification list and mark-as-read behaviour.
- [ ] Verify customer medicine availability requests and cancellation.
- [ ] Verify customer profile update and password change.
- [ ] Check responsive behaviour and accessibility on key customer pages.

## Phase 2 — Pharmacy owner experience audit

- [ ] Verify pharmacy registration and pending verification state.
- [ ] Verify verification document upload and private retrieval.
- [ ] Verify owner dashboard only shows the owner's pharmacy data.
- [ ] Verify inventory create/update and quantity validation.
- [ ] Verify stock-status recalculation.
- [ ] Verify incoming order visibility and allowed status transitions.
- [ ] Verify stock deduction occurs only at ACCEPTED → CONFIRMED.
- [ ] Verify insufficient stock prevents confirmation.
- [ ] Verify stock restoration on valid post-deduction cancellation/rejection.
- [ ] Verify delivery prescription review and rejection reason handling.
- [ ] Verify pickup physical-prescription workflow is communicated correctly.
- [ ] Verify pharmacy-side medicine availability request workflow.
- [ ] Verify pharmacy notifications.

## Phase 3 — Admin experience audit

- [ ] Verify admin-only access to admin endpoints and dashboard.
- [ ] Verify pending pharmacy list and document review.
- [ ] Verify approve/reject actions and resulting pharmacy visibility.
- [ ] Verify active/inactive toggle is separate from verification status.
- [ ] Verify administrative prescription review permissions and metrics.
- [ ] Verify admin actions generate appropriate notifications where implemented.

## Phase 4 — Security and data integrity

- [ ] Audit role checks and resource ownership checks for every protected route.
- [ ] Test IDOR cases by requesting another user's order, prescription, notification and request.
- [ ] Review upload validation, size limits, filenames and file streaming.
- [ ] Confirm Supabase service-role credentials are never present in frontend assets.
- [ ] Confirm storage buckets remain private.
- [ ] Verify local upload fallback is not relied on for durable Vercel storage.
- [ ] Review session cookie and production secret configuration.
- [ ] Review error handling for sensitive data leakage.
- [ ] Test duplicate order submissions and idempotency.
- [ ] Test concurrent stock confirmation against PostgreSQL.
- [ ] Review database constraints and migration consistency.

## Phase 5 — Quality and usability

- [ ] Establish a reproducible test command and baseline test report.
- [ ] Add regression tests for any bugs found during audits.
- [ ] Review empty/loading/error/success states across major pages.
- [ ] Review mobile layouts and keyboard navigation.
- [ ] Remove only confirmed dead code or unused assets after impact analysis.
- [ ] Review console errors and server logs during primary workflows.
- [ ] Update README and API documentation when behaviour changes.

## Phase 6 — Deployment readiness

- [ ] Verify Vercel build and route mapping against the current application.
- [ ] Verify production uses durable PostgreSQL, not ephemeral SQLite on `/tmp`.
- [ ] Verify migration procedure against the configured production database.
- [ ] Verify private Supabase Storage buckets and server-side credentials.
- [ ] Verify a strong production `SECRET_KEY` is configured.
- [ ] Perform a smoke test after deployment.
- [ ] Document rollback and backup procedures.

## Change-specific task template

For each new feature or bug fix, add a task using this format:

### [Short task title]
- **Status:** Not started
- **Problem:** What is wrong or missing?
- **Expected result:** Observable behaviour after the change.
- **Scope:** Exact files/modules expected to change.
- **Dependencies:** API, database, design or product decisions.
- **Acceptance criteria:**
  - [ ] Criterion 1
  - [ ] Criterion 2
- **Tests:** Tests to add/run.
- **Risks:** Possible regressions or data implications.

## Rules for maintaining this file

1. Keep tasks specific and testable.
2. Do not mark a task complete just because code was added.
3. Record blockers and decisions explicitly.
4. Split large tasks into smaller items.
5. Remove or archive tasks only when they are obsolete, with a short reason.
6. Keep this backlog aligned with `memory.md` after meaningful work.
