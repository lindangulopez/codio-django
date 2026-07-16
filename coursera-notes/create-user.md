## Project Migration

Do the following in your Codio terminal.

### 1. Make sure you are in the `blango` folder

Your terminal should look something like:

```bash
codio@nativeobscure-baboonpostage:~/workspace/blango$
```

If not, run:

```bash
cd ~/workspace/blango
```

---

### 2. Run the migrations

Type:

```bash
python3 manage.py migrate
```

Wait for it to finish. You should see lots of lines ending with:

```
OK
```

---

### 3. Create your admin user

Next run:

```bash
python3 manage.py createsuperuser
```

It will ask:

```
Username (leave blank to use 'codio'):
```

You can press **Enter** to use `codio`, or type your own username.

Then:

```
Email address:
```

You can enter something like:

```
codio@example.com
```

Then:

```
Password:
```

Type a password. (You won't see characters appear while typing — that is normal.)

Example for a class project:

```
password123
```

If Django says:

```
This password is too common.
Bypass password validation and create user anyway? [y/N]
```

Type:

```bash
y
```

You should see:

```
Superuser created successfully.
```

