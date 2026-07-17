# Add Comment modules

`models.py` should be updated by:

* Importing `GenericForeignKey` and `ContentType`.
* Declaring the `Comment` model **before** `Post`.
* Adding the three fields required for a generic relationship:

  * `content_type`
  * `object_id`
  * `content_object`

Here's the rewritten file:

```python
from django.db import models
from django.conf import settings
from django.contrib.contenttypes.fields import GenericForeignKey
from django.contrib.contenttypes.models import ContentType


class Tag(models.Model):
    value = models.TextField(max_length=100)

    def __str__(self):
        return self.value


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

    def __str__(self):
        return self.content[:50]


class Post(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.PROTECT
    )
    created_at = models.DateTimeField(auto_now_add=True)
    modified_at = models.DateTimeField(auto_now=True)
    published_at = models.DateTimeField(blank=True, null=True)
    title = models.TextField(max_length=100)
    slug = models.SlugField()
    summary = models.TextField(max_length=500)
    content = models.TextField()
    tags = models.ManyToManyField(Tag, related_name="posts")

    def __str__(self):
        return self.title
```

After saving the file, don't forget to create and apply the migration:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

