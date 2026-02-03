# Nolokayon

Nolokayon is an e-commerce backend built with Django REST Framework plus a small frontend area that includes a Vite + React starter and static HTML templates. The backend exposes product, homepage content, and checkout APIs and sends order confirmation emails.

**What’s in here**
- Django backend with SQLite, Django admin, and REST-style JSON endpoints.
- Product, category, coupon, visitor, and checkout data models.
- Email notifications for orders using a Django HTML template.
- Frontend workspace under `fronend/` (note the folder name is spelled this way in the repo).

**Project Structure**
- `backend/` Django project and app code, SQLite DB, templates, and requirements.
- `backend/app/` Models, serializers, views.
- `backend/templates/` Email template for order notifications.
- `fronend/nolokayon/` Vite + React app scaffold.
- `fronend/login.html` and `fronend/product-details/index.html` Static template pages.

## Backend

**Key Features**
- Home, header, hero, banners, contact, and footer content endpoints.
- Products with categories, optional discounts, galleries, and “other details”.
- Coupon validation by code and optional category targeting.
- Checkout flow that creates orders, calculates discounts, and sends emails.
- Order lookup by order ID.

**API Endpoints**
- `GET /home` Home page payload (header, hero, banners, contact, products, categories).
- `GET /products` List products.
- `GET /product-details/<id>` Product detail and increments view count.
- `GET /about` About content.
- `GET /contact` Contact content.
- `GET /footer` Footer content and products.
- `GET /discount-coupon` List coupons.
- `POST /discount-coupon` Validate coupon and compute discount for a product.
- `GET /visitor/<visitor_id>` Fetch a visitor.
- `POST /visitor/<visitor_id>` Create or increment visitor visits.
- `GET /checkout?visitor_id=<id>` List checkout items for a visitor.
- `POST /checkout` Create an order and send notification emails.
- `GET /order-details/<order_id>` Lookup an order.

**Local Setup**
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements.txt
python backend/manage.py migrate
python backend/manage.py runserver
```

**Django Admin**
```bash
python backend/manage.py createsuperuser
```
Then visit `http://localhost:8000/admin/`.

**Email Configuration**
The project is currently configured to use SMTP with Gmail in `backend/backend/settings.py`. For real deployments, move credentials to environment variables and use an app password.

## Frontend

**Vite + React App**
The React app lives in `fronend/nolokayon/` and currently contains the default Vite starter UI.

```bash
cd fronend/nolokayon
npm install
npm run dev
```

**Static Templates**
- `fronend/login.html`
- `fronend/product-details/index.html`

These HTML files are standalone templates and are not wired to the React app or backend yet.

## Notes
- The backend uses SQLite by default via `backend/db.sqlite3`.
- CORS is currently set to allow all origins in development.
- CSRF middleware is disabled in settings; re-enable it for production.

## Suggested Next Steps
- Replace hardcoded email credentials with environment variables.
- Add production settings (allowed hosts, static/media hosting, secure cookies).
- Connect the React app to the backend APIs.

