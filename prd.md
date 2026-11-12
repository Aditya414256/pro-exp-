# MediFind — Product Requirements Document

**Project:** MediFind  
**Document status:** Baseline derived from repository inspection  
**Repository:** https://github.com/Aditya414256/pro-exp-  
**Branch inspected:** `main`  
**Last inspected:** 2026-11-12

## 1. Product overview

MediFind is a web application that helps customers discover medicines and nearby verified pharmacies. Customers can search for medicines, compare pharmacy availability, choose store pickup or home delivery, place and track orders, and receive notifications. Pharmacy owners manage their pharmacy profile, inventory, incoming orders, prescriptions and medicine availability requests. Administrators review pharmacy verification and oversee platform activity.

MediFind is an information and ordering platform. It does not replace a doctor's advice, pharmacist judgement, a valid prescription, or applicable pharmacy regulations.

## 2. Product goals

1. Make medicine discovery and pharmacy availability easier.
2. Help customers identify pharmacies that have completed platform verification.
3. Provide separate, role-appropriate workflows for customers, pharmacy owners and administrators.
4. Support a reliable order lifecycle with correct stock accounting.
5. Handle prescriptions and pharmacy verification documents privately.
6. Keep the application usable on common desktop and mobile screen sizes.
7. Preserve existing working behaviour while improving the product incrementally.

## 3. User roles

### Customer
- Register, sign in, sign out and manage profile information.
- Search medicines by medicine name, generic name and supported attributes.
- Browse the pharmacy directory and available stock.
- Choose store pickup or home delivery where supported.
- Select a pharmacy and submit an order.
- Upload a prescription when required for home delivery.
- View order history, order status and notifications.
- Cancel orders only when the backend permits it.
- Submit and track medicine availability requests.

### Pharmacy owner
- Register a pharmacy and submit its verification details/documents.
- View verification status.
- Manage pharmacy inventory, including quantity, price, batch number and expiry date where supported.
- View incoming orders and perform only valid order-state transitions.
- Review delivery prescriptions manually.
- Verify physical prescriptions at the counter for pickup orders.
- Review customer medicine availability requests.
- Receive relevant in-app notifications.

### Administrator
- Review pharmacy registrations and verification documents.
- Approve or reject pharmacy verification.
- Activate or deactivate pharmacies.
- Access administrative platform metrics and supported review workflows.
- Perform administrative actions only through server-authorized endpoints.

## 4. Functional requirements

### FR-01 — Authentication and authorization
- Support customer and pharmacy registration and login.
- Support role-based access for customer, pharmacy and admin.
- Store password hashes, never plaintext passwords.
- Enforce authorization in the backend; hiding a frontend control is not security.
- Preserve session behaviour and existing Flask-Login integration.

### FR-02 — Medicine discovery
- Provide medicine search and autocomplete.
- Search supported medicine fields, including name, generic name, brand, strength and dosage form.
- Return medicine details and prescription requirement where available.
- Do not invent stock or availability; display backend results.

### FR-03 — Pharmacy discovery
- Display active, verified pharmacies through the supported public directory.
- Show supported location and service information.
- Respect pickup and delivery capability flags.
- Do not expose private owner or verification-document information publicly.

### FR-04 — Store pickup
- Show pharmacies eligible for pickup and with sufficient stock.
- Let the customer choose a pharmacy.
- Create an order without prematurely deducting stock.
- For prescription-required medicines, a digital upload is not required solely for store pickup; the pharmacy must verify the physical prescription at the counter before dispensing.

### FR-05 — Home delivery
- Show eligible pharmacies that support delivery.
- Collect the delivery details required by the backend.
- Require digital prescription upload for prescription-required medicines before placing a delivery order.
- Keep prescription review as a manual pharmacy workflow.
- Do not treat prescription approval as automatic order acceptance.

### FR-06 — Order lifecycle and stock integrity
The current service defines these transitions:

`PENDING → ACCEPTED → CONFIRMED → PREPARING → READY_FOR_PICKUP / OUT_FOR_DELIVERY → COMPLETED`

Other supported terminal paths include rejection and cancellation. The backend service is authoritative for exact transition rules.

- Creating or accepting an order must not deduct stock.
- Stock is deducted exactly once at ACCEPTED → CONFIRMED.
- Deduction must be atomic and must not allow negative inventory.
- If a stock-deducted order is cancelled or rejected through an allowed path, restore stock exactly once.
- Customer cancellation is limited to PENDING and ACCEPTED in the current service rules.
- Preserve idempotency protection against duplicate order submissions.

### FR-07 — Prescriptions and verification documents
- Validate uploaded file extensions and enforce upload-size limits.
- Store prescription and pharmacy verification files in private Supabase Storage when configured.
- Use protected local storage only as the development fallback.
- Never expose the Supabase service-role key to frontend code.
- Authorize every download/stream request on the server.
- Keep manual review and rejection reasons where the existing workflow requires them.

### FR-08 — Medicine availability requests
- Customers can submit a request for a medicine to a pharmacy.
- Customers can view their own requests and cancel eligible pending requests.
- Pharmacy owners can review and update requests through allowed states.
- Keep ownership and role checks on every request operation.

### FR-09 — Notifications
- Provide in-app notifications for supported order, request and verification events.
- Users may view notifications and mark their own notifications as read.
- Never allow one user to read or mutate another user's notifications.

### FR-10 — Profile and administration
- Support the existing customer profile update and password-change capabilities.
- Admin endpoints must reject non-admin users.
- Verification status and active status are distinct concepts; do not merge them.

## 5. Non-functional requirements

- **Security:** server-side authorization, password hashing, private file storage, input validation and secret isolation.
- **Data integrity:** transactions, database constraints, idempotency and safe inventory transitions.
- **Reliability:** clear API errors, predictable frontend error states and safe retries.
- **Maintainability:** keep routes, services, models and frontend modules separated.
- **Compatibility:** preserve Flask application factory, current API contracts and deployment entry points unless deliberately migrated.
- **Accessibility:** semantic HTML, keyboard-operable controls, visible focus, labels and sufficient contrast.
- **Responsive design:** layouts should remain usable on mobile, tablet and desktop.
- **Testability:** business rules and critical workflows must have automated tests.

## 6. Out of scope unless explicitly approved

- Replacing Flask or the existing frontend framework.
- Rewriting the application from scratch.
- Automatically diagnosing conditions or recommending prescription medicines.
- Automatic prescription approval or AI-based medical decisions.
- Payment gateway, live courier tracking, chat, reviews or other features not verified in the current implementation.
- Changing the database provider or deployment platform as part of an unrelated UI task.

## 7. Success criteria

- A customer can complete supported search and order workflows.
- Pharmacy and admin actions are protected by backend role checks.
- Order transitions and inventory accounting remain correct under retries and concurrent requests.
- Prescription/document access remains private.
- Existing automated tests continue to pass, with new tests for changed behaviour.
- No unrelated page or workflow regresses during an experiment.

## 8. Product decisions and open verification

Treat README feature descriptions as intended/current product documentation, not proof that every path is defect-free. Before marking a requirement complete, inspect the relevant route, service, frontend integration and tests. Record unverified behaviour in `tasks.md` or `memory.md`; do not silently assume it works.
