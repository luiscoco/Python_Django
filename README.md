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

![image](https://github.com/luiscoco/Python_Django/assets/32194879/8b826bda-01f6-4503-9711-e6abf03a7816)

## 4. Map the View to a URL

For the new view to be accessible by visiting a URL, you need to create a URL pattern for it

First, ensure that your app knows about its own URLs

Create a file called **urls.py** inside your **myapp** directory if it's not already there

![image](https://github.com/luiscoco/Python_Django/assets/32194879/d00be9d1-8468-4b19-9d83-93821b48b9c1)

In **myapp/urls.py**, write the following code:

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

![image](https://github.com/luiscoco/Python_Django/assets/32194879/61473497-a68a-4b2c-b2d0-c2fdcd09f386)

## 5. Run Your Development Server

**Applying Migrations**

**Migrations** are Django's way of propagating changes you make to your models (adding a field, deleting a model, etc.) into your database schema. Here’s how you can apply migrations:

In your terminal where you are running the server, stop the server by pressing CTRL+C

Run the following command to apply the migrations:

```
python manage.py migrate
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/d05da70d-27e3-49f0-81bf-735641df18cf)

This command applies all the default migrations necessary for Django's built-in apps to function, such as the admin, auth, and sessions apps

It creates the necessary database tables for them

Run the server again with:

```
python manage.py runserver
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/e52baf39-6393-4c40-a8da-c37c27036bb5)

Now, you should be able to visit **http://127.0.0.1:8000/hello/** and see **"Hello, world!"** displayed

This approach organizes your project into discrete apps, making it more maintainable as it grows

![image](https://github.com/luiscoco/Python_Django/assets/32194879/e05362b6-9e33-4cda-93f3-0e947a4eb455)

## 6. How to add more features to my first django application

Let’s expand your first Django application by adding more features

We can include a model to store data, create a form to submit data, and build templates to display web pages. Here's a guide to add these elements:

![image](https://github.com/luiscoco/Python_Django/assets/32194879/222e26ac-df41-4c6e-8b99-17e7f88dfa00)

### 6.1. Define a Model

Models in Django are Python classes that define the structure of an application’s data

Let’s create a simple model to store messages

Open **models.py** in your myapp directory

Add the following model definition:

```python
from django.db import models

class Message(models.Model):
    text = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Message(id={self.id}, text={self.text})"
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/32517554-da3d-4266-963e-540cc899cebb)

After defining the model, you need to **create a migration file** for these changes and then migrate to apply them to your database

Run these commands:

```
python manage.py makemigrations
python manage.py migrate
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/35e0b3d9-0f14-49f6-9788-be6e2b4b4a80)

### 6.2. Create a Form

Forms in Django handle the rendering of HTML forms and the extraction of data from submitted forms

Let’s create a form for the Message model

Create a file **forms.py** in the **myapp** directory

Add the following code to forms.py:

```python
from django import forms
from .models import Message

class MessageForm(forms.ModelForm):
    class Meta:
        model = Message
        fields = ['text']
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/ba0fa7ae-3e8e-4827-ba36-b4ba0323bab2)

### 6.3. Update Views

Let’s update the views to handle showing a form and saving data from it

Open **views.py** in the **myapp** directory

Add the following code:

```python
from django.shortcuts import render, redirect
from .forms import MessageForm
from .models import Message

def hello_world(request):
    if request.method == 'POST':
        form = MessageForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('hello')
    else:
        form = MessageForm()
    messages = Message.objects.all().order_by('-created_at')
    return render(request, 'hello.html', {'form': form, 'messages': messages})
```

![image](https://github.com/luiscoco/Python_Django/assets/32194879/a24fd658-32f4-4f91-a479-606721318f7d)

### 6.4. Create Templates

Create a directory called templates in your **myapp** directory, and inside that, create a file called **hello.html**

Add the following HTML code to **hello.html**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
</head>
<body>
    <h1>Hello, world!</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Send</button>
    </form>
    <ul>
        {% for message in messages %}
        <li>{{ message.text }} ({{ message.created_at }})</li>
        {% endfor %}
    </ul>
</body>
</html>
```

This template displays a form and lists all messages saved in the database

![image](https://github.com/luiscoco/Python_Django/assets/32194879/efed5ed2-fb57-46b2-9eb3-06e78c37fa27)

### 6.5. Update the Project URLs

Make sure your project’s **urls.py** is set to find the templates by updating the settings:

In **settings.py**, make sure you have the following:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'myapp/templates'],
        'APP_DIRS': True,
        ...
    },
]
```

Now, your application can handle data input through a form and display stored data

Run your server to see these features in action:

```
python manage.py runserver
```

Navigate to **http://127.0.0.1:8000/hello/** to see the form and list of messages

This setup demonstrates a basic CRUD (Create, Read, Update, Delete) operation in Django, tailored to creating and reading operations

![image](https://github.com/luiscoco/Python_Django/assets/32194879/99122b1e-3270-43fb-ab90-0480f3b35540)

![image](https://github.com/luiscoco/Python_Django/assets/32194879/a37a5732-3e45-4575-8c23-ed48d3715700)
