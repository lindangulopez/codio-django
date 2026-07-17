# First generic comment

Your code is working correctly. The reason you are getting:

```python
Out[10]: <QuerySet []>
```

is because **there are currently no comments linked to that Post**.

The reverse relationship (`p.comments.all()`) is not creating comments; it only **finds comments that already exist**.

### What happened

You successfully tested:

```python
p.comments.all()
```

Django looked for comments where:

* `content_type` = Post
* `object_id` = this Post's ID

It found none, so it returned:

```python
<QuerySet []>
```

Then this failed:

```python
c1 = p.comments.all()[0]
```

because you tried to get the first item from an empty list.

---

## Next: Create a Comment linked to your Post

While still inside the Django shell, run:

```python
from django.contrib.auth.models import User
```

Get a user:

```python
u = User.objects.first()
```

Get your post:

```python
p = Post.objects.first()
```

Create a comment:

```python
c1 = Comment(
    creator=u,
    content="This is my first generic comment!",
    content_object=p
)
```

Save it:

```python
c1.save()
```

Now test the reverse relationship:

```python
p.comments.all()
```

You should now see something like:

```text
<QuerySet [<Comment: This is my first generic comment!>]>
```

---

### What you have learned

Before creating a comment:

```python
p.comments.all()
```

returns:

```text
<QuerySet []>
```

because no relationship exists.

After creating:

```python
Comment(content_object=p)
```

Django automatically stores:

* `content_type` → Post model
* `object_id` → Post's ID

Now both directions work:

From Comment → Post:

```python
c1.content_object
```

From Post → Comments:

```python
p.comments.all()
```

This confirms your **GenericForeignKey + GenericRelation** setup is working.
