# Slugify

You are now editing:

```text
blango/blog/admin.py
```

Right now you have:

```python
from django.contrib import admin

# Register your models here.
```

You need to register your `Tag` and `Post` models with the Django admin site.

### 1. Replace the contents of `admin.py` with:

```python
from django.contrib import admin
from blog.models import Tag, Post


admin.site.register(Tag)


class PostAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("title",)}


admin.site.register(Post, PostAdmin)
```

---

### 2. Save the file

Press:

```
Ctrl + S
```

---

### 3. Start the Django server to test it

In the terminal, make sure you are in:

```bash
~/workspace/blango
```

Run:

```bash
python3 manage.py runserver
```

You should see something like:

```
Starting development server at http://0.0.0.0:8000/
```

You can then open the Codio preview and go to:

```
/admin/
```

Example:

```
https://your-codio-project-8000.codio.io/admin/
```

Log in using the superuser account you created earlier.


