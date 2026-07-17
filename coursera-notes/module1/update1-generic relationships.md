You are very close. Based on the lesson, you need to make sure your project matches the required Blango updates.

## 1. Update `models.py`

You already have most of it done, but your `Comment` model is missing the required timestamps:

Add:

```python
created_at = models.DateTimeField(auto_now_add=True)
modified_at = models.DateTimeField(auto_now=True)
```

Your `Comment` model should include:

```python
class Comment(models.Model):
    creator = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE
    )
    content = models.TextField()
    content_type = models.ForeignKey(
        ContentType,
        on_delete=models.CASCADE
    )
    object_id = models.PositiveIntegerField()
    content_object = GenericForeignKey("content_type", "object_id")
    created_at = models.DateTimeField(auto_now_add=True)
    modified_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content[:50]
```

Make sure your `Post` model still has:

```python
comments = GenericRelation(Comment)
```

---

## 2. Your `admin.py` is already correct ✅

You currently have:

```python
from django.contrib import admin
from blog.models import Tag, Post, Comment


admin.site.register(Tag)
admin.site.register(Comment)


class PostAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("title",)}
    list_display = ("slug", "published_at")


admin.site.register(Post, PostAdmin)
```

This satisfies the requirement.

---

## 3. Create the migration

In the terminal:

```bash
python3 manage.py makemigrations
```

Because you added `created_at` and `modified_at`, Django may show:

```
You are trying to add the field 'created_at' with 'auto_now_add=True'...
```

Choose:

```
1
```

Then press **Enter** when it shows:

```
[default: timezone.now] >>>
```

This gives existing comments a creation date.

---

## 4. Apply migrations

Run:

```bash
python3 manage.py migrate
```

You should see:

```
Applying blog.0003_comment_created_at_comment_modified_at... OK
```

---

## 5. Test in Django shell

Start:

```bash
python3 manage.py shell
```

Then:

```python
from blog.models import Post, Comment
from django.contrib.auth.models import User

p = Post.objects.first()
u = User.objects.first()

c = Comment(
    creator=u,
    content="Great post!",
    content_object=p
)

c.save()
```

Check the reverse relationship:

```python
p.comments.all()
```

You should now see your comment.

---

### Reading Question Answers

* Using **GenericRelation** indicates a **reverse generic relationship**.
* Using **GenericForeignKey** indicates a **generic relationship**.

Your next milestone is completing the migration and testing that comments appear in the admin and through `Post.comments.all()`.
