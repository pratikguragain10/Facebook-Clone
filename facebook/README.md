# Connectly 

---

**Connectly** is a social networking web application built with **Django**, inspired by modern social platforms.  
It supports user profiles, posts with media, friendships, and interactive social features.

---

## Features

- User authentication (Signup / Login / Logout)
- User profiles  
  - Profile picture & cover photo  
  - Bio, education, work, location
- Create posts with text, images & videos
- Like posts
- Comment & reply to comments
- Friend requests (send, accept, reject, remove)
- Media storage powered by **Cloudinary**
- Persistent database using **PostgreSQL**
- Deployed on **Render**

---

## Tech Stack

### Backend
- Python
- Django

### Database
- PostgreSQL

### Media Storage
- Cloudinary

### Deployment
- Render
- Gunicorn
- Whitenoise

---

## 📂 Project Structure

```text
facebook/
├── facebook/           # Django project settings
├── user/               # Main application
├── templates/          # HTML templates
├── static/             # Static assets
├── manage.py
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Environment Variables

Set the following environment variables in **Render → Environment**:

```env
DJANGO_SECRET_KEY=your-secret-key
DEBUG=False

ALLOWED_HOSTS=your-render-url.onrender.com
CSRF_TRUSTED_ORIGINS=https://your-render-url.onrender.com

DATABASE_URL=postgresql://...

CLOUDINARY_CLOUD_NAME=xxxx
CLOUDINARY_API_KEY=xxxx
CLOUDINARY_API_SECRET=xxxx
CLOUDINARY_URL=cloudinary://xxxx
```
---

## Local Setup

```bash
# Clone repository
git clone https://github.com/yourusername/connectly-social.git
cd connectly-social

# Create virtual environment
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate      # Windows

# Install dependencies
pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Start development server
python manage.py runserver
```

---

## Deployment Notes

- PostgreSQL ensures **data persistence** on Render
- Cloudinary handles **all media uploads**
- Whitenoise serves static files efficiently
- HTTPS enabled automatically by Render

---

## Security

- CSRF protection enabled
- Secrets stored using environment variables
- Secure cookies enabled in production
- No sensitive data committed to Git

---

## Future Improvements

- Django built-in authentication
- Password hashing & reset
- Notifications
- Real-time chat
- Custom domain support

---