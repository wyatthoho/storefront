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