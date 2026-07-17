You are now editing:

```text
blango/blog/models.py
```

Right now you only have:

```python
from django.db import models

# Create your models here.
```

You need to replace that with the models from the instructions.

### 1. Replace everything in `models.py` with this:

```python
from django.db import models
from django.conf import settings


class Tag(models.Model):
    value = models.TextField(max_length=100)

    def __str__(self):
        return self.value


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.PROTECT)
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

---

### 2. Save the file

Press:

```
Ctrl + S
```

---

### 3. Create the database migration

Go back to the terminal. Make sure you are in:

```bash
~/workspace/blango
```

Run:

```bash
python3 manage.py makemigrations
```

You should see something like:

```
Migrations for 'blog':
  blog/migrations/0001_initial.py
    - Create model Tag
    - Create model Post
```

---

### 4. Apply the migration

Run:

```bash
python3 manage.py migrate
```

You should see:

```
Applying blog.0001_initial... OK
```


