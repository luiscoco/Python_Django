# Python a brief introduction to Django

https://www.djangoproject.com/

**Django** is a powerful **web framework** for **Python**, ideal for building **web applications** quickly and efficiently

Below, I'll provide you with a couple of simple Django code samples that you can try out in Visual Studio Code (VSCode)

## 1. Setting Up a Basic Django Project

First, let's set up a new Django project. You'll need Python and Django installed

You can **install Django** using **pip** if you haven't already:

```
pip install django
```

After installing Django, you can create a new Django project by running:

```
django-admin startproject myproject
```

This command creates a new directory called **myproject** with the basic structure of a Django project

![image](https://github.com/luiscoco/Python_Django/assets/32194879/6dcf992d-517a-4d6f-bf91-342f082b1c81)

## 2. Create an App

In Django, an app is a web application that does something – e.g., a weblog, a database of public records, or a small poll app

Each app is supposed to have a single, focused purpose and can be reused in different projects if needed

In your terminal, navigate to your project directory where **manage.py** is located

Create a new app by running:

```
python manage.py startapp myapp
```

This command will create a new directory called **myapp** inside your project directory

This directory will include several files, one of which will be **views.py**

![image](https://github.com/luiscoco/Python_Django/assets/32194879/93ae0cb7-50f5-4017-be5e-7edc0fa4e2d6)

## 3. Create a View in views.py

Open the **views.py** file in the myapp directory

Add the following simple view:

```python
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse("Hello, world!")
```

## 4. Map the View to a URL

For the new view to be accessible by visiting a URL, you need to create a URL pattern for it:

First, ensure that your app knows about its own URLs

Create a file called urls.py inside your myapp directory if it's not already there

In myapp/urls.py, write the following code:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.hello_world, name='hello'),
]
```

Now, include the myapp URLs in your project's main URL configuration in **myproject/urls.py**

Modify it to include your app’s URL configurations:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hello/', include('myapp.urls')),
]
```

## 5. Run Your Development Server

Run the server again with:

```
python manage.py runserver
```

Now, you should be able to visit **http://127.0.0.1:8000/hello/** and see **"Hello, world!"** displayed

This approach organizes your project into discrete apps, making it more maintainable as it grows

