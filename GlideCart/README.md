# GlideCart

GlideCart is a Django backend with a Chrome extension that manages access to ChatGPT projects for subscribed users. The backend handles accounts, access keys, device limits, and settings; the extension enforces access by syncing cookies and restricting ChatGPT UI elements based on each account's permissions.

Production backend: `https://extension.glidecartbd.com/`

## Project Structure
- `Backend/glideCart/`: Django project
- `Backend/glideCart/app/`: Core app (models, views, migrations, templates)
- `Extension/GPT-Extension/`: Chrome Extension (Manifest V3)
- `Extension/GPT-Extension-v*.zip`: Packaged extension builds

## Core Features
- Account management with roles (`admin`, `user`), device limits, expiry, and blocking
- Admin dashboard and landing pages in Django templates
- Extension login/logout and access verification
- Extension injects content script on `chatgpt.com` to restrict navigation and features

## API Endpoints
Base URL: `https://extension.glidecartbd.com/`
- `POST /api/account/create/`: Admin creates a user account (requires admin key + project URL + cookies)
- `POST /api/account/login/`: User login by access key
- `POST /api/account/logout/`: User logout
- `POST /api/account/verify/`: Validate access key, returns account details + settings
- `POST /api/account/update/`: Admin updates stored cookies/access
- `GET /r/<key>/`: Deactivate or block user when extension is removed

## Backend Setup (Local)
1. Create a virtual environment and install dependencies.
   `python -m venv .venv`
   `source .venv/bin/activate`
   `pip install -r Backend/glideCart/requirements.txt`

2. Run migrations and start the server.
   `cd Backend/glideCart`
   `python manage.py migrate`
   `python manage.py createsuperuser`
   `python manage.py runserver`

3. Open the admin site at `http://127.0.0.1:8000/admin/`.

Notes:
- The project uses SQLite by default (`Backend/glideCart/db.sqlite3`).
- `ALLOWED_HOSTS` is set to `*` and `DEBUG` is `True` in `Backend/glideCart/glideCart/settings.py`.

## Extension Setup (Local)
1. Open Chrome and go to `chrome://extensions`.
2. Enable Developer Mode.
3. Click "Load unpacked" and select `Extension/GPT-Extension`.
4. The API base URL is defined in `Extension/GPT-Extension/utils/apiurl.js`.

## How It Works
- Admins log in via the extension and can create user accounts tied to a specific ChatGPT project URL.
- The extension stores login details in local storage and syncs cookies from the admin session to the user session.
- The content script hides ChatGPT UI elements (projects/history/inbox) based on server-provided settings.
- If the extension is removed or ping fails, the backend can deactivate or block the account via the `/r/<key>/` endpoint.

## Deployment Notes
- The production backend currently runs at `https://extension.glidecartbd.com/`.
- For production, set `DEBUG = False`, restrict `ALLOWED_HOSTS`, and configure a production database and static files.

## License
Proprietary. All rights reserved.

