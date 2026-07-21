# Database optimization

## 1. The problem: doing things one by one

Imagine you have 1,000 blog posts and want to add a comment to each post.

A beginner might write:

```python
for post in Post.objects.all():
    Comment.objects.create(
        creator=post.author,
        content="Thank you for reading my post!",
        content_object=post
    )
```

What happens?

Django sends **1,000 separate database requests**:

```
Database:
Create comment
Create comment
Create comment
Create comment
...
(1000 times)
```

This works, but it is slow.

---

# Bulk Create

## The optimized way

Instead, Django lets you prepare everything first:

```python
comments = []

for post in Post.objects.all():
    comments.append(
        Comment(
            creator=post.author,
            content="Thank you for reading my post!",
            content_object=post
        )
    )
```

Now you have a Python list:

```
comments = [
    Comment 1,
    Comment 2,
    Comment 3,
    Comment 4
]
```

Then send them all together:

```python
Comment.objects.bulk_create(comments)
```

Now Django sends one database operation:

```
Database:
Create Comment 1
Create Comment 2
Create Comment 3
Create Comment 4
```

Much faster.

---

# Bulk Update

Imagine you have 10,000 comments.

You want to change:

```
Thank you for reading my post!
```

into:

```
Thank you for reading my post! Signed, Bob
```

---

## Normal update

You might do:

```python
comments = Comment.objects.all()

for comment in comments:
    comment.content = "New text"
    comment.save()
```

The problem:

```
UPDATE comment 1
UPDATE comment 2
UPDATE comment 3
UPDATE comment 4
...
```

Thousands of database trips.

---

## Bulk update

First get the objects:

```python
comments = Comment.objects.all()
```

Change them in Python:

```python
for comment in comments:
    comment.content = "New text"
```

Then save them together:

```python
Comment.objects.bulk_update(
    comments,
    ["content"]
)
```

Now Django does it in a batch.

---

# The difference between update() and bulk_update()

This is the confusing part.

They sound similar, but they are different.

---

## update()

Example:

```python
Post.objects.all().update(published_at=None)
```

Meaning:

> "Change every post's published date to empty."

The database does everything.

Django sends:

```sql
UPDATE posts
SET published_at = NULL;
```

Advantages:

* Very fast
* One database query
* No Python loop

Disadvantage:

Every object gets the same value.

You cannot do:

```
Post 1 → today
Post 2 → yesterday
Post 3 → tomorrow
```

because every row receives the same update.

---

## bulk_update()

Example:

```python
posts = list(Post.objects.all())

posts[0].title = "New title 1"
posts[1].title = "New title 2"
posts[2].title = "New title 3"

Post.objects.bulk_update(
    posts,
    ["title"]
)
```

Here every object can have a different value.

You are saying:

```
Update this object with this value
Update that object with another value
```

---

# Bulk Delete

Deleting multiple records is also optimized.

Instead of:

```python
for comment in comments:
    comment.delete()
```

which does:

```
DELETE comment 1
DELETE comment 2
DELETE comment 3
```

You do:

```python
Comment.objects.filter(
    content__contains="Thank you"
).delete()
```

Django sends one database command:

```
DELETE all matching comments
```

---

# Why database optimization matters

Imagine your blog grows:

### Small blog

```
10 posts
50 comments
5 users
```

You won't notice problems.

---

### Large blog

```
100,000 posts
1 million comments
50,000 users
```

Poor code might do:

```
10,000 database requests
```

Optimized code might do:

```
5 database requests
```

The user sees pages load much faster.

---

# Other Django optimization ideas from this module

## 1. Filter in the database

Bad:

```python
posts = Post.objects.all()

for post in posts:
    if post.author == "Bob":
        print(post)
```

You downloaded everything.

Better:

```python
Post.objects.filter(author="Bob")
```

The database does the filtering.

---

## 2. Retrieve only what you need

Bad:

```python
Post.objects.all()
```

Gets every column:

```
id
title
content
image
author
date
...
```

If you only need titles:

```python
Post.objects.values("title")
```

Less data transferred.

---

## 3. Use indexes

Imagine a library.

Without an index:

You search every book:

```
Book 1
Book 2
Book 3
...
Book 1,000,000
```

With an index:

```
Author A → shelf 25
Author B → shelf 48
```

Databases work the same way.

Indexes make searches faster.

---

# Simple summary

| Operation       | Purpose                             | Speed     |
| --------------- | ----------------------------------- | --------- |
| `create()`      | Add one object                      | Normal    |
| `bulk_create()` | Add many objects together           | Fast      |
| `save()`        | Update one object                   | Normal    |
| `bulk_update()` | Update many different objects       | Fast      |
| `update()`      | Update many objects with same value | Very fast |
| `delete()`      | Remove many objects                 | Fast      |

The main lesson:

> **Let the database do database work. Avoid making Python perform thousands of small operations when Django can send one efficient command.**

This is one of the biggest differences between a beginner Django project and a production-level Django application.
