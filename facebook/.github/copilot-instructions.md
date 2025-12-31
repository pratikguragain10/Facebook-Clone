<!-- Copilot / AI agent instructions for the Connectly codebase -->
# Repository snapshot

- Framework: Django (single-project, single-app monolith)
- App: `user` implements core models, views and templates
- Storage: Cloudinary for media (CloudinaryField used on model fields)
- DB: `dj-database-url` with PostgreSQL in production, fallback to local sqlite (`db.sqlite3`)
- Deployment: Render / Gunicorn / Whitenoise (see `Procfile`)

# Quick orientation (what to read first)

- Start with these files to understand architecture and conventions:
- - [facebook/settings.py](facebook/settings.py)
- - [user/models.py](user/models.py)
- - [user/views.py](user/views.py)
- - [user/urls.py](user/urls.py)
- - [user/templates/profile.html](user/templates/profile.html)
- - [README.md](README.md)

# Big-picture notes for an AI agent

- This is a Django monolith: the `user` app contains the domain logic (users, posts, friends, likes, comments). There are no microservices.
- Authentication is *session-based* using `request.session['user_id']` and a local `User_Data` model (not Django's `auth.User`) — many views assume that pattern.
- Media fields use `cloudinary.models.CloudinaryField` and production relies on Cloudinary credentials from environment variables.
- Production DB and static handling are configured in `settings.py` via `dj-database-url` and `whitenoise`.
- The codebase mixes django-allauth in settings with custom auth views. Be careful when changing auth flows to avoid breaking either approach.

# Developer workflows & commands

- Local dev (from repository root):

```
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
# For local quick testing set DEBUG to True (env var) when running server
DEBUG=True python manage.py runserver
```

- Production-like tasks:
- - Set `DATABASE_URL` and Cloudinary env vars before running migrations.
- - Run `python manage.py collectstatic --noinput` before starting Gunicorn.
- Start the production server used by Render: `gunicorn facebook.wsgi:application` (see `Procfile`).

# Project-specific conventions & gotchas

- Do not assume Django's `request.user` is configured — the app uses `request.session['user_id']` and often passes a `user` context variable to templates which may be a `User_Data` instance.
- Passwords are stored on `User_Data.password` as plain text in this codebase. Any auth changes should address secure password handling and use Django's auth system or at minimum `make_password`/`check_password`.
- Many views create model instances directly from `request.POST` and `request.FILES` (e.g., creating posts, uploading images). When adding features, reuse existing patterns (validate before saving).
- Media storage: models use CloudinaryField; in DEBUG some code references `settings.MEDIA_ROOT` for static serving — `MEDIA_ROOT` is not required for Cloudinary but may be checked in local static setups.
- Database queries: views use `select_related`, `prefetch_related`, and manual `values_list` to optimize feeds and friend lists — mirror these patterns when adding list endpoints.

# Integration points to watch

- Cloudinary: configured via `CLOUDINARY_*` env vars in `settings.py` and `django-cloudinary-storage` as `DEFAULT_FILE_STORAGE`.
- Database: `dj-database-url` reads `DATABASE_URL`. Local fallback is sqlite (`db.sqlite3`).
- allauth + socialaccount (Google) are present in INSTALLED_APPS, but the app also includes custom signup/login views — changing authentication may require reconciliing both approaches.
- Whitenoise: static file handling in production via `CompressedManifestStaticFilesStorage`.

# Examples (how to reason about code changes)

- To add a new field to posts: update [user/models.py](user/models.py), create migrations, and update templates under `user/templates/*` where posts are rendered.
- To add an API endpoint analogous to existing views: follow `user/urls.py` patterns, use `require_POST` for mutating routes and maintain session checks (`'user_id' in request.session`).

# Safety & testing notes

- Be conservative: changing authentication or the `User_Data` model has wide impact. Run migrations and search for direct attribute access (e.g., `user.password`, `request.session['user_id']`).
- Run `python manage.py migrate` and `python manage.py runserver` locally; for static assets run `collectstatic` if testing whitenoise behavior.

# If you edit this file

- Keep this doc short and specific. When in doubt, reference the files listed in "Quick orientation" and run the local dev setup from README.md.

---
If anything here is unclear or you want the agent to expand an area (tests, auth migration plan, or Cloudinary/local media behavior), ask and I'll iterate.