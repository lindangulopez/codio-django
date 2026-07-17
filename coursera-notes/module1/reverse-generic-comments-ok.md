# Completed the model and admin updates

The next steps are to **create the database migration, apply it, and test the comment system**.

### 1. Create migrations

In your terminal (make sure you are in `~/workspace/blango`):

```bash
python3 manage.py makemigrations
```

Because you added:

```python
created_at = models.DateTimeField(auto_now_add=True)
modified_at = models.DateTimeField(auto_now=True)
```

Django may ask:

```
You are trying to add the field 'created_at' with 'auto_now_add=True'...
Select an option:
```

Choose:

```
1
```

Then when it asks:

```
[default: timezone.now] >>>
```

Press:

```
Enter
```

---

### 2. Apply the migration

Run:

```bash
python3 manage.py migrate
```

You should see something like:

```
Applying blog.0003_... OK
```

---

### 3. Restart your Django server

If your server is not running:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

---

### 4. Test the Admin

Open:

```
https://enigmaevident-garbolucky-8000.codio.io/admin/
```

You should now see:

```
Blog
 ├── Comments
 ├── Posts
 └── Tags
```

Try adding a comment:

* Go to **Comments → Add Comment**
* Select a creator
* Add some content
* Choose:

  * Content type: `blog | post`
  * Object ID: the ID of an existing post

Save it.

---

### 5. Test the reverse relationship in Django shell

Open:

```bash
python3 manage.py shell
```

Then:

```python
from blog.models import Post, Comment

p = Post.objects.first()

p.comments.all()
```

You should see the comments connected to that post.

You can also test the other direction:

```python
c = Comment.objects.first()

c.content_object
```

This should return the Post the comment belongs to.

---

### Learning checkpoint

At this stage your Blango project has:

✅ `Comment` model
✅ GenericForeignKey (`Comment → Post/User/other models`)
✅ GenericRelation (`Post → Comments`)
✅ Comments available in Admin
✅ Ability to query comments from either side

After this, the next likely step in the lesson will be adding comments to templates/views so users can see and create them on blog posts.
