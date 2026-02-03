# Agro Farm (Multi-Vendor Agro E-commerce)

Agro Farm is a Django-based multi-vendor e-commerce platform focused on agricultural products. It includes a customer-facing storefront, vendor/admin dashboard, order management, and an integrated LLM agent that can chat with customers, recommend products (including fresh food suggestions), and help initiate orders.

## Features
- Multi-vendor product catalog with categories, pricing, discounts, reviews, and images.
- Shopping cart, checkout, order history, and order tracking.
- Address book and profile management.
- Blog and content pages (about, services, testimonials, contact).
- Admin/vendor dashboard with sales stats, notifications, and messages.
- Product recommendation engine using implicit feedback (visits + purchases).
- LLM agent endpoint for customer chat, product recommendations, and order assistance.
- CKEditor for rich content editing.

## Tech Stack
- Django (ASGI via `daphne`)
- SQLite (default; PostgreSQL config stub exists)
- Pandas, SciPy, implicit ALS for recommendations
- Google GenAI SDK for LLM agent

## Project Structure (Key Parts)
- `agro_farm/` Django project settings/urls/asgi
- `farm/` customer storefront, product/order logic, LLM chat endpoint
- `admin_dashboard/` vendor/admin dashboard, analytics, notifications
- `farm/templates/` HTML templates
- `farm/static/` CSS assets
- `img/` media uploads

## Setup

### 1) Clone and enter the project
```bash
git clone https://github.com/tanjim750/agro-farm
cd agro_farm
```

### 2) Create and activate a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate
```

### 3) Install dependencies
```bash
pip install -r requirements.txt
```

### 4) Configure environment (recommended)
The current settings file includes hard-coded secrets. For production, move these to environment variables.

Recommended environment variables:
- `DJANGO_SECRET_KEY`
- `DJANGO_DEBUG`
- `DJANGO_ALLOWED_HOSTS`
- `EMAIL_HOST_USER`
- `EMAIL_HOST_PASSWORD`
- `GOOGLE_API_KEY` (for Gemini/LLM agent)

You can add a `.env` loader (for example `python-dotenv`) if you prefer.

### 5) Run migrations
```bash
python manage.py migrate
```

### 6) Create a superuser
```bash
python manage.py createsuperuser
```

### 7) Run the server
```bash
python manage.py runserver
```

Open the site:
- Storefront: `http://127.0.0.1:8000/`
- Admin: `http://127.0.0.1:8000/mysite/admin`
- Vendor dashboard: `http://127.0.0.1:8000/admin`

## LLM Agent (Customer Assistant)
The LLM agent is exposed at:
- `GET/POST /chat-agent/`

It can:
- chat with customers,
- recommend products (including fresh food),
- assist with order creation workflows.

Tokens and system prompts are stored in the database:
- `GeminiAccessToken` and `LLMSystemPrompt` in `farm/models.py`.

## Recommendation Engine
The product listing page uses an implicit feedback ALS model built from:
- `visitedProduct`
- `Purchase`

See the logic in `farm/views.py` under the `products` view.

## Notes and Caveats
- `DEBUG = True` and `ALLOWED_HOSTS = ['*']` are set in `agro_farm/settings.py` for development.
- Email credentials and secret keys are currently in the settings file; move them to env vars before deploying.
- SQLite is used by default; PostgreSQL config is stubbed in `settings.py`.

## Common Commands
```bash
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

## License
See `farm/templates/LICENSE.txt` for template licensing.
