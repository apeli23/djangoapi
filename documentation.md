# How to create a simple Django API on Linux OS.

## Introduction

In this article, we will learn to develop a REST API using python's Django framework. This will be a step-by-step guide on how to download, install and create a Restful Django API.

## Source Code

Get the GitHub source code [here](https://github.com/apeli23/-reactdjangoapi.git)

## Prerequisites

This article requires a prior entry-level understanding of javascript, react, and python language.

## Installation guide:

### Python
To use Django, you will first be required to have Python installed. Use this [link](https://www.python.org/downloads/) to head to the python downloads page and install the latest Python setup.
![Python Download Page](https://res.cloudinary.com/dlt0f5pvq/image/upload/v1670932954/Screenshot_12_y4gf60.png 'Python Download Page')
You will receive the `<setup>.exe`file in your designated downloads folder. Install it and proceed to the next section

### Django
[Django](https://docs.djangoproject.com/en/4.1/) refers to an advanced web framework built in python that uses Model View Controller(MVC) architectural pattern. It was designed to make development fast and easy.

To install Django, we first need to create a [virtual environment](https://www.geeksforgeeks.org/python-virtual-environment/#:~:text=A%20virtual%20environment%20is%20a,that%20most%20Python%20developers%20use). A virtual environment helps in keeping dependencies required by our project separate from the rest of the system. It implements this concept by creating an isolated python virtual environment for the project. Use the following command to download the virtual environment package

```
Directory: Terminal


sudo pip3 install virtualenv 
```

Now create your virtual environment. You can use a name of your preference. Here we will use `venv`.

```
Directory: Terminal


virtualenv venv
```
Activate your virtual environment.

```
Directory: Terminal


source venv/bin/activate
```
To deactivate it, you can use
```
Directory: Terminal


deactivate
```
On an active virtual environment, use the following command to install Django.

```
Directory: Terminal

python -m pip install Django
```
You can confirm your Django installation using the following command
```
Directory: Terminal

python -m django --version
```


### VSCode
Finally, we will need a code editor to compile our project code. [Visual Studio Code](https://code.visualstudio.com/docs/editor/whyvscode), commonly known as Vscode is a source code editor made by Microsoft containing powerful developer tooling. For this article, we use VSCode as our editor. Use the following [link](https://code.visualstudio.com/download) to access the VSCode download page.

## Project setup
Create your Django project. We will name it `backend`. We are going to create a simple RESTful API. It will include the following steps;
-   Install Django rest framework
-   Build model
-   Create URLs
-   Create Serializers

Head to the `backend` folder
```
cd backend
```
We start by creating an app called `events`.
```
django-admin startapp events
```
Head to the app's models and add the following model;
```
Directory: "backend/events/models.py"

from django.db import models

# Create your models here.
class Events(models.Model):
    title = models.CharField(max_length=100)
    details = models.CharField(max_length=100)
```

Run the following command.

```
Directory: terminal

python manage.py makemigrations events
python manage.py migrate
```
By running `python manage.py makemigrations events`, we inform Django that we have made changes to our models and would like them to be stored as a migration. Changes in the Django models are stored as a migration. To automatically run the migrations and manage our project's database schema, we use : `python managee.py migrate`.

Proceed by installing the Django rest framework using the following command.
```
Directory: Terminal

pip install djangorestframework
```
Configure the framework with the project settings by editing the following in `settings.py`

```
Directory: "backend/backend/settings.py"

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'events',
    'rest_framework'
]
```
Create a new file in the `events` directory called `serializers.py`. Serializers allow complex data- query sets and model instances to be converted easily and rendered into JSON.
In the `serializers.py` directory, paste the following:
```
Directory: backend/events/serializers.py"

from rest_framework import serializers
from .models import Events

class LeadSerializer(serializers.ModelSerializer):
    class Meta:
        model = Events
        fields = ('title', 'details')
```
The code above implements a new class called `LeadSerializer` to utilize the tools within the rest framework serializers to serialize data in our `Events` model.

Now, head to the app's `views.py` directory and paste the following:
```
Directory: "backend/events/views.py":

from .models import Events
from .serializers import LeadSerializer
from rest_framework import generics


class LeadListCreate(generics.ListCreateAPIView):
    queryset = Events.objects.all()
    serializer_class = LeadSerializer
```

In the code above, we utilize the list API view, which is a generic API view and collects all the events from the `events  model`.

We can then configure the Django URLs. Head to the project's `urls.py` directory and replace the contents with the following code
```
Directory: "backend/backend/urls.py";

 
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('events.urls')),
]
```
In the code above, we create a pattern to connect our new app(events) and we include it in the URL path.
At this point, if you head to your api url(http://127.0.0.1:8000/events/api/), you should see something like follows.

![API](https://res.cloudinary.com/dlt0f5pvq/image/upload/v1670932937/Screenshot_11_hdffk9.png 'API')

 
 