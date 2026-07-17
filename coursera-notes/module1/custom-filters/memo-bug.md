# Memo: Temporary Rollback of Custom Django Template Filter Changes

## Issue Summary

While working through the Django custom template filters tutorial, an error occurred when updating the `author_details` filter to support additional functionality such as:

* Passing `request.user` as a filter argument
* Displaying "me" for the currently logged-in author
* Creating safe clickable email links using `format_html`

The application began showing the following error:

```
NameError at /
name 'user_model' is not defined
```

The error pointed to:

```
blog/templatetags/blog_extras.py
```

Although the file contents were corrected to use Django's `User` model, the application continued loading the previous error state. To avoid blocking progress through the tutorial, the custom filter changes were temporarily reverted.

## Files Changed

### 1. `blog/templatetags/blog_extras.py`

The filter was simplified back to the original `author_details` functionality.

Current behaviour:

* Checks that the author is a valid Django `User`
* Displays the author's full name if available
* Falls back to the username if no full name exists

The advanced features were removed temporarily:

* Current user comparison
* `"me"` display
* Email links
* Additional HTML formatting

### 2. `templates/blog/index.html`

The template filter usage was reverted from:

```django
{{ post.author|author_details:request.user }}
```

back to:

```django
{{ post.author|author_details }}
```

This returns the project to the stable state before introducing filter arguments.

## Current Status

The blog application can continue through the tutorial using the simpler custom filter implementation.

The advanced filter features can be reintroduced later after confirming:

* Django is loading the correct template tag file
* The development server has restarted correctly
* No cached Python modules remain
* The filter argument syntax is working correctly

## Next Steps

Continue with the tutorial using the working filter version. Once the remaining sections are complete, revisit the advanced `author_details` implementation and add features incrementally while testing after each change.
