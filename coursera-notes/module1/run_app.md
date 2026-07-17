# Clone & run App

To run this Django project locally (from your GitHub repository), you need to **clone it, install dependencies, configure the database, and start Django**.

## Option 1 — Run locally from GitHub (recommended)

### 1. Clone the repository

Open a terminal:

```bash
git clone https://github.com/lindangulopez/blango.git
```

Go into the project:

```bash
cd blango
```

You should see:

```text
manage.py
db.sqlite3
blog/
blango/
templates/
```

---

## 2. Create a Python virtual environment

A virtual environment keeps Django dependencies isolated.

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### macOS/Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

You should see something like:

```text
(venv) $
```

---

## 3. Install Django

Check if the project has a requirements file:

```bash
ls
```

If you see:

```text
requirements.txt
```

run:

```bash
pip install -r requirements.txt
```

If there is no requirements file, install Django:

```bash
pip install django
```

You can verify:

```bash
python -m django --version
```

---

## 4. Set up the database

This repository already contains:

```text
db.sqlite3
```

so the database may already exist.

Run migrations anyway:

```bash
python manage.py migrate
```

You should see:

```text
Applying migrations...
OK
```

---

## 5. Create an admin user (optional)

To access Django admin:

```bash
python manage.py createsuperuser
```

Example:

```text
Username: admin
Email: admin@example.com
Password:
```

---

## 6. Start the Django server

Run:

```bash
python manage.py runserver
```

You should see:

```text
Starting development server at http://127.0.0.1:8000/
```

Open:

```
http://127.0.0.1:8000/
```

Admin:

```
http://127.0.0.1:8000/admin/
```

---

# Option 2 — Run directly from GitHub Codespaces

If you want a cloud development environment:

1. Open the repository on GitHub.
2. Click:

```
Code
  ↓
Codespaces
  ↓
Create codespace on master
```

GitHub creates a Linux machine with your project.

Then run:

```bash
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

Codespaces will provide a browser preview.

---

# Option 3 — Run with GitHub Actions (testing only)

GitHub Actions does **not normally run your website**. It tests/builds it.

A workflow would look like:

```
Git push
    |
    ↓
GitHub Actions starts
    |
    ↓
Install Python
    |
    ↓
Install Django
    |
    ↓
Run migrations
    |
    ↓
Run tests
    |
    ↓
Pass/fail result
```

For example:

```
.github/
 └── workflows/
      └── django.yml
```

A typical Django workflow:

```yaml
name: Django Test

on:
  push:
    branches:
      - master

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - uses: actions/setup-python@v5
      with:
        python-version: "3.x"

    - run: |
        pip install django
        python manage.py migrate
        python manage.py test
```

---

# Option 4 — Deploy it publicly

To make it available on the internet, you would deploy it to a service such as:

* [Render](https://render.com?utm_source=chatgpt.com)
* [Railway](https://railway.app?utm_source=chatgpt.com)
* [PythonAnywhere](https://www.pythonanywhere.com?utm_source=chatgpt.com)
* [Amazon Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/?utm_source=chatgpt.com)

The deployment workflow becomes:

```
GitHub repository
        |
        ↓
Hosting provider
        |
        ↓
Install dependencies
        |
        ↓
Run migrations
        |
        ↓
Start Gunicorn/Django server
        |
        ↓
Public website
```

---

## Before running, check this project specifically

Because this is an older Coursera/Codio Django project, I would check:

### Django version

Run:

```bash
python -m django --version
```

Older training projects sometimes require older Django versions.

### Installed apps

Open:

```
blango/settings.py
```

Look for:

```python
INSTALLED_APPS = [
    'blog',
]
```

### Database

Check:

```
db.sqlite3
```

If it exists, you can usually run immediately.

---

## Fastest path for this exact repo

```bash
git clone https://github.com/lindangulopez/blango.git
cd blango

python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

pip install django

python manage.py migrate

python manage.py runserver
```

Then visit:

```
http://127.0.0.1:8000/
```

That should launch the blog locally.
