# Environment Variables Exercise — Progress Notes

## 1. Starting location

You started in the Codio terminal at:

```bash
~/workspace
```

Your Django project was inside the `blango` folder, so you entered it:

```bash
cd blango/
```

Now you were here:

```bash
~/workspace/blango
```

This folder contains the Django project files:

```
blango/
blog/
db.sqlite3
manage.py
templates/
README.md
```

---

# 2. Generated the repository tree

You ran:

```bash
./generate_tree.sh
```

This created:

```
REPO_TREE.md
```

It is only a file listing your project structure. It is unrelated to the environment variable exercise.

---

# 3. Tried to run the environment variable script

You tried:

```bash
python3 environ_test.py
```

and received:

```
python3: can't open file 'environ_test.py': [Errno 2] No such file or directory
```

Meaning:

* Python was working.
* The command was correct.
* The file did not exist yet.

The course instructions said to create and paste code into `environ_test.py`, so you needed to create that file.

---

# 4. Created the practice file

You created the missing file:

```bash
touch environ_test.py
```

Then opened it:

```bash
nano environ_test.py
```

You pasted the course-provided Python code into it and saved it.

The file now contained code that tests four environment variables:

| Variable            | Purpose                                               |
| ------------------- | ----------------------------------------------------- |
| `MUST_BE_SET`       | Must exist before running or Python raises `KeyError` |
| `PYTHON_DEFAULT`    | Uses a default value if none exists                   |
| `ALWAYS_OVERRIDDEN` | Shows Python replacing an environment value           |
| `OPTIONAL`          | Uses `.get()` and returns `None` if missing           |

---

# 5. Tested without environment variables

You ran:

```bash
python3 environ_test.py
```

Result:

```
KeyError: 'MUST_BE_SET'
```

This was expected.

Reason:

The script contains:

```python
environ['MUST_BE_SET']
```

Python requires this variable to exist.

---

# 6. Set a variable temporarily

You tested:

```bash
MUST_BE_SET="is now set" python3 environ_test.py
```

Important concept:

This sets the variable **only for that one command**.

Output:

```
Value of 'MUST_BE_SET': 'is now set'
```

After the program finishes, the variable disappears.

---

# 7. Tested multiple temporary variables

You ran:

```bash
MUST_BE_SET="is now set" OPTIONAL="an optional value" python3 environ_test.py
```

This demonstrated that multiple variables can be added before a command.

Output:

```
Value of 'OPTIONAL': 'an optional value'
```

---

# 8. Used export to create persistent session variables

You ran:

```bash
export MUST_BE_SET="set in export"
```

Unlike the previous method, this remains available for future commands in the same terminal session.

Then:

```bash
python3 environ_test.py
```

Output:

```
Value of 'MUST_BE_SET': 'set in export'
```

---

# 9. Tested overriding defaults

You ran:

```bash
export PYTHON_DEFAULT="also exported"
python3 environ_test.py
```

Output:

```
Value of 'PYTHON_DEFAULT': 'also exported'
```

This proved that environment variables override the Python default:

```python
environ.setdefault("PYTHON_DEFAULT", "Python Default")
```

---

# 10. Removed an environment variable

You ran:

```bash
unset PYTHON_DEFAULT
```

Then:

```bash
python3 environ_test.py
```

Output:

```
Value of 'PYTHON_DEFAULT': 'Python Default'
```

The variable was removed, so Python used the default value again.

---

# 11. Completed the final exercise

The instructions asked:

Set:

```
ALWAYS_OVERRIDDEN="Environment variables are useful"
```

and:

```
OPTIONAL="The OPTIONAL variable now has a value"
```

You did:

```bash
export ALWAYS_OVERRIDDEN="Environment variables are useful"

export OPTIONAL="The OPTIONAL variable now has a value"
```

Then:

```bash
python3 environ_test.py
```

Final output:

```
Value of 'MUST_BE_SET': 'set in export'
Value of 'PYTHON_DEFAULT': 'Python Default'
Value of 'ALWAYS_OVERRIDDEN' before override: 'Environment variables are useful'
Value of 'ALWAYS_OVERRIDDEN' after override: 'Always Overridden In Python'
Value of 'OPTIONAL': 'The OPTIONAL variable now has a value'
```

---

# Current status

✅ `environ_test.py` created
✅ Environment variables tested
✅ Temporary variables tested
✅ Exported variables tested
✅ Default values tested
✅ Unset variables tested
✅ Final exercise completed

The next step in the lesson is the **Django configuration section**, where you will apply the same environment variable ideas to `settings.py` (for things like `SECRET_KEY`, `DEBUG`, and database configuration).
