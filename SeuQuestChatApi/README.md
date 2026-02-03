# SEUQuest API

SEUQuest is a Django REST API that powers a university-focused chatbot. It provides JWT-authenticated endpoints for chat, feedback collection, and (admin-only) training/retraining against a Qdrant-backed knowledge store.

![SEUQuest logo](https://github.com/tanjim750/SeuQuest/blob/main/seu_logo.png)

**Highlights**
- JWT authentication for all API calls.
- Chat endpoint with conversation-mode routing and metadata filtering.
- Feedback collection tied to conversation IDs.
- Admin-only training and retraining endpoints for knowledge updates.
- Qdrant vector store + LangChain-based retrieval + HuggingFace embeddings.

**Project Layout**
- `SeuQuest/`: Django project settings/urls.
- `app/`: API endpoints, models, migrations.
- `Seuquest_bot/`: chatbot + vector store utilities.
- `requirements.txt`: Python dependencies (note: not all runtime deps are listed here).

**Key Concepts**
- **Conversation**: Stored in the DB with `human_query`, `bot_response`, `feedback`, `description`, and `metadata`.
- **Conversation modes**: The API uses `conversation_mode` to control metadata filters. Expected values include `general` or one of:
  `cse_dept`, `eee_dept`, `pharmacy_dept`, `textile_dept`, `architecture_dept`, `bba_dept`, `law_dept`, `english_dept`, `bangla_dept`, `economics_dept`, `mds_dept`.
- **Vector store**: Qdrant is used for retrieval and updates; FAISS helpers exist but are not wired to API endpoints.

**Setup (Local Dev)**
1. Create and activate a virtualenv.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Install the spaCy model used by the text cleaner:

```bash
python -m spacy download en_core_web_md
```

4. Configure Qdrant credentials.
   - Currently hardcoded in `app/api_view_fn/chat_api.py`, `app/api_view_fn/train_api.py`, and `app/api_view_fn/retrain_api.py`.
   - For production, move these to environment variables and rotate the keys.

5. Run migrations and create a superuser:

```bash
python manage.py migrate
python manage.py createsuperuser
```

6. Start the server:

```bash
python manage.py runserver
```

**Authentication**
All API endpoints use JWT authentication (`rest_framework_simplejwt`). Obtain a token pair and include the access token in the `Authorization` header:

```
Authorization: Bearer <access_token>
```

**API Endpoints**
Base prefix: `/api/`

1. `POST /api/token`
- Obtain JWT access/refresh tokens.

2. `POST /api/token/refresh/`
- Refresh the access token.

3. `POST /api/verify/`
- Verify a token.

4. `GET /api/chat/`
- Query params:
  - `query` (required): user question.
  - `conversation_mode` (required): see list above.
  - `metadatas` (optional): metadata object; if omitted, the server derives filters from `conversation_mode` and keywords.
- Returns the bot answer and conversation ID.

5. `POST /api/chat-feedback/`
- Form data:
  - `id` (required): conversation ID.
  - `rating` (required): feedback value.
  - `description` (optional): free-text feedback.

6. `POST /api/train-bot/?conversation_mode=...`
- Admin only (requires `is_superuser`).
- Form data:
  - `data` (required): training text.

7. `POST /api/retrain-bot/`
- Admin only (requires `is_superuser`).
- Form data:
  - `id` (required): conversation ID.
  - `data` (required): corrective context to update existing Qdrant payload.

**Notes and Caveats**
- The project uses SQLite by default (`db.sqlite3`).
- Secrets (Django `SECRET_KEY`, Qdrant URL/API key) are committed in the repo. Rotate and move to environment variables for production.
- `requirements.txt` includes many global/system packages; some runtime dependencies used in code (e.g., `langchain`, `qdrant-client`, `transformers`, `torch`, `spacy`) may need to be added explicitly for clean deployments.

**License**
Proprietary. Contact the maintainers for commercial licensing details.

