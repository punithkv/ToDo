<div align="center">

# ✅ ToDo
### *A clean, server-rendered task manager built with Django*

![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-4.1.7-092E20?logo=django&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?logo=bootstrap&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-Default%20DB-003B57?logo=sqlite&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)

</div>

---

## 📖 Table of Contents
- [Overview](#-overview)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Folder Structure](#-folder-structure)
- [Setup](#-setup)
- [Usage](#-usage)
- [Routes](#-routes)
- [Deployment](#-deployment)
- [Roadmap](#-roadmap)

---

## 🔭 Overview

**ToDo** is a straightforward, no-JavaScript-framework task manager. Django handles routing, persistence, and server-side rendering; Bootstrap provides the styling. Tasks can be created, marked done/undone, edited, and deleted — all through classic HTML forms and full-page redirects, no client-side API layer required.

**What it does:**
- Add tasks from a simple input form
- View active and completed tasks in separate lists, side by side
- Mark tasks as done or undone with a single click
- Edit or delete existing tasks
- Manage tasks directly through Django's built-in admin panel

---

## 🏗 Architecture

```
┌─────────────────────┐
│      Browser            │
│  (Bootstrap + forms)     │
└─────────┬─────────────────┘
          │ HTTP (GET/POST)
          ▼
┌─────────────────────────┐
│     todo_main/                │
│  urls.py → views.py             │
│  (project-level: home page)       │
└─────────┬─────────────────────────┘
          │ includes
          ▼
┌─────────────────────────┐
│        todo/                    │
│  urls.py → views.py             │
│  (app-level: add/edit/delete/     │
│   mark done/undone)                 │
└─────────┬─────────────────────────┘
          ▼
┌─────────────────────────┐
│      models.py                  │
│  Task (task, is_completed,        │
│  created_at, updated_at)             │
└─────────┬─────────────────────────┘
          ▼
┌─────────────────────────┐
│      db.sqlite3                 │
└─────────────────────────┘
```

The project follows a standard two-level Django layout: `todo_main/` is the project (settings, root URLs, the home view), and `todo/` is the app that owns the `Task` model and all task-mutation logic.

---

## 🧰 Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Backend framework** | Django 4.1.7 | Routing, ORM, admin panel, templating |
| **Database** | SQLite | Default `db.sqlite3` — zero setup, file-based |
| **Frontend** | Django Templates + Bootstrap 5.3 (CDN) | Server-rendered HTML, no build step |
| **Icons** | Font Awesome (CDN) | Action icons (check, edit, delete) |
| **WSGI server** | gunicorn | Production-grade serving, referenced in `Procfile` |
| **Containerization** | Docker | `Dockerfile` for containerized deployment |

---

## 🗂 Folder Structure

```
ToDo-main/
├── manage.py                 # Django's command-line entry point
├── todo_main/                    # Project package
│   ├── settings.py                 # App config, installed apps, database
│   ├── urls.py                       # Root URL routing (home + admin + todo/)
│   ├── views.py                        # home() — renders active + completed tasks
│   ├── wsgi.py / asgi.py                 # Server entry points
│   └── __init__.py
├── todo/                          # Task-management app
│   ├── models.py                    # Task model
│   ├── views.py                        # addTask, mark_as_done/undone, edit_task, delete_task
│   ├── urls.py                           # App-level routes
│   ├── admin.py                            # Task registered in Django admin
│   └── migrations/                           # Database schema history
├── templates/
│   ├── home.html                    # Main task list page
│   └── edit_task.html                  # Edit-task form
├── static/css/styles.css               # Custom styles
├── db.sqlite3                            # SQLite database (dev)
├── requirements.txt                        # Python dependencies
├── Procfile                                  # For gunicorn-based deployment (e.g. Heroku)
└── Dockerfile                                  # Container build definition
```

---

## 🚀 Setup

### Prerequisites
- Python 3.11+

### 1. Create and activate a virtual environment
```bash
python -m venv env
source env/Scripts/activate     # Git Bash on Windows
# or: source env/bin/activate   # macOS/Linux
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Apply migrations
```bash
python manage.py migrate
```

### 4. (Optional) Create an admin user
```bash
python manage.py createsuperuser
```

---

## ▶️ Usage

### Run the development server
```bash
python manage.py runserver
```
Visit **http://localhost:8000** to use the app, or **http://localhost:8000/admin** to manage tasks through Django's admin panel (requires a superuser).

---

## 🛤 Routes

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/` | Home page — lists active and completed tasks |
| `POST` | `/todo/addTask/` | Create a new task |
| `GET` | `/todo/mark_as_done/<pk>/` | Mark a task as completed |
| `GET` | `/todo/mark_as_undone/<pk>/` | Revert a task to active |
| `GET`/`POST` | `/todo/edit_task/<pk>/` | View or submit the edit form for a task |
| `GET` | `/todo/delete_task/<pk>/` | Delete a task |
| — | `/admin/` | Django admin panel |

---

## 📦 Deployment

**Via Docker:**
```bash
docker build -t todo-app .
docker run -p 8000:8000 todo-app
```

**Via Procfile (Heroku or similar):**
The included `Procfile` runs `gunicorn todo_main.wsgi:application` — compatible with any platform that reads a Procfile.

> ⚠️ Before deploying: set `DEBUG = False`, replace the hardcoded `SECRET_KEY` in `settings.py` with an environment variable, and restrict `ALLOWED_HOSTS` from `['*']` to your actual domain.

---

## 🛣 Roadmap

- [ ] Move `SECRET_KEY` and `DEBUG` into environment variables
- [ ] Restrict `ALLOWED_HOSTS` for production
- [ ] Add due dates / priority levels to tasks
- [ ] Add per-user accounts so tasks aren't global
- [ ] Add a REST API layer (Django REST Framework) for a future frontend rework
- [ ] Add automated tests in `tests.py` (currently empty)

---

<div align="center">

A simple, dependency-light Django project — built as a clean example of server-rendered CRUD.

</div>