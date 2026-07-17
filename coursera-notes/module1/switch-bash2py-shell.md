# switch from bash to py

You entered **Python code directly into the Bash terminal**. The terminal is interpreting `from` as a Linux command, which is why you see:

```text
from: can't read /var/mail/django.contrib.contenttypes.models
```

You need to open the **Django Python shell first**.

### Step 1: Start the Django shell

In your terminal, run:

```bash
python3 manage.py shell
```

You should see something like:

```text
Python 3.x.x ...
(InteractiveConsole)
>>>
```

The `>>>` means you are now inside Python.

---

### Step 2: Run your Django commands

Now enter:

```python
from django.contrib.contenttypes.models import ContentType
from blog.models import Post, Comment
```

Then test your reverse relationship:

```python
p = Post.objects.first()
p.comments.all()
```

You should see your comments connected to that post.

---

### Step 3: Exit the Django shell when finished

```python
exit()
```

### Learning point

* **Bash terminal** → runs commands like:

  ```bash
  python3 manage.py shell
  python3 manage.py runserver
  ```

* **Django/Python shell** → runs Python code like:

  ```python
  from blog.models import Post
  Post.objects.all()
  ```

You were using the right code, just in the wrong environment.
