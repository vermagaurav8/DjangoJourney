# Django Journey

---

# what is Django?

Django is a free, open-source web framework that is written in Python. It follows the Model-View-Controller (MVC) architectural pattern and provides a simple and efficient way to build web applications. Django was first released in 2005 and has since become one of the most popular web frameworks for Python developers.

Django includes a lot of features out of the box, such as an ORM (Object-Relational Mapping) system for database communication, URL routing, authentication, and administration interfaces. It also provides built-in support for handling common web development tasks like form handling, file uploading, and caching.

One of the key advantages of Django is its flexibility and scalability. It allows developers to quickly build prototypes and scale up to handle high traffic websites. Additionally, Django has a large and active community, which means there are many third-party packages available for extending its functionality.

---

## How Web Works?

The web has 2 parts Front-end and back-end, the front-end interacts with the client

1. [heygaurav.me](http://heygaurav.me) : URL(Uniform Resource Locator), which points to some resource on the internet.
2. when we go to domain, the front-end sends a request to the server that host the website and receives a response.
3. The request are made through protocols know as HTTP.
4. Server provide endpoint to interact with clients.
5. API (Application Programming interface), an interface helps client to interact with endpoints.

---

## Basics

- Python : Lang  to work with django.
- pip: Package installer for python.
- pipenv: Virtual environment  created using pip, so dependencies of apps don’t mix up.

## Django Environment

- **pipenv install django** Initialises pipfile which contain dependency of the project (like package,json)
- **django-admin startproject <proj-name> .**   Initialise project files.

```jsx
**__init__.py**     (says use it as python package)
**url.py**          (Contain all the urls of the django app)
**settings.py**     (Contain settings of the project)
**asgi.py/wsgi.py** (used for deployment)

manage.py        (Manages the whole project)

Run project using  **python3 manage.py runserver**
```

---

## Creating apps

```python
python3 manage.py startapp <appname>
```

- It creates apps associated to project which has following components.
    - [admin.py](http://admin.py) Admin file for the app.
    - [apps.py](http://apps.py) Config file for the app.
    - [model.py](http://model.py) Create Model for the app.
    - [test.py](http://test.py) Contain tests.
    - [views.py](http://views.py) Contain views used in request/response.
    - Another folder migration which propagates changes made to your model and this folder tracks all of that.

- Views file manage the request and response between the client and server like how to handle that and what to do with that HTTP response.

---

## Mapping URLs to Views

- For sending a request to a custom url and getting a response, you have to create [urls.py](http://urls.py) (not this name mandatory, just a convention) in your app.
- Map URLs to views(Implement functions in views) using path function like this:

```python
#<appname>/views.py
from django.shortcuts import render
from django.http import HttpResponse

def say_hello(requests):
    return HttpResponse('Hello World')
```

```python
#<appname>/urls.py
from django.urls import path
from . import views

# URL Configuration 
urlpatterns = [
    path('hello/', views.say_hello)
]
```

- And include that mapping in Django-admin [urls.py](http://urls.py) using include function to get that working.

```python
#<django-admin startproject>/urls.py 
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls'))
]
```

- if we go to [http://127.0.0.1:8000/playground/hello/](http://127.0.0.1:8000/playground/hello/) it should respond with Hello World.

---

## Django Templates

Views are not really views in django, they are more like request handlers. What views in JS are called Templates in Django which is defined under the template folder in app that was created by django admin project 

Templates generally return what we see on the website (generally html) so all the files place under template and we can modify html using functions and render them using render function from django.shortcuts module.

```python
#<appname>/views.py
from django.shortcuts import render
from django.http import HttpResponse

def say_hello(request):
    return render(request, 'hello.html', { 'name': 'Gaurav'})

#<appname>/templates/hello.html
{% if name %}
<h1>Hello {{ name }}</h1>
{% else %}
<h1>Hello World</h1>
{% endif %}

# Check live on server running on http://127.0.0.1:8000/playground/hello/
```

**Imp:** But templates are not generally used in today’s world, because djnago is mainly used for API development which send data, not HTML to render on front-end.

---

## Debugging

We can use traditional way of using debugger in VS code IDE or there is an awesome package for debugging django app known as **django-debug-toolbar**

django-debug-toolbar provides an interactive way to debug files and see aspects like history, versions , logs, SQL connection and their management with project. The way to use this package in your project are:

```python
# install using pip
pip install django-debug-toolbar

# in <django-admin project>/settings.py file add some params
1. INSTALLED_APPS = [
    # ...
    "debug_toolbar",
    # ...
		]
2. MIDDLEWARE = [
    # ...
    "debug_toolbar.middleware.DebugToolbarMiddleware",
    # ...
		]
3. INTERNAL_IPS = [
    # ...
    "127.0.0.1",
    # ...
		]

# in <django-admin project>/urls.py add
1. from django.urls import include, path

urlpatterns = [
    # ...
    path('__debug__/', include('debug_toolbar.urls')),
]
```

---

## Data Modeling

Data modeling in Django is the process of defining the structure of your database tables and relationships between them using Python classes. Django provides a built-in Object-Relational Mapping (ORM) system that allows developers to interact with the database using Python objects rather than writing SQL queries.

In Django, data models are defined in a Python file called **`models.py`**, which is typically located in each app directory. Each data model is represented by a Python class that inherits from the **`django.db.models.Model`** class.

To define a data model, you define a set of fields that represent the columns of the corresponding database table. Django provides a wide variety of field types, including **`CharField`**, **`IntegerField`**, **`ForeignKey`**, **`ManyToManyField`**, and many more.

You can also define relationships between data models using ForeignKey and ManyToManyField fields. These relationships allow you to create complex database structures and query them using the ORM.

Once you have defined your data models, you can use Django's built-in tools to create database tables and relationships based on these models. You can create tables by running the **`migrate`** command, which generates the necessary SQL statements based on the data models and applies them to the database.

Overall, Django's data modeling system provides a powerful and flexible way to define and manage your database schema using Python code, rather than SQL queries.
