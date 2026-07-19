# Django Configurations Values Part 2 — Beginner Instructions

You have already completed:

✅ Installed `django-configurations`
✅ Changed `settings.py` into a `Dev(Configuration)` class
✅ Added `values.Value()` for `TIME_ZONE`

Now we are going to make Django settings smarter by allowing different types of values to come from environment variables.

---

# What are we changing?

Right now your settings contain values like:

```python
DEBUG = True
```

and:

```python
ALLOWED_HOSTS = ['*']
```

The problem:

Environment variables are always **text (strings)**.

Example:

```bash
DEBUG="False"
```

Python sees this as:

```python
"False"
```

not:

```python
False
```

A string with text inside is considered **True** in Python.

So Django Configurations gives us special tools:

| Setting type  | Tool                    |
| ------------- | ----------------------- |
| Text          | `values.Value()`        |
| True/False    | `values.BooleanValue()` |
| Secret values | `values.SecretValue()`  |
| Lists         | `values.ListValue()`    |

---

# Step 1 — Open settings.py

Open:

```
blango/blango/settings.py
```

At the top you should already have:

```python
from configurations import Configuration, values
```

If you do not, add it.

---

# Step 2 — Change DEBUG

Find this:

```python
DEBUG = True
```

Replace it with:

```python
DEBUG = values.BooleanValue(True)
```

What this means:

* Default value is `True`
* But an environment variable can change it

Example:

```bash
DJANGO_DEBUG=False python3 manage.py runserver
```

Now Django will use:

```python
DEBUG = False
```

---

# Step 3 — Change ALLOWED_HOSTS

Find:

```python
ALLOWED_HOSTS = ['*']
```

Replace it with:

```python
ALLOWED_HOSTS = values.ListValue(
    ['localhost', '0.0.0.0', '.codio.io']
)
```

Why?

Because `ALLOWED_HOSTS` is a list.

Environment variables cannot directly store lists.

Instead we can write:

```bash
ALLOWED_HOSTS=localhost,0.0.0.0,.codio.io
```

and Django converts it into:

```python
[
    'localhost',
    '0.0.0.0',
    '.codio.io'
]
```

---

# Step 4 — Add a production configuration

At the bottom of your file, after:

```python
class Dev(Configuration):
```

add:

```python
class Prod(Dev):

    DEBUG = False

    SECRET_KEY = values.SecretValue()
```

---

# What does this do?

Your file now has two configurations:

## Development

```python
class Dev(Configuration):
```

Used when coding.

It can have:

```python
DEBUG = True
```

---

## Production

```python
class Prod(Dev):
```

Used when the website is live.

It changes:

```python
DEBUG = False
```

because showing debugging information on a live website is unsafe.

---

# Why use SecretValue?

Your secret key currently looks like:

```python
SECRET_KEY = 'django-insecure-xxxxx'
```

This is dangerous because it could accidentally be uploaded to GitHub.

For production we use:

```python
SECRET_KEY = values.SecretValue()
```

This forces Django to get the secret from an environment variable instead.

Example:

```bash
DJANGO_SECRET_KEY="my-secret-key" 
```

---

# Step 5 — Save the file

Save:

```
settings.py
```

---

# Step 6 — Test your website

Run:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

You should see:

```
django-configurations version ..., using configuration Dev
```

Your blog should still work.

---

# Reading Question

Question:

```python
MY_BOOL = values.Value("False")
```

What is wrong?

Correct answer:

✅ **The value will be treated as True.**

Why?

Because:

```python
"False"
```

is a string.

Python sees:

```python
bool("False")
```

as:

```python
True
```

because the string is not empty.

The correct code would be:

```python
MY_BOOL = values.BooleanValue(False)
```

because `BooleanValue` understands true/false text.

---

## Final checklist

Before moving on, you should have:

✅ `from configurations import Configuration, values`
✅ `DEBUG = values.BooleanValue(True)`
✅ `TIME_ZONE = values.Value("UTC")`
✅ `ALLOWED_HOSTS = values.ListValue([...])`
✅ Added:

```python
class Prod(Dev):
    DEBUG = False
    SECRET_KEY = values.SecretValue()
```

✅ Server starts successfully

You are now using environment-based configuration following the **12-Factor App Config principle (Factor 3)**.
