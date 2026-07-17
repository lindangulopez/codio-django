# Blango HTML Django Setup Notes

## Bootstrap Setup

To install Bootstrap in the project, add the Bootstrap CSS link inside the `<head>` section of `base.html`:

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprGAn1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
```

Add the Bootstrap JavaScript bundle before the closing `</body>` tag:

```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMF0sXsLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
```

## Global Templates

Create the global templates folder:

```bash
mkdir templates
```

Project structure:

```
blango/
├── manage.py
├── blango/
├── blog/
└── templates/
    └── base.html
```

Create:

```
templates/base.html
```

Example:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet">

    <title>Blango</title>
</head>

<body>

<h1>Hello, world!</h1>

{% block content %}
{% endblock %}

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"></script>

</body>
</html>
```

---

# Configure Django Templates

Open:

```
blango/settings.py
```

Find:

```python
'DIRS': [],
```

Change it to:

```python
'DIRS': [BASE_DIR / 'templates'],
```

Keep:

```python
'APP_DIRS': True,
```

This allows Django to find templates inside app directories.

---

# Blog Template Setup

Create the blog template folder:

```bash
mkdir -p templates/blog
```

Create:

```
templates/blog/index.html
```

Example:

```html
{% extends "base.html" %}

{% block content %}
<h2>Index Template</h2>
{% endblock %}
```

Important:

The file must be:

```
templates/blog/index.html
```

NOT:

```
blog/__pycache__/index.html
```

`__pycache__` contains Python cache files and Django does not load templates from there.

---

# Blog View

Open:

```
blog/views.py
```

Add:

```python
from django.shortcuts import render

def index(request):
    return render(request, "blog/index.html")
```

---

# URL Configuration

Open:

```
blango/urls.py
```

Update:

```python
from django.contrib import admin
from django.urls import path
import blog.views

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", blog.views.index),
]
```

---

# Checking Template Locations

If Django shows:

```
TemplateDoesNotExist at /
blog/index.html
```

Check where the template actually exists:

```bash
find . -name "index.html"
```

Correct result:

```
./templates/blog/index.html
```

Incorrect result:

```
./blog/__pycache__/index.html
```

Move files into the correct location:

```bash
mkdir -p templates/blog
mv blog/__pycache__/index.html templates/blog/index.html
```

Verify:

```bash
find . -name "index.html"
```

---

# Codio URL Setup

To get the correct Codio URL, first go to the project folder:

```bash
cd ~/workspace/blango
```

Display your Codio hostname:

```bash
echo $CODIO_HOSTNAME
```

Example output:

```
classicescape-brotherfiction
```

Create the Django port URL:

```bash
echo https://$CODIO_HOSTNAME-8000.codio.io
```

Example:

```
https://classicescape-brotherfiction-8000.codio.io
```

---

# Running Django Server

Start Django:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

Open the Codio URL:

```
https://YOUR_CODIO_HOSTNAME-8000.codio.io
```

---

# When to Restart Django

Template changes:

* Creating/moving `.html` files → usually no restart needed.
* Changing `settings.py`, `urls.py`, or Python code → restart the server.

Restart:

```bash
CTRL+C

python3 manage.py runserver 0.0.0.0:8000
```


