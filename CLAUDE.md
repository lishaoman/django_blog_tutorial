# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Django blog tutorial project (Chinese language) demonstrating best practices for building a blog application. The codebase serves as both a working application and educational material.

**Tech Stack:** Python 3.7, Django 2.2.13, Bootstrap 4.1.3, SQLite

## Development Commands

```bash
# Setup virtual environment
python -m venv env
env\Scripts\activate.bat  # Windows
source env/bin/activate  # Linux/Mac

# Install dependencies
pip install -r requirements.txt

# Database migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Run development server
python manage.py runserver

# Run tests
python manage.py test

# Run tests with coverage
pip install coverage
coverage run manage.py test
coverage report
```

**Default admin credentials:** dusai / adminpassword

## Architecture

### Project Structure
```
my_blog/          # Django project configuration (settings, urls, wsgi)
article/          # Blog posts, columns, tags, search
comment/          # Threaded comments (MPTT)
userprofile/      # User profiles, auth, avatars
notice/           # Notification system wrapper
templates/        # HTML templates (base template inheritance)
static/           # CSS, JS, images (Bootstrap, jQuery, CKEditor, Prism)
media/            # User uploads (article images, avatars)
```

### Key Patterns

- **Views:** Uses both function-based views (FBVs) and class-based views (CBVs). CBVs use mixins for shared behavior.
- **Templates:** Base template inheritance pattern. `templates/base.html` is the root template with header/footer includes.
- **Models:**
  - `ArticlePost` - Blog posts with tags (django-taggit), columns, markdown support
  - `Comment` - Threaded comments using django-mptt for tree structure
  - `Profile` - Extends Django User model via OneToOne relationship
- **URL Namespacing:** All apps use namespaced URLs (e.g., `article:article_detail`, `userprofile:login`)

### Third-Party Integrations

- **django-allauth:** Social authentication (GitHub, Weibo configured)
- **django-ckeditor:** Rich text editing with code highlighting (Prism plugin)
- **django-mptt:** Modified Preorder Tree Traversal for nested comments
- **django-taggit:** Tagging system for articles
- **django-notifications-hq:** Notification system
- **django-password-reset:** Password recovery flow

### Static Files Setup

Static files are in `/static/`. In production, collect with:
```bash
python manage.py collectstatic
```
Files are collected to `collected_static/`.

### Media Files

User uploads stored in `media/` directory:
- `media/article/` - Article header images
- `media/avatar/` - User avatars

## Important Notes

- **DEBUG = True** in settings - must be False for production
- **SECRET_KEY** is exposed - change for production
- Email settings (SMTP) are placeholders requiring configuration
- Time zone is set to `Asia/Shanghai`