# **Launching the Blog** 

Now you need to run the server and check the admin page.

### 1. Start the Django server

Your terminal is already in:

```bash
cd blango
```

Run:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

Wait until you see something like:

```text
Starting development server at http://0.0.0.0:8000/
```

Keep this terminal running.

---

### 2. Open the admin page

Your preview panel is already open:

```
https://{{domain8000}}/
```

Change the address to:

```
https://{{domain8000}}/admin
```

or:

```
https://{{domain8000}}/admin/
```

Press Enter.

---

### 3. Log in

Use the superuser username and password you created earlier with:

```bash
python3 manage.py createsuperuser
```

After logging in, you should see:

* Groups
* Users
* Posts
* Tags

---

### 4. Create a test Post (optional but recommended)

1. Click **Posts**
2. Click **Add Post**
3. Enter:

   * Author → your user
   * Title → something like `My First Post`
   * Summary → a short summary
   * Content → some text
   * Add a Tag
4. Notice that the **slug** fills automatically from the title.
5. Click **Save**



