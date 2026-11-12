# MediFind — AI Agent and Development Rules

These rules apply to every developer and AI coding agent working in this repository.

## 1. Protect the existing application

- This repository already contains a working application. Do not scaffold a replacement or rewrite it from scratch.
- Inspect the relevant HTML, CSS, JavaScript, Flask route, service, model and tests before editing.
- Make the smallest change that solves the requested task.
- Do not change unrelated pages, APIs, workflows, database behaviour or deployment configuration.
- Do not delete existing features because they are inconvenient to work with.
- Do not assume the README is proof that a feature is currently bug-free. Verify implementation and tests.

## 2. Scope and approval

- Follow the user's latest explicit request.
- Do not implement backend functionality when the task is frontend-only.
- Do not change database providers, frameworks, authentication architecture or deployment targets without explicit approval.
- Do not add a new page, feature, library or service merely because it seems useful.
- Ask one focused question when a required product decision cannot be inferred safely.
- If a requested change conflicts with an existing invariant, explain the conflict before changing the invariant.

## 3. Frontend-specific rules

- Current frontend stack is HTML5, vanilla CSS and vanilla JavaScript. Do not introduce React, Vue, Tailwind, Bootstrap or a build system without approval.
- Preserve existing page structure and working interactions unless the task requires a change.
- Keep page-specific styles in the corresponding CSS file and shared navigation styles in the existing shared stylesheet where appropriate.
- Reuse existing API helpers and JavaScript modules rather than creating duplicate fetch wrappers.
- Use semantic HTML, accessible labels, keyboard support, visible focus states and responsive layouts.
- Avoid inline JavaScript for new behaviour when an existing page script can own it.
- Do not hardcode fake API results into production UI.
- Handle loading, empty, error and success states.
- Do not expose private user, pharmacy-owner or admin information in public pages.

## 4. Explicit product/UI constraints

- The Find Medicine page must not have the removed filter sidebar/section. Do not reintroduce filters unless the user explicitly asks.
- Do not create a How It Works page unless explicitly requested.
- Do not add decorative background colors to header/intro sections that the user has intentionally kept plain.
- Preserve the existing MediFind identity and minimal healthcare visual direction.
- Do not redesign all pages when asked to change one component.
- Home and Find Medicine navigation decisions must follow the current approved page structure; do not re-add a removed navigation item based on an old mockup.
- Keep existing page copy unless the requested task includes copy changes.

## 5. Backend and API rules

- Keep route handlers focused on HTTP concerns; place reusable domain logic in service modules.
- Validate all untrusted input on the server.
- Enforce authentication, role permissions, ownership and resource access on every protected endpoint.
- Never trust a user ID, pharmacy ID, role, price, stock quantity or status supplied by the browser without server-side validation.
- Preserve existing response shapes unless all callers and tests are updated.
- Return clear, consistent errors without leaking secrets, file paths or stack traces.
- Use SQLAlchemy transactions appropriately and roll back on failures.
- Avoid N+1 queries and unbounded result sets when adding list endpoints.
- Do not use frontend-only checks as an authorization mechanism.

## 6. Critical order and inventory invariants

- Do not deduct inventory when an order is created.
- Do not deduct inventory when an order is merely accepted.
- Deduct inventory exactly once on the existing ACCEPTED → CONFIRMED transition.
- The deduction must be atomic and must not permit negative stock.
- Restore stock exactly once when a stock-deducted order is validly cancelled/rejected.
- Do not restore stock for PENDING/ACCEPTED cancellation because stock has not yet been deducted.
- Preserve order idempotency protection.
- Preserve the allowed state transition table in `OrderService`; do not invent transitions in frontend code.
- Add tests for retries, duplicate submissions, insufficient stock and concurrent confirmation when changing this logic.

## 7. Prescription and document rules

- Store prescriptions and pharmacy verification documents privately.
- Never expose `SUPABASE_SERVICE_ROLE_KEY` or any secret in browser code, HTML, JavaScript, logs or committed files.
- Validate file extension, size and relevant content assumptions server-side.
- Every download must verify authentication, role and ownership/permission.
- Store pickup prescription flow requires physical verification at the pharmacy counter; do not force digital upload for pickup unless the product requirement changes.
- Prescription-required home delivery requires the supported digital upload and manual review flow.
- Prescription approval is not the same as order acceptance.
- Never put real prescriptions, pharmacy licenses or customer data in test fixtures committed to Git.

## 8. Database and migrations

- Update SQLAlchemy models and create a reviewed Alembic migration for schema changes.
- Never manually edit production database schema as a substitute for migrations.
- Do not run destructive downgrade/drop/reset operations on production.
- Preserve unique constraints, foreign keys and indexes unless a reviewed migration intentionally changes them.
- Test SQLite and PostgreSQL-specific behaviour where relevant.
- Never commit local database files or credentials.

## 9. Security and privacy

- Never commit `.env`, tokens, passwords, API keys, service-role keys or real user data.
- Use environment variables and update `.env.example` with placeholder names only.
- Keep password hashing through the existing secure helpers.
- Prevent IDOR: users must not access another customer's order, prescription, notification or request by changing an ID.
- Avoid logging passwords, session cookies, secrets, prescription contents or sensitive personal data.
- Treat uploaded filenames and request data as untrusted.

## 10. Code quality

- Follow the style already used in the file being edited.
- Avoid unnecessary dependencies and abstractions.
- Prefer readable, small functions with clear responsibilities.
- Do not silently swallow exceptions.
- Do not add comments that merely restate obvious code.
- Do not leave dead code, debug prints, temporary files or TODOs without a reason.
- Keep naming consistent with existing project conventions.

## 11. Testing and completion

Before changing code:
1. Inspect the relevant implementation and tests.
2. Identify the expected behaviour and regression risks.
3. Record the planned scope.

After changing code:
1. Run the narrowest relevant tests.
2. Run the full applicable test suite when practical.
3. Check the browser flow manually for frontend changes when possible.
4. Report tests actually run and their real results.
5. Mention tests not run and any known limitations.
6. Summarize changed files and user-visible effects.

Never claim tests passed if they were not executed. Never mark a task complete solely because code was written.

## 12. Git discipline

- Do not force-push, rewrite history, delete branches or overwrite user work without explicit approval.
- Inspect `git status` and the diff before committing.
- Do not include unrelated changes in a commit.
- Use descriptive commit messages.
- Do not commit generated caches, virtual environments, local databases, uploads or secrets.

## 13. AI agent response format

For implementation tasks, report:
- What was inspected.
- What changed and why.
- Files modified.
- Tests run and exact outcomes.
- Remaining risks or follow-up work.

If the request is ambiguous, ask before making a high-impact or irreversible change. Be direct about uncertainty and do not invent repository facts.
