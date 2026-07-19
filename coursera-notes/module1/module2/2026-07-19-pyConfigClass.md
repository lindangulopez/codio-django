# Django Configurations Setup — Beginner Guide

## Goal

We are going to change Django's `settings.py` so that:

* Settings are organized inside a configuration class.
* Environment variables can later be used safely.
* Development and production settings can be separated.

You have already completed the installation step.

---

# Step 1 — Open the terminal and go to your Django project

Your project is located in:

```bash
~/workspace/blango
```

Go there:

```bash
cd ~/workspace/blango
```

You should see files like:

```text
manage.py
blog/
blango/
db.sqlite3
templates/
```

---

# Step 2 — Install Django Configuration packages

Run:

```bash
pip3 install django-configurations dj-database-url
```

You should see:

```text
Successfully installed django-configurations ... dj-database-url ...
```

This means the installation worked.

These packages do:

* `django-configurations`
  → lets Django settings use classes.

* `dj-database-url`
  → allows databases to be configured using URLs later.

---

# Step 3 — Open the Django settings file

The file you need is:

```
blango/blango/settings.py
```

Important:

There are two `blango` folders:

```
workspace
└── blango              ← project folder
    └── blango          ← Django settings folder
        └── settings.py ← EDIT THIS FILE
```

Open:

```
blango/settings.py
```

---

# Step 4 — Add the Configuration import

At the top of `settings.py` you should see:

```python
from pathlib import Path
import os
```

Change it to:

```python
from pathlib import Path
import os
from configurations import Configuration
```

This imports the Django Configurations package.

---

# Step 5 — Create the Dev configuration class

Before:

```python
BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'your-secret-key'

DEBUG = True
```

After:

```python
class Dev(Configuration):

    BASE_DIR = Path(__file__).resolve().parent.parent

    SECRET_KEY = 'your-secret-key'

    DEBUG = True
```

The important parts:

* Add:

```python
class Dev(Configuration):
```

* Indent all settings underneath it.

---

# Step 6 — Move all settings inside the class

Every Django setting needs to be inside:

```python
class Dev(Configuration):
```

Examples:

Before:

```python
TIME_ZONE = 'UTC'
```

After:

```python
    TIME_ZONE = 'UTC'
```

Before:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
]
```

After:

```python
    INSTALLED_APPS = [
        'django.contrib.admin',
    ]
```

Everything gets 4 spaces.

---

# Step 7 — Save the file

Save:

```
settings.py
```

Make sure there are no indentation errors.

---

# Step 8 — Understand what changed

Before:

```python
DEBUG = True
```

Django directly reads the setting.

After:

```python
class Dev(Configuration):
    DEBUG = True
```

The settings now belong to a configuration called `Dev`.

Later, you can create:

```python
class Production(Dev):
    DEBUG = False
```

This allows different settings for development and production.

---

# Step 9 — Answer the reading question

Question:

> Why is the settings.py file converted to be a class?

Choose:

✅ **In order to make use of the Django Configurations package, you need to make a class that inherits from Configurations.**

---

# Your current progress

You have completed:

✅ Entered Django project folder
✅ Installed `django-configurations`
✅ Installed `dj-database-url`

Your next task:

➡️ Edit:

```
blango/blango/settings.py
```

and convert it into:

```python
class Dev(Configuration):
```

Do not edit Codio settings or terminal settings files. Only edit the Django file.
