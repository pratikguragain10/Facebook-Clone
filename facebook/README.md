# Connectly 🌐

**Connectly** is a social networking web application built with **Django**, inspired by modern social platforms.  
It supports user profiles, posts with media, friendships, and real-time interactions.

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
- Photo gallery powered by Cloudinary
- Persistent database using PostgreSQL
- Deployed on Render

---

## Tech Stack

**Backend**
- Python
- Django

**Database**
- PostgreSQL

**Media Storage**
- Cloudinary

**Deployment**
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
