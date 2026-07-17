### Summary: Why You Are Getting a 404 for `Comment` in Admin

Your Django server is working now ✅. The problem is that your new **`Comment` model is not registered in the Django admin**.

        Page not found (404)
        Request Method:	GET
        Request URL:	http://enigmaevident-garbolucky-8000.codio.io/admin/blog/comment/add
        Raised by:	django.contrib.admin.sites.catch_all_view
        Using the URLconf defined in blango.urls, Django tried these URL patterns, in this order:

        admin/ [name='index']
        admin/ login/ [name='login']
        admin/ logout/ [name='logout']
        admin/ password_change/ [name='password_change']
        admin/ password_change/done/ [name='password_change_done']
        admin/ autocomplete/ [name='autocomplete']
        admin/ jsi18n/ [name='jsi18n']
        admin/ r/<int:content_type_id>/<path:object_id>/ [name='view_on_site']
        admin/ auth/group/
        admin/ auth/user/
        admin/ blog/tag/
        admin/ blog/post/
        admin/ ^(?P<app_label>auth|blog)/$ [name='app_list']
        admin/ (?P<url>.*)$
        The current path, admin/blog/comment/add, matched the last one.

        You’re seeing this error because you have DEBUG = True in your Django settings file. Change that to False, and Django will display a standard 404 page.

The error shows Django knows about:

```
admin/blog/tag/
admin/blog/post/
```

but it does **not** show:

```
admin/blog/comment/
```

This means the admin does not have a page for creating Comments.

### Fix: Register `Comment` in `blog/admin.py`

Open:

```bash
blog/admin.py
```

Add:

```python
from django.contrib import admin
from .models import Comment, Post, Tag

admin.site.register(Comment)
admin.site.register(Post)
admin.site.register(Tag)
```

Save the file.

### Restart the server

Stop Django:

```bash
CTRL+C
```

Start it again:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

Then refresh:

```
https://enigmaevident-garbolucky-8000.codio.io/admin/
```

You should now see:

```
Blog
    Comments
    Posts
    Tags
```

### Learning Point

Creating a model is only one step. Django admin does **not automatically display every model**. A model must be **registered** in `admin.py` before it appears in the admin interface.

For your generic relationship lesson:

* `Comment` stores:

  * `content_type` → which model is being commented on
  * `object_id` → the ID of that object
  * `content_object` → the actual related object

Once registered, you can test creating comments linked to different models like `Post` or `User`.


---
Update your `admin.py` to include the new `Comment` model:

```python id="48391"
from django.contrib import admin
from blog.models import Tag, Post, Comment


admin.site.register(Tag)
admin.site.register(Comment)


class PostAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("title",)}
    list_display = ("slug", "published_at")


admin.site.register(Post, PostAdmin)
```

### What changed:

* Added `Comment` to the import:

  ```python
  from blog.models import Tag, Post, Comment
  ```
* Registered `Comment` with the admin site:

  ```python
  admin.site.register(Comment)
  ```

Now Django Admin will include:

* **Tags**
* **Posts**
* **Comments**

and you should be able to access:

```
https://enigmaevident-garbolucky-8000.codio.io/admin/blog/comment/add/
```

The `Comment` form will allow you to enter the generic relationship fields (`content_type`, `object_id`, etc.).
