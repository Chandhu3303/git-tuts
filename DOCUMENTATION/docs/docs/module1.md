# Basic Web Application
#### Go through Home page once, before you read the info below
This is will give you a basic idea about how we do a Django project.

As we discussed, the first step is to create a virtual environment and install Django.

    python -m venv myenv
    myenv\Scripts\activate
    pip install django

Then, Create Project:

    django-admin startproject myproject
    cd myproject

Create a new app within your project:

    python manage.py startapp myapp

Configure Your Project:
Edit 'myproject/settings.py' to add your new app to the 'INSTALLED_APPS' list

          INSTALLED_APPS = [
              ...
              'myapp',
          ]

#### Define Views

I want to create a views for homepage and contact page.
Edit myapp/views.py:

    from django.http import HttpResponse
    from django.shortcuts import render

    def home(request):
        return HttpResponse("Welcome to the homepage!")

    def contact(request):
        return HttpResponse("Contact us at SRIT")

#### Set Up URLs

Create a urls.py file in the myapp directory (if it doesn't already exist) and define the URL patterns:

    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='home'),
        path('contact/', views.contact, name='contact'),
    ]

Edit **myproject/urls.py** to include your app’s URLs:

    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('myapp.urls')),
    ]

#### Run the Development Server

    python manage.py runserver

Open your web browser and navigate to http://127.0.0.1:8000/ to see the homepage, and http://127.0.0.1:8000/contact/ to see the contact page.


                  REVISIT HOMEPAGE IF YOU GOT STUCK, HAPPY LEARNING!!

 

