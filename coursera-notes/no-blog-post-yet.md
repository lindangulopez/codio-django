# WIP

If you go to:

```text
https://nativeobscure-baboonpostage-8000.codio.io/
```

and you do **not** see your blog post, that is expected at this stage.

You have only built:

* ✅ Models (`Post`, `Tag`)
* ✅ Database migrations
* ✅ Admin interface
* ✅ Created a test post

You have **not created a public blog page yet** (views + templates + URLs). The `/` homepage is still empty because Django does not know what to display there.

Your working page is:

```text
https://nativeobscure-baboonpostage-8000.codio.io/admin/
```

where you can see:

```
Blog
 ├── Posts
 └── Tags
```

---

### To verify your test post

Go to:

```text
https://nativeobscure-baboonpostage-8000.codio.io/admin/
```

Then:

1. Click **Posts**
2. Click your post (`test1`)
3. Confirm the fields are there:

   * Author: codio
   * Title: test1
   * Slug: test1
   * Summary
   * Content
   * Tags

---

### If you want to see the blog on `/`

The next lessons will have you create:

1. A view (`views.py`)
2. URL routes (`urls.py`)
3. HTML templates
4. A page that displays your `Post` objects

Then:

```text
https://nativeobscure-baboonpostage-8000.codio.io/
```

will show actual blog content.

For now, your Django Admin setup is successful. Codio's preview URL works because the Django server is running on port 8000, which is the port you exposed with:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

([docs.codio.com][1])

[1]: https://docs.codio.com/common/develop/ide/editing/preview.html "Preview | Codio Documentation"
