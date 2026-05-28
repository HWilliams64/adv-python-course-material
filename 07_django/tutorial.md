# Building a Notes App with Django

> **Prerequisites:** Python basics — variables, functions, classes, loops, and conditionals. No prior web development experience required.

Django is Python's most popular web framework, used by companies like Instagram, Pinterest, and Mozilla. In this tutorial you will build a **note-taking web app** from scratch — one that can create, read, update, and delete notes stored in a real database, all through a browser.

By the end you will understand:

- How Django structures a project (apps, models, views, templates, and URLs)
- How to define a database table using a Python class (called a *model*)
- How Django's request/response cycle works
- How to render HTML pages with real data from a database

<video src="media/django-concept-overview.mp4" controls width="100%"></video>

---

## How to Use This Tutorial

This is a **Runme** tutorial. Every code block has a ▶ button — click it to run the block. Follow the steps **in order from top to bottom**. Each block is written to be safe to re-run if something goes wrong.

> All commands run from the **same folder as this tutorial file**.

---

## The Big Picture — Django's MVT Pattern

Before writing any code, it helps to understand how Django thinks about web requests.

When a browser asks for a page, Django follows a three-part pattern called **MVT**:

<pre><code>Browser Request → URLs → View → Model → View → Template → Browser Response</code></pre>

| Part | Your File | Job |
|------|-----------|-----|
| **Model** | `models.py` | Defines the shape of your data and talks to the database |
| **View** | `views.py` | Contains the logic — fetches data, decides what template to show |
| **Template** | `*.html` | The HTML the browser receives, with data filled in |
| **URL Config** | `urls.py` | Maps a web address like `/notes/` to the right view function |

You will create each of these pieces step by step.

---

## Step 1 — Install Dependencies

Python projects list their required packages in a file called `requirements.txt`. This is a standard convention — anyone can read the file to see what your project needs, and one command installs everything.

This project uses four packages:

| Package | Why |
|---------|-----|
| `django` | The web framework itself |
| `django-crispy-forms` | Renders Django forms with Bootstrap styling automatically |
| `crispy-bootstrap5` | The Bootstrap 5 theme plugin for crispy-forms |
| `django-bootstrap-v5` | Provides Bootstrap 5 CSS/JS template tags |

```bash
pwd
```

**Create the requirements file:**

```bash {"interactive":"false","name":"create-requirements"}
cat > requirements.txt << 'EOF'
django>=4.1.7,<5.0.0
django-crispy-forms>=2.0,<3.0
crispy-bootstrap5>=0.7,<1.0
django-bootstrap-v5>=1.0.11,<2.0.0
EOF
echo "✓ requirements.txt created"
```

**Install all packages:**

```bash {"interactive":"false","name":"install-deps"}
VENV_PATH="../../.venv"

if [ -n "$VIRTUAL_ENV" ]; then
    echo "✓ Virtual environment already active: $VIRTUAL_ENV"
elif [ -d "$VENV_PATH" ]; then
    source "$VENV_PATH/bin/activate"
    echo "✓ Virtual environment activated: $VENV_PATH"
else
    python -m venv "$VENV_PATH"
    echo "✓ Virtual environment created: $VENV_PATH"
    source "$VENV_PATH/bin/activate"
    echo "✓ Virtual environment activated: $VENV_PATH"
fi

pip install -r requirements.txt
```

---

## Step 2 — Create the Django Project

`django-admin startproject` generates a new Django project. Think of a *project* as the outer container — it holds your site-wide settings, URL configuration, and one or more apps.

The command creates a `note_app/` directory next to this tutorial file with this structure:

<pre><code>note_app/              ← the project root (manage.py lives here)
├── manage.py          ← a command-line tool for running Django commands
└── note_app/          ← the project Python package
    ├── __init__.py
    ├── settings.py    ← all project-wide settings
    ├── urls.py        ← the main URL router
</code></pre>

```bash {"interactive":"false","name":"create-project"}
if [ ! -d note_app ]; then
    django-admin startproject note_app
    echo "✓ Django project created"
else
    echo "✓ Project already exists — skipping"
fi
```

---

## Step 3 — Create the Notes App

Inside a Django project you can have multiple *apps*. An app is a self-contained module responsible for one piece of functionality. Keeping apps focused makes large projects easier to maintain.

`python manage.py startapp notes` generates a new app directory:

<pre><code>note_app/
└── notes/             ← the new app
    ├── __init__.py
    ├── admin.py       ← register models with the admin site
    ├── apps.py        ← app configuration
    ├── migrations/    ← database migration files (auto-generated)
    ├── models.py      ← data models (your database tables)
    ├── tests.py       ← unit tests
</code></pre>

```bash {"interactive":"false","name":"create-app"}
if [ ! -d note_app/notes ]; then
    cd note_app && python manage.py startapp notes
    echo "✓ Notes app created"
else
    echo "✓ App already exists — skipping"
fi
```

---

## Step 4 — Configure Project Settings

`settings.py` is where you control everything about your Django project. You need to make five changes:

1. __`ALLOWED_HOSTS`__ — tells Django which domain names are allowed to serve requests. Required for security.
2. __`INSTALLED_APPS`__ — every app Django should know about must be listed here. This includes your `notes` app and the third-party styling packages.
3. **Crispy Forms settings** — tells crispy-forms to use Bootstrap 5 as its HTML template pack.
4. __`CSRF_TRUSTED_ORIGINS`__ — required when your app runs behind a proxy or at a non-localhost domain (like a GraderThan workspace).
5. **`XFrameOptionsMiddleware`** — disables a header that blocks the app from being shown in an iframe. Only removed in development (`DEBUG = True`) mode.

The script below makes each change only if it has not been made already, so it is safe to run more than once.

```python {"interactive":"false","name":"configure-settings"}
import re

path = "note_app/note_app/settings.py"
with open(path) as f:
    content = f.read()

changes = []

# 1. ALLOWED_HOSTS
if ".workspace.graderthan.com" not in content:
    content = content.replace(
        "ALLOWED_HOSTS = []",
        "ALLOWED_HOSTS = ['127.0.0.1', 'localhost', '.workspace.graderthan.com']"
    )
    changes.append("ALLOWED_HOSTS")

# 2. INSTALLED_APPS — append our apps before the closing ]
if "'notes'" not in content and '"notes"' not in content:
    content = content.replace(
        "    'django.contrib.staticfiles',\n]",
        "    'django.contrib.staticfiles',\n"
        "    'crispy_forms',\n"
        "    'crispy_bootstrap5',\n"
        "    'bootstrap5',\n"
        "    'notes',\n"
        "]"
    )
    changes.append("INSTALLED_APPS")

# 3. Crispy Forms template pack
if "CRISPY_TEMPLATE_PACK" not in content:
    content += '\n# Crispy Forms\nCRISPY_TEMPLATE_PACK = "bootstrap5"\nCRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"\n'
    changes.append("CRISPY_TEMPLATE_PACK")

# 4. CSRF trusted origins for workspace domains
if "CSRF_TRUSTED_ORIGINS" not in content:
    content += "\nCSRF_TRUSTED_ORIGINS = ['https://*.workspace.graderthan.com']\n"
    changes.append("CSRF_TRUSTED_ORIGINS")

# 5. Remove clickjacking middleware in dev so the app works inside iframes
if "MIDDLEWARE.remove" not in content:
    content += '\nif DEBUG:\n    MIDDLEWARE.remove("django.middleware.clickjacking.XFrameOptionsMiddleware")\n'
    changes.append("XFrameOptionsMiddleware fix")

with open(path, "w") as f:
    f.write(content)

if changes:
    for c in changes:
        print(f"  ✓ {c} updated")
else:
    print("  ✓ Settings already configured — no changes needed")
```

---

## Step 5 — Define the Data Model

A Django *model* is a Python class that represents one database table. Each attribute of the class maps to a column in the table. Django reads these classes and automatically generates the SQL to create the table — you never write SQL by hand.

The `Note` model has four fields:

| Field | Type | Purpose |
|-------|------|---------|
| `title` | `CharField` | Short text, maximum 200 characters |
| `content` | `TextField` | Long text, no character limit |
| `created_at` | `DateTimeField` | Set automatically when the note is first saved |
| `updated_at` | `DateTimeField` | Updated automatically every time the note is saved |

The `__str__` method controls what Django displays when it shows a Note object in the admin panel or shell.

```python {"interactive":"false","name":"write-models"}
content = '''\
from django.db import models


class Note(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
'''

with open("note_app/notes/models.py", "w") as f:
    f.write(content)

print("✓ notes/models.py written")
```

---

## Step 6 — Build the Note Form

A Django *form* handles validating user input and rendering HTML `<input>` elements. A `ModelForm` is a special kind of form that is tied directly to a model — Django can automatically generate form fields from the model's fields.

`crispy-forms` is added to the form's `__init__` method via a _helper_ object. The helper tells crispy-forms to apply Bootstrap 5 styling when the form is rendered in a template.

```python {"interactive":"false","name":"write-forms"}
content = '''\
from crispy_forms.helper import FormHelper
from django import forms

from .models import Note


class NoteForm(forms.ModelForm):
    class Meta:
        model = Note
        fields = ["title", "content"]

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.helper = FormHelper()
'''

with open("note_app/notes/forms.py", "w") as f:
    f.write(content)

print("✓ notes/forms.py written")
```

---

## Step 7 — Create the Database Tables

Django uses a two-step process to update the database:

1. **`makemigrations`** — Django reads your models and writes a *migration file*: a Python script that describes the changes needed to the database schema. Think of it as writing down the instructions.
2. **`migrate`** — Django executes those instructions, creating or altering the actual database tables. SQLite (the default) stores everything in a single file called `db.sqlite3`.

Both commands are safe to run multiple times — if there is nothing new to migrate, Django simply reports that and does nothing.

```bash {"interactive":"false","name":"make-migrations"}
cd note_app && python manage.py makemigrations
```

```bash {"interactive":"false","name":"migrate"}
cd note_app && python manage.py migrate
```

---

## Step 8 — Write the Views

A *view* is a Python function that receives a web request and returns a web response. This is where your application's logic lives.

This app has four views — one for each CRUD operation:

| View | URL | What it does |
|------|-----|-------------|
| `note_list` | `/notes/` | Fetches all notes from the database and displays them |
| `note_create` | `/notes/new/` | Shows an empty form; saves a new note on POST |
| `note_edit` | `/notes/<id>/edit/` | Shows a form pre-filled with an existing note; saves changes on POST |
| `note_delete` | `/notes/<id>/delete/` | Shows a confirmation page; deletes the note on POST |

A few Django helpers used here:

- **`render(request, template, context)`** — renders an HTML template with a dictionary of data and returns it as an HTTP response.
- __`redirect(url_name)`__ — sends the browser to a different URL (used after a successful form save).
- __`get_object_or_404(Model, pk=id)`__ — fetches a record from the database by its primary key, or returns a 404 error if it does not exist.

```python {"interactive":"false","name":"write-views"}
content = '''\
from django.shortcuts import get_object_or_404, redirect, render

from .forms import NoteForm
from .models import Note


def note_list(request):
    notes = Note.objects.all()
    return render(request, "notes/note_list.html", {"notes": notes})


def note_create(request):
    form = NoteForm(request.POST or None)
    if form.is_valid():
        form.save()
        return redirect("notes:note_list")
    return render(request, "notes/note_form.html", {"form": form})


def note_edit(request, pk):
    note = get_object_or_404(Note, pk=pk)
    form = NoteForm(request.POST or None, instance=note)
    if form.is_valid():
        form.save()
        return redirect("notes:note_list")
    return render(request, "notes/note_form.html", {"form": form})


def note_delete(request, pk):
    note = get_object_or_404(Note, pk=pk)
    if request.method == "POST":
        note.delete()
        return redirect("notes:note_list")
    return render(request, "notes/note_confirm_delete.html", {"note": note})
'''

with open("note_app/notes/views.py", "w") as f:
    f.write(content)

print("✓ notes/views.py written")
```

---

## Step 9 — Set Up URL Routing

Django's URL configuration maps a URL pattern to a view function. When a request comes in, Django checks the patterns in order and calls the first matching view.

There are two `urls.py` files to configure:

__`notes/urls.py`__ (app-level) — handles URLs within the `/notes/` prefix. The `app_name = "notes"` line creates a _namespace_, allowing templates to refer to URLs as `notes:note_list` instead of just `note_list` — useful when multiple apps have views with the same name.

__`note_app/urls.py`__ (project-level) — the main router. It includes the notes app's URLs under the `/notes/` path, and adds a redirect so visiting the root URL `/` automatically goes to the note list.

**Create the app-level URL config:**

```python {"interactive":"false","name":"write-app-urls"}
content = '''\
from django.urls import path

from . import views

app_name = "notes"

urlpatterns = [
    path("", views.note_list, name="note_list"),
    path("new/", views.note_create, name="note_create"),
    path("<int:pk>/edit/", views.note_edit, name="note_edit"),
    path("<int:pk>/delete/", views.note_delete, name="note_delete"),
]
'''

with open("note_app/notes/urls.py", "w") as f:
    f.write(content)

print("✓ notes/urls.py written")
```

**Patch the project-level URL config:**

```python {"interactive":"false","name":"patch-project-urls"}
path = "note_app/note_app/urls.py"

with open(path) as f:
    content = f.read()

if "notes.urls" not in content:
    content = (
        "from django.contrib import admin\n"
        "from django.urls import include, path\n"
        "from django.views.generic.base import RedirectView\n"
        "\n"
        "urlpatterns = [\n"
        "    path('admin/', admin.site.urls),\n"
        "    path('', RedirectView.as_view(pattern_name='notes:note_list')),\n"
        "    path('notes/', include('notes.urls')),\n"
        "]\n"
    )
    with open(path, "w") as f:
        f.write(content)
    print("✓ note_app/urls.py updated")
else:
    print("✓ note_app/urls.py already configured — skipping")
```

---

## Step 10 — Create the HTML Templates

Django templates are HTML files with special `{{ variable }}` and `{% tag %}` syntax added. Django fills in the variables at request time before sending the page to the browser.

Templates for an app named `notes` live at:

<pre><code>notes/templates/notes/   ← two levels deep — the first "notes" is required by Django</code></pre>

**Create the template directory:**

```bash {"interactive":"false","name":"create-template-dir"}
mkdir -p note_app/notes/templates/notes
echo "✓ Template directory created"
```

---

### Template 1 — Note List

`note_list.html` displays all notes. Key Django template features used here:

- `{% load bootstrap5 %}` — loads the bootstrap5 template tag library
- `{% bootstrap_css %}` / `{% bootstrap_javascript %}` — inserts Bootstrap's CSS and JS `<link>`/`<script>` tags
- `{% for note in notes %}` — loops over the `notes` list passed from the view
- `{% url 'notes:note_create' %}` — generates the correct URL for the `note_create` view using its name (avoids hardcoding paths)
- `{{ note.content|truncatechars:32 }}` — a *filter* that shortens the content to 32 characters
- `{% empty %}` — shown if the list is empty

```python {"interactive":"false","name":"write-note-list"}
content = '''\
{% load crispy_forms_tags %}

<!DOCTYPE html>
<html>
<head>
    <title>Note Taking App</title>
    {% load bootstrap5 %}
    {% bootstrap_css %}
    {% bootstrap_javascript %}
</head>
<body>
<div class="container mt-4">
    <h1>My Notes</h1>
    <div class="d-grid gap-2 mb-3">
        <a href="{% url 'notes:note_create' %}" class="btn btn-primary">+ New Note</a>
    </div>
    <hr>
    {% for note in notes %}
    <div class="card mb-2">
        <div class="card-body">
            <h5 class="card-title">{{ note.title }}</h5>
            <p class="card-text text-muted">{{ note.content|truncatechars:32 }}</p>
            <div class="btn-group" role="group">
                <a href="{% url 'notes:note_edit' note.pk %}" class="btn btn-outline-success btn-sm">Edit</a>
                <a href="{% url 'notes:note_delete' note.pk %}" class="btn btn-outline-danger btn-sm">Delete</a>
            </div>
        </div>
    </div>
    {% empty %}
    <div class="alert alert-info">No notes yet — create your first one!</div>
    {% endfor %}
</div>
</body>
</html>
'''

with open("note_app/notes/templates/notes/note_list.html", "w") as f:
    f.write(content)

print("✓ note_list.html written")
```

---

### Template 2 — Create / Edit Form

`note_form.html` is shared by both the create and edit views. The same template handles both cases:

- `{% if form.instance.pk %}` — checks whether the form is bound to an existing database object (edit) or a new one (create). `pk` is the primary key — every database row has one.
- `{{ form|crispy }}` — renders the form fields with Bootstrap styling, thanks to crispy-forms.
- `{% csrf_token %}` — inserts a hidden security token. Django requires this on every POST form to prevent _Cross-Site Request Forgery_ attacks.
- `<button type="submit">` — the Save button that submits the form.

```python {"interactive":"false","name":"write-note-form"}
content = '''\
{% load crispy_forms_tags %}

<!DOCTYPE html>
<html>
<head>
    <title>Note Taking App</title>
    {% load bootstrap5 %}
    {% bootstrap_css %}
    {% bootstrap_javascript %}
</head>
<body>
<div class="container mt-4">
    <h1>{% if form.instance.pk %}Edit{% else %}Create{% endif %} Note</h1>
    <form method="post">
        {% csrf_token %}
        {{ form|crispy }}
        <button type="submit" class="btn btn-primary">Save</button>
        <a href="{% url 'notes:note_list' %}" class="btn btn-secondary ms-2">Cancel</a>
    </form>
</div>
</body>
</html>
'''

with open("note_app/notes/templates/notes/note_form.html", "w") as f:
    f.write(content)

print("✓ note_form.html written")
```

---

### Template 3 — Delete Confirmation

`note_confirm_delete.html` shows the note's title and asks the user to confirm. Deletion only happens on a POST request (when the "Yes, delete" button is clicked), not on a GET request (when the page first loads). This prevents accidental deletions from link prefetching or browser history.

```python {"interactive":"false","name":"write-note-confirm-delete"}
content = '''\
<!DOCTYPE html>
<html>
<head>
    <title>Note Taking App</title>
    {% load bootstrap5 %}
    {% bootstrap_css %}
    {% bootstrap_javascript %}
</head>
<body>
<div class="container mt-4">
    <h1>Delete Note</h1>
    <div class="alert alert-warning">
        Are you sure you want to delete <strong>"{{ note.title }}"</strong>? This cannot be undone.
    </div>
    <form method="post">
        {% csrf_token %}
        <button type="submit" class="btn btn-danger">Yes, delete it</button>
        <a href="{% url 'notes:note_list' %}" class="btn btn-secondary">No, cancel</a>
    </form>
</div>
</body>
</html>
'''

with open("note_app/notes/templates/notes/note_confirm_delete.html", "w") as f:
    f.write(content)

print("✓ note_confirm_delete.html written")
```

---

## Step 11 — Verify the Project Structure

Before starting the server, let us confirm that all the files are in the right places.

```bash {"interactive":"false","name":"verify-structure"}
echo "=== Project layout ==="
find note_app -type f -name "*.py" -o -name "*.html" | sort
echo ""
echo "=== Database file ==="
ls note_app/db.sqlite3 2>/dev/null && echo "db.sqlite3 found" || echo "db.sqlite3 not found — run migrate first"
```

---

## Step 12 — Run the Development Server

`manage.py runserver` starts a lightweight web server built into Django. It is meant for development only — it automatically reloads whenever you save a Python file, so you can see changes immediately without restarting.

```bash {"background":"true","interactive":"true","name":"run-server"}
echo "#####################################################################"
echo "# Access server at: https://8000-$HOSTNAME.ws.graderthan.com/notes/ #"
echo "#####################################################################"
cd note_app && python manage.py runserver 8000
```

> **Accessing the app:**
>
> - **Local machine:** open [http://127.0.0.1:8000](http://127.0.0.1:8000) in your browser
> - **GraderThan workspace:** in your workspace URL, replace `ide` with `8000`
>    - Example: `https://ide-abc123.ws.graderthan.com` → `https://8000-abc123.ws.graderthan.com`
>
> **Run the code cell below to see the proper URL to access your notes server**

```bash
echo "Access server at https://8000-$HOSTNAME.ws.graderthan.com/notes/"
```

---

## What Just Happened?

<video src="media/django-notes-demo.mp4" controls width="100%"></video>

You built a complete web application by connecting five Django building blocks:

<pre><code>Browser
  │
  ▼
urls.py          ← matched /notes/ to note_list view
  │
  ▼
views.py         ← note_list() fetched all Note objects
  │
  ▼
models.py        ← Note.objects.all() queried the SQLite database
  │
  ▼
views.py         ← passed the notes list to render()
  │
  ▼
note_list.html   ← Django filled in {{ note.title }} etc. and returned HTML
  │
  ▼
</code></pre>

---

## Checkpoint — What to Try

Now that the server is running, try each of these to see all the pieces working together:

1. **Create a note** — click "+ New Note", fill in the form, click "Save note"
2. **Edit a note** — click "Edit" on any note, change the content, save
3. **Delete a note** — click "Delete", confirm on the next page
4. **Look at the admin panel** — visit `/admin/` (you will need to create a superuser first with `python manage.py createsuperuser`)

---

## Next Steps

You now know the foundations of Django. In **Django Part 2** you will extend this project with:

- **User authentication** — sign up, log in, log out, and keep each user's notes private
- **PostgreSQL** — swap out SQLite for a production-grade database running in a Docker container

Some things to explore on your own before then:

- Add a `search` field to `note_list` that filters notes by title
- Add pagination so the list does not grow infinitely long
- Try registering the `Note` model in `notes/admin.py` and explore the built-in admin interface
