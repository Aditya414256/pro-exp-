# MediFind — Architecture

**Repository:** https://github.com/Aditya414256/pro-exp-  
**Branch inspected:** `main`  
**Architecture status:** Reverse-engineered from repository files; verify before major changes.

## 1. System overview

MediFind is a single Flask application serving a static HTML/CSS/JavaScript frontend and JSON API endpoints. The frontend uses browser JavaScript and the Fetch API to communicate with Flask. Flask-Login manages user sessions. Flask-SQLAlchemy provides persistence through SQLite in local development or PostgreSQL (including Supabase PostgreSQL) when configured. Supabase Storage is used for private document storage when credentials are configured.

```text
Browser
  |
  | HTML / CSS / Vanilla JavaScript
  | Fetch API (same-origin credentials)
  v
Flask application (backend/app.py)
  |-- Static page delivery from frontend/
  |-- API blueprints
  |     |-- Authentication
  |     |-- Medicines
  |     |-- Orders
  |     |-- Pharmacies / inventory
  |     |-- Admin
  |     |-- Notifications / prescriptions
  |     |-- Medicine requests
  |
  v
Service layer (backend/services/)
  |-- Order and stock rules
  |-- Pharmacy and inventory operations
  |-- Medicine search
  |-- Prescription and private storage
  |-- Notifications and requests
  |-- Location calculations
  |
  +--> SQLAlchemy --> SQLite (development)
  |              -> PostgreSQL / Supabase (configured deployment)
  |
  +--> Supabase Storage private buckets
       prescriptions / pharmacy-docs
       Local protected upload fallback for development
```

## 2. Repository structure

### Backend
- `backend/app.py`: Flask application factory, extension setup, blueprints, static page routes and error handlers.
- `backend/config.py`: environment loading, database URL normalization, SQLAlchemy options, upload configuration and Supabase settings.
- `backend/database.py`: shared SQLAlchemy database object.
- `backend/models.py`: ORM entities and serialization helpers.
- `backend/seed.py`: development seed data and schema helper logic.
- `backend/routes/`: HTTP request parsing, authentication/authorization checks and response construction.
- `backend/services/`: domain logic and integrations.

### Frontend
- `frontend/*.html`: page markup.
- `frontend/*.css`: page-specific styles; `HomePage.css` also provides shared navigation styling.
- `frontend/js/api.js`: shared API client.
- `frontend/js/auth.js`: authentication-related frontend behaviour.
- `frontend/js/home.js`: home medicine autocomplete and search.
- `frontend/js/selection.js`: medicine search, selection and pickup/delivery ordering flow.
- `frontend/js/navbar.js`: navigation/session-aware navigation behaviour.
- `frontend/js/pharmacies.js`: pharmacy directory behaviour.
- `frontend/js/pharmacy-portal.js`: pharmacy dashboard behaviour.
- `frontend/js/admin-portal.js`: admin dashboard behaviour.

### Other
- `migrations/`: Flask-Migrate/Alembic migration configuration and revisions.
- `tests/`: application, PostgreSQL concurrency and Supabase Storage tests.
- `api/index.py`: Vercel serverless entry point.
- `vercel.json`: Vercel build and routing configuration.
- `run.py`: local development entry point.
- `requirements.txt`: Python dependencies.
- `.env.example`: environment-variable template; never place real secrets here.

## 3. Flask request lifecycle

1. A browser page loads from the `frontend/` directory.
2. JavaScript calls the shared `API` object in `frontend/js/api.js`.
3. The API client sends same-origin requests with credentials and JSON-encodes ordinary object bodies.
4. Flask routes receive requests through registered blueprints.
5. Routes perform request-level validation and authentication/role checks.
6. Domain operations are delegated to service classes where implemented.
7. Services use SQLAlchemy models and the shared `db` object; storage operations use the storage service.
8. Flask returns JSON or a protected file response.
9. Frontend code updates the page and handles failures.

Keep route handlers thin. Put reusable domain rules in services, not duplicated in multiple routes or JavaScript.

## 4. Data model

The inspected `backend/models.py` defines these primary entities:

- **User** (`users`): email, password hash, name, phone and role (customer, pharmacy, admin).
- **Pharmacy** (`pharmacies`): owner, name, license, contact/address/location, pickup/delivery capabilities, verification status, active status and verification document metadata.
- **Medicine** (`medicines`): name, generic/brand name, strength, dosage form, description and prescription requirement.
- **PharmacyInventory** (`pharmacy_inventory`): pharmacy, medicine, quantity, price, stock status, batch and expiry metadata. A unique constraint exists for pharmacy + medicine.
- **Order** (`orders`): customer, pharmacy, medicine, quantity, order type, state and related ordering details.
- **Prescription** (`prescriptions`): customer, optional pharmacy association, storage path and review metadata.
- **Notification** (`notifications`): user-targeted in-app messages and read state.
- **MedicineRequest** (`medicine_requests`): customer medicine availability request, pharmacy association and request state.

Inspect the current model definitions and migrations before changing columns, relationships, constraints or enum-like state values. Never edit a production schema by changing model code alone.

## 5. Domain services

- `order_service.py`: order creation, idempotency, state machine and stock accounting.
- `inventory_service.py`: inventory operations and stock-status recalculation.
- `medicine_service.py`: medicine search and autocomplete.
- `pharmacy_service.py`: pharmacy registration, verification and listing.
- `prescription_service.py`: upload validation, storage and prescription workflow.
- `supabase_storage_service.py`: server-side Supabase Storage REST integration.
- `request_service.py`: medicine availability request workflow.
- `notification_service.py`: notification creation/retrieval/read state.
- `location_service.py`: geographic distance calculations.

## 6. API blueprint map

The registered blueprints include:

| Blueprint | Prefix | Responsibility |
|---|---|---|
| auth | `/api/auth` | Registration, login, logout, current user and profile |
| medicines | `/api/medicines` | Search, suggestions and medicine detail |
| orders | `/api/orders` | Order creation, retrieval, cancellation and status |
| pharmacies | `/api` and pharmacy routes | Pharmacy directory, owner operations, inventory and documents |
| admin | `/api/admin` | Pharmacy verification and admin operations |
| common | `/api` | Notifications and prescription upload/download |
| requests | `/api` | Customer and pharmacy medicine availability requests |

For exact endpoint methods, request fields and response schemas, inspect the corresponding route file. Do not invent endpoints or alter response shapes without updating all callers and tests.

## 7. Persistence and configuration

- Default local database: SQLite file at the repository root (`medifind.db`).
- If `DATABASE_URL` is set, configuration normalizes PostgreSQL URLs to the psycopg2 driver.
- On Vercel without a configured database URL, the current config falls back to SQLite under `/tmp`. This is ephemeral and must not be treated as durable production persistence.
- Production persistence is intended to use PostgreSQL, with Supabase documented in the README.
- `MIGRATION_DATABASE_URL` is documented for migration operations; follow the repository's deployment instructions and use a suitable direct connection for schema migrations.
- Flask-Migrate manages Alembic revisions under `migrations/`.

## 8. File storage and security boundaries

- Supabase Storage buckets are configured as private: `prescriptions` and `pharmacy-docs`.
- The service-role key must remain server-side.
- The backend validates uploads and performs authorization before serving stored files.
- Local disk storage is a fallback when Supabase Storage is not configured.
- Vercel serverless filesystem storage is not durable; do not rely on it for production uploads.
- Never add secrets, real customer data, prescriptions or license documents to Git.

## 9. Deployment

- Local: `python run.py`, default port 5000.
- Vercel: `api/index.py` exposes the Flask app; `vercel.json` maps API, static assets and HTML routes.
- Production configuration must supply a strong `SECRET_KEY`, durable PostgreSQL `DATABASE_URL`, and the required Supabase Storage settings when using remote file storage.
- Verify Vercel runtime/build compatibility before changing dependencies or routing.

## 10. Architecture change rules

1. Preserve the Flask app factory and blueprint registration pattern unless migration is intentional.
2. Keep API contracts backward-compatible where practical.
3. Add Alembic migrations for schema changes; review generated migrations before applying.
4. Keep all authorization checks server-side.
5. Keep inventory and order state transitions in the service layer and transactional.
6. Keep file access private and storage credentials server-only.
7. Update tests and documentation whenever architecture changes.
8. Do not describe SQLite on Vercel `/tmp` as persistent storage.
