# ZyncOps Django Server

Backend service for the ZyncOps WooCommerce plugin. This Django app powers license validation, courier automation, fraud/risk checks, Facebook Conversions API dispatch, and plugin download management. It also exposes a small internal admin/CRM API (under `trizync`) for managing clients, services, and projects.

## What This Server Does
- Validates plugin licenses and returns expiry status.
- Creates and tracks courier parcels for Redx, Steadfast, and Pathao.
- Performs courier success‑ratio / fraud checks via bdCourier.
- Sends Facebook Conversions API events on behalf of the plugin.
- Hosts the latest plugin ZIP in `media/` via the `ZyncopsPlugin` model.
- Provides internal dashboard APIs secured by JWT.

## Tech Stack
- Django 5.2 + Django REST Framework
- MySQL (default) or SQLite (commented in settings)
- JWT auth for internal APIs
- Requests-based integrations (bdCourier, Facebook Graph API)

## Project Structure
- `server/` Django project settings and root URLs
- `app/` ZyncOps plugin services (licenses, couriers, events, fraud checks)
- `trizync/` Internal dashboard + CRM-style APIs (clients, projects, finance)
- `templates/` Server-rendered admin templates
- `static/` and `media/` assets

## API Endpoints (Core)
- `POST /verify-license/` Validate a license key for a given domain
- `POST /order-completed/` Capture completed order data
- `GET|POST /new-key/` Admin-only license generator form
- `POST /create-parcel/` Create courier orders (Redx/Steadfast/Pathao)
- `POST /track-parcel/` Track courier order status
- `POST /fraud-check/` Courier success ratio lookup via bdCourier
- `POST /send-event/` Send Facebook CAPI events
- `GET|POST /fb-graph/` Facebook webhook endpoint
- `GET /api-test/` Simple health response
- `GET /get-events/` List configured Facebook events

## API Endpoints (Internal Dashboard)
- `POST /api/token/` Obtain JWT
- `POST /api/token/refresh/` Refresh JWT
- `POST /api/token/verify/` Verify JWT
- `GET /api/dashboard/` Summary dashboard
- `GET|POST /api/clients/`, `GET|PUT|DELETE /api/clients/<id>/`
- `GET|POST /api/services/`, `GET|PUT|DELETE /api/services/<id>/`
- `GET|POST /api/projects/`, `GET|PUT|DELETE /api/projects/<id>/`
- `GET|POST /api/expenses/`, `GET|PUT|DELETE /api/expenses/<id>/`
- `GET|POST /api/payments/`, `GET|PUT|DELETE /api/payments/<id>/`

## Setup (Local)
1. Create and activate a virtual environment.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure the database in `server/settings.py`.
   - Default is MySQL with local credentials. Update `NAME`, `USER`, `PASSWORD`, and `HOST` to match your environment.
4. Run migrations:
   ```bash
   python manage.py migrate
   ```
5. (Optional) Create a superuser for `/admin/`:
   ```bash
   python manage.py createsuperuser
   ```
6. Run the server:
   ```bash
   python manage.py runserver 0.0.0.0:8000
   ```

## Docker (Dev Container)
`docker-compose.yml` provides a Python container shell. It does not run the server by default.
```bash
docker compose up -d
```
Then exec into the container and run the same install/migrate/run steps.

## Notes and Operational Details
- License validation expects `license_key` + `domain` and checks `License.is_valid()`.
- Courier integrations are implemented in `app/couriers/`.
- Facebook events are sent via the Graph API and logged in `FacebookEventRequest`.
- `media/` stores uploaded plugin ZIP files; `ZyncopsPlugin` holds the latest one.
- `DEBUG` is `False` by default and `ALLOWED_HOSTS` is `*` in `server/settings.py`.

## Related Plugin
The WooCommerce plugin uses this server for license validation, courier automation, fraud checks, and event dispatch.

