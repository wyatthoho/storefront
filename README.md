# storefront

This repository is my practice work based on 
the instructions from the YouTube video 
[Python Django Tutorial for Beginners](https://youtu.be/rHux0gMZ3Eg?si=nLOUg8gMsjWm2-2j).


# Notes

## Create the Virtual Environment
I’m using Python version 3.12.1 on a Windows system. 

Instead of using `pipenv` as shown in the video, 
I used the built-in `venv` package to manage the 
virtual environment. 

To create the environment, execute the following command:
```
py -m venv env
```

Afterward, activate the environment with:
```
.\env\Scripts\activate
```

The required packages are listed in the `requirements.txt` file.
Install the required packages into the environment with:
```
pip install -r .\requirements.txt
```


## Initialize the Django Project
Start the project at the current directory with:
```
django-admin startproject storefront .
```

To start the development server, run:
```
python manage.py runserver
```

The development server will be accessible 
at `http://127.0.0.1:8000/`.

Next, remove `'django.contrib.sessions'` from the 
automatically generated `INSTALLED_APPS` list in 
`.\storefront\settings.py`. The session is a legacy 
feature that we no longer use; it was originally intended 
for temporarily managing user data on the server.

After making this change, the `INSTALLED_APPS` should look 
like this:
```Python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

## Create Our Own APPs

To create an app called `playground`, run the 
following command in your Django project directory:

```
python manage.py startapp playground
```

This command will create a new app folder called 
`playground`, with a specific directory structure.

```
.
├── playground
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── ...
└── .gitignore
```

The new app follows a standard Django app 
structure. Here's a brief explanation of the 
key files and directories:

- `migrations/`: for generating database tables
- `__init__.py`: marks the directory as a Python package
- `admin.py`: defines how the admin interface is going to look like
- `apps.py`: contains the configurations for the app
- `models.py`: defines the model classes for the app
- `test.py`: where we write the unit tests
- `views.py`: we will talk about in the next lesson

Next, register the app in the `.\storefront\settings.py `
file by adding it to the `INSTALLED_APPS` list:

```Python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'playground',
]
```

## Writing Views

HTTP is a **response request protocal**, so every data 
exchange involves a request and a response. This is 
where **views** come into play in Django.

Create a new file `.\playground\views.py`:

```Python
from django.shortcuts import render
from django.http import HttpResponse


def say_hello(request):
    return HttpResponse('Hello, World!')
```


## Mapping URLs to Views

Add a new file `.\playground\urls.py`. You can
name it anything you like, but by convention, 
we call it `urls.py`.

```Python
from django.urls import path
from . import views

# URLConf
urlpatterns = [
    path('hello/', views.say_hello)
]
```

Every app can have its own URL configuration.
Now we need to import this URL configuration into 
the main URL configuration for the project.

Modify the `.\storefront\urls.py`:

```Python
"""
Skip the comments...
"""
import debug_toolbar
from django.contrib import admin
from django.urls import path, include

# playground/hello
urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls')),
]
```

## Using Templates

Let's see how we can use a **template** to return
**HTML** content to the client.

We're going to add a folder `.\playground\templates`.
In this folder, we're going to add a new file called
`hello.html`. We can write some HTML markup, for example:

```html
<html>
<body>
    <h1>Hello World</h1>
</body>
</html>
```

Back to `.\playground\views.py`, we're going to
use the render function to render a template and
return HTML markup to the client.

```Python
from django.shortcuts import render
from django.http import HttpResponse


def say_hello(request):
    return render(request, 'hello.html')
```

We can dynamically render some value. There 
is a context object and the type of this is a
mapping of string to Any. So, here we can pass
a dictionary.

```Python
from django.shortcuts import render
from django.http import HttpResponse


def say_hello(request):
    return render(request, 'hello.html', {'name': 'wyatthoho'})
```

Now, back to our template. Instead of `Hello, World!`,
we can render the name that we passed here.

```html
<html>

<body>
    {% if name %}
    <h1>Hello {{name}}</h1>
    {% else %}
    <h1>Hello, World!</h1>
    {% endif %}
</body>

</html>
```

## Using Django Debug Toolbar

First, we have to install Django Debug Toolbar:

```
pip install django-debug-toolbar
```

The next step is to add debug toolbar in the list
of `INSTALLED_APPS` in `.\storefront\settings.py`:

```python
INSTALLED_APPS = [
    # ...,
    'debug_toolbar',
]
```

The next step is to add a new URL pattern in 
`.\storefront\urls.py`:

```python
import debug_toolbar

urlpatterns = [
    # ...,
    path('__debug__/', include(debug_toolbar.urls)),
]
```

The next step is to add a **middleware**. We use
middleware to hook into Django's request response
processing. In `.\storefront\settings.py` we're going
to add this line:

```python
MIDDLEWARE = [
    # ...,
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    # ...,
]
```

The final step is to add our IP address in the 
`INTERNAL_IPS` in `.\storefront\settings.py`:

```python
INTERNAL_IPS = [
    '127.0.0.1',
]
```

Because the toolbar only appears when we return 
a proper HTML document, we must have to create
the **head** and **body** elements, that is:

```html
<html>
    <body>
    <!-- ... -->
    </body>
</html>
```