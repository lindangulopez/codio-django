# Debugging Django Blog Comments Feature — Lessons Learned

## Goal

Add a comment system to a Django blog post page using:

* Django Forms
* Bootstrap styling
* Django Crispy Forms
* Templates
* Models
* Views

---

# 1. Understanding the Django Request Flow

A Django page follows this path:

```
Browser
   ↓
URL pattern (urls.py)
   ↓
View function (views.py)
   ↓
Model/database
   ↓
Template (.html)
   ↓
Browser output
```

When debugging, always check in this order.

---

# 2. Creating the Comment Form

Create:

```
blog/forms.py
```

Example:

```python
from django import forms
from blog.models import Comment


class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ["content"]
```

The form connects the HTML input to the Comment model.

---

# 3. Adding the Form to the View

In:

```
blog/views.py
```

The view must:

1. Check if the user is logged in.
2. Create an empty form for GET requests.
3. Validate submitted data for POST requests.
4. Save the comment with extra information.

Example flow:

```python
if request.method == "POST":
    comment_form = CommentForm(request.POST)

    if comment_form.is_valid():
        comment = comment_form.save(commit=False)
        comment.content_object = post
        comment.creator = request.user
        comment.save()
```

Important concept:

`commit=False`

means:

> Create the object but do not save it yet.

This allows adding missing fields before writing to the database.

---

# 4. Template Problems and How to Debug Them

## Error:

```
TemplateDoesNotExist:
blog/post-detail.html
```

Meaning:

Django found the view but could not find the HTML file.

Check:

```
templates/
└── blog/
    └── post-detail.html
```

The folder name and filename must match exactly.

---

# 5. URL Problems

Example:

```
No Post matches the given query
```

Meaning:

The URL worked, but Django could not find the database object.

Check:

```python
Post.objects.all()

Post.objects.values("title", "slug")
```

Example output:

```
title: test1
slug: test1
```

Then use:

```
/post/test1/
```

not:

```
/post/my-first-post/
```

---

# 6. Custom Template Tag Problems

Error:

```
Invalid block tag: 'row'
```

Cause:

The template contained:

```django
{% row %}
{% col %}
```

but Django did not know those tags.

Solutions:

Option 1:
Load the correct custom template tags:

```django
{% load blog_extras %}
```

Option 2:
Replace custom tags with normal Bootstrap:

```html
<div class="row">
    <div class="col">
    </div>
</div>
```

Normal Bootstrap HTML is easier to debug.

---

# 7. Crispy Forms Setup

Install:

```bash
pip install crispy-bootstrap5
```

In:

```
settings.py
```

Add:

```python
INSTALLED_APPS = [
    ...
    "crispy_forms",
    "crispy_bootstrap5",
]
```

Add:

```python
CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"
```

---

# 8. Updating the Template for Crispy Forms

Before:

```django
{{ comment_form.as_p }}
```

This creates normal Django HTML.

After:

```django
{% load crispy_forms_tags %}

{{ comment_form|crispy }}
```

Crispy automatically adds Bootstrap classes.

---

# 9. Displaying Comments

Template:

```
templates/blog/post-comments.html
```

The template should:

1. Loop through existing comments:

```django
{% for comment in post.comments.all %}
```

2. Show comment information:

```django
{{ comment.creator }}
{{ comment.content }}
```

3. Display the form:

```django
{{ comment_form|crispy }}
```

---

# 10. HTML Content Appearing as Text

Problem:

Browser shows:

```
<h1>Hello Blog</h1>
```

instead of rendering:

```
Hello Blog
```

Cause:

Django escapes HTML by default.

Fix:

File:

```
templates/blog/post-detail.html
```

Change:

```django
{{ post.content }}
```

to:

```django
{{ post.content|safe }}
```

Use carefully because `safe` allows HTML rendering.

---

# 11. General Django Debugging Checklist

When something breaks:

## Step 1 — Read the error

Examples:

```
TemplateDoesNotExist
```

→ Check files and folders.

```
No Post matches query
```

→ Check database values.

```
Invalid block tag
```

→ Check template tags.

```
ModuleNotFoundError
```

→ Install package or check imports.

---

## Step 2 — Check the database

Use Django shell:

```bash
python3 manage.py shell
```

Example:

```python
from blog.models import Post

Post.objects.all()

Post.objects.values("title", "slug")
```

---

## Step 3 — Check the three main files

For a Django feature:

### Model

```
blog/models.py
```

Defines database structure.

### View

```
blog/views.py
```

Controls logic.

### Template

```
templates/blog/*.html
```

Controls display.

---

# Final Working Structure

```
blango
│
├── blog
│   ├── forms.py
│   ├── models.py
│   ├── views.py
│
├── templates
│   └── blog
│       ├── index.html
│       ├── post-detail.html
│       └── post-comments.html
│
└── blango
    ├── settings.py
    └── urls.py
```

Main lesson:

## Django debugging becomes easier when you follow the request path:
> URL → View → Model → Template → Browser.
