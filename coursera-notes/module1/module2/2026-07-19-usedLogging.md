# Django logging setup is working correctly

Great work — your results show that your **Django logging setup is working correctly**. Let's break down what happened, assuming you are still building your understanding of Django.

Your terminal output:

```text
Time zone: UTC
System check identified no issues (0 silenced).
July 19, 2026 - 14:15:42
Django version 3.2.6, using settings 'blango.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
```

## 1. Django server started successfully ✅

This command:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

means:

* `python3` → run Python
* `manage.py` → Django's control program
* `runserver` → start the development web server
* `0.0.0.0:8000` → listen on port 8000 so Codio can display your website

Django loaded your project:

```text
using settings 'blango.settings'
```

This means Django found:

```
blango/settings.py
```

and used your configuration.

---

# 2. "System check identified no issues" ✅

Django automatically checks your project before starting.

It checks things like:

* Are your models valid?
* Are your URLs correct?
* Are installed apps available?
* Are settings readable?
* Are migrations okay?

You got:

```text
System check identified no issues (0 silenced).
```

Meaning:

> Django looked around your project and did not find any problems.

---

# 3. Your logging worked 🎉

This is the important part:

```text
Got 1 posts
```

This comes from the line you added in:

```
blog/views.py
```

Inside:

```python
def index(request):
    posts = Post.objects.filter(published_at__lte=timezone.now())

    logger.debug("Got %d posts", len(posts))
```

Let's follow the journey.

---

## The request flow

When you open:

```
http://your-codio-address/
```

Django receives:

```
GET /
```

Your URL configuration sends `/` to your `index()` view.

Your view runs:

```python
posts = Post.objects.filter(
    published_at__lte=timezone.now()
)
```

Django asks the database:

> "Give me all posts that are published."

Your database answered:

```
1 post found
```

So:

```python
len(posts)
```

becomes:

```python
1
```

Then this line runs:

```python
logger.debug("Got %d posts", len(posts))
```

Django replaces:

```
%d
```

with:

```
1
```

The final message becomes:

```
Got 1 posts
```

---

# 4. The HTTP request line

You also got:

```text
[19/Jul/2026 14:16:06] "GET / HTTP/1.1" 200 5467
```

This is Django's normal server logging.

Let's decode it:

| Part                   | Meaning                         |
| ---------------------- | ------------------------------- |
| `19/Jul/2026 14:16:06` | Time of request                 |
| `GET /`                | Browser requested the home page |
| `HTTP/1.1`             | Web communication protocol      |
| `200`                  | Success                         |
| `5467`                 | Size of response in bytes       |

The important number is:

```
200
```

It means:

> The page loaded successfully.

---

# What you learned in this lesson

You have now connected several Django concepts together.

## Before logging

Your view:

```
Browser
  |
  v
URL
  |
  v
views.py
  |
  v
Database
  |
  v
HTML page
```

You could see the result, but not what happened internally.

---

## After logging

Now:

```
Browser
  |
  v
URL
  |
  v
views.py
  |
  +--> logger.debug()
  |
  v
Database
  |
  v
HTML page
```

Your application tells you what it is doing.

---

# Why use logging instead of print()?

A beginner might think:

```python
print("Got posts")
```

Why not just use that?

Because logging gives you control.

Example:

```python
logger.debug("Got %d posts", len(posts))
```

can be:

* shown during development ✅
* hidden in production ✅
* sent to files ✅
* sent to monitoring systems ✅

A `print()` statement cannot do that easily.

---

# Why this line is important

You wrote:

```python
logger = logging.getLogger(__name__)
```

`__name__` automatically becomes the file name.

In your case:

```
blog/views.py
```

becomes:

```
blog.views
```

So Django knows:

> "This message came from the blog views module."

Later, you can configure:

```
blog.views → DEBUG
blog.models → WARNING
django → INFO
```

individually.

---

# Next thing to test

Your lesson also added:

```python
logger.info(
    "Created comment on Post %d for user %s",
    post.pk,
    request.user,
)
```

Now:

1. Open your blog.
2. Log in.
3. Add a comment.
4. Watch your terminal.

You should see something like:

```text
Created comment on Post 1 for user your_username
```

That will prove your second logging event works.

---

## Your current status

✅ Django starts
✅ Settings load
✅ Database works
✅ Blog page loads
✅ DEBUG logging works
✅ STDOUT logging works

You have successfully completed the first half of Django logging.
