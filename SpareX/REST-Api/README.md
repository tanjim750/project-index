# SpareX API

SpareX API is the Django REST backend for the SpareX Mobile app — an e-commerce car parts listing experience focused on Middle East markets. The API handles authentication, user and business profiles, product listings, images, and engagement tracking.

**Tech Stack**
- Python + Django 5.2
- Django REST Framework
- JWT auth via `djangorestframework-simplejwt`
- SQLite (dev default)

**Core Features**
- User signup and JWT login
- Personal and business profiles
- Business-owned product categories and products
- Product image uploads (single and bulk)
- Product engagement tracking (views + time spent)

**Key API Routes**
- `POST /api/signup/` Create user and profile (and business profile if role is `BUSINESS`)
- `POST /api/login/` Obtain JWT access/refresh tokens
- `POST /api/login/refresh/` Refresh access token
- `POST /api/login/verify/` Verify token
- `GET /api/profile/` Retrieve current user profile
- `PUT /api/profile/` Update current user profile
- `POST /api/change-password/` Change password
- `api/categories/` Product categories (business only)
- `api/products/` Products (business only)
- `api/product-images/` Product images
- `POST /api/product-images/bulk/` Bulk image upload
- `api/product/view/` Product engagement tracking

## Installation and Setup (Local)

**Prerequisites**
- Python 3.11+ recommended
- `pip` and `virtualenv`

**Steps**
1. Create and activate a virtual environment.
2. Install dependencies:
   - `pip install django djangorestframework djangorestframework-simplejwt pillow`
3. Run migrations:
   - `python manage.py migrate`
4. (Optional) Create a superuser:
   - `python manage.py createsuperuser`
5. Start the server:
   - `python manage.py runserver`

The API will be available at `http://127.0.0.1:8000/`.

## Installation and Setup (Docker)

**Prerequisites**
- Docker and Docker Compose

**Steps**
1. Start the container:
   - `docker compose up -d`
2. Install Python dependencies inside the container:
   - `docker exec -it django_sparex pip install django djangorestframework djangorestframework-simplejwt pillow`
3. Run migrations:
   - `docker exec -it django_sparex python manage.py migrate`
4. Start the dev server inside the container:
   - `docker exec -it django_sparex python manage.py runserver 0.0.0.0:8000`

The API will be available at `http://localhost:8000/`.

## Configuration Notes
- `DEBUG` is enabled in `sparex/settings.py` by default.
- `ALLOWED_HOSTS` is empty by default and should be set for production.
- The default database is SQLite (`db.sqlite3`).
- Image fields use Django's media system. If you need uploads in dev, add `MEDIA_ROOT`/`MEDIA_URL` in `sparex/settings.py` and configure URL serving accordingly.

## Useful Commands
- Run migrations: `python manage.py migrate`
- Create admin user: `python manage.py createsuperuser`
- Run tests: `python manage.py test`
