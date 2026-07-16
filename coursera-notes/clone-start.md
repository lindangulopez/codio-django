# Using Codio

Open `settings.py` either from the file browser or from the terminal.

### Method 1: Use the Codio file browser

1. In the left sidebar, open your project folder:

   ```
   blango
   ```
2. Open:

   ```
   blango/settings.py
   ```

   (It may be inside another `blango` folder.)
3. Find:

   ```python
   INSTALLED_APPS = [
   ```
4. Add:

   ```python
   "blog",
   ```

   before the closing `]`.

It should look like:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "blog",
]
```

5. Save the file:

   * Press **Ctrl + S** (Windows/Linux)
   * Press **Cmd + S** (Mac)

---

### Method 2: Open it from the terminal

You are currently in:

```bash
/home/codio/workspace/blango
```

Run:

```bash
ls
```

You should see something like:

```
blog  manage.py  blango
```

Enter the project folder:

```bash
cd blango
```

List files:

```bash
ls
```

You should see:

```
settings.py
urls.py
...
```

Open it with Codio's editor:

```bash
code settings.py
```

Then edit `INSTALLED_APPS`.

---

After saving, return to the terminal and you can test that Django recognizes the app:

```bash
python3 manage.py check
```

If you see:

```
System check identified no issues
```

your setup is correct.
