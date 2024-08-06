# Basic CURD Operations
#### Go through Home page once, before you read the info below
#### Turn on Autosave in your VSC
This is will give you a basic idea about how we do a Django project.

As we discussed, the first step is to create a virtual environment and install Django.

    python -m venv myenv
    myenv\Scripts\activate
    pip install django

Then, Create Project:

    django-admin startproject mypro
    cd mypro

Create a new app within your project:

    python manage.py startapp myapp

Configure Your Project:
Edit 'myproject/settings.py' to add your new app to the 'INSTALLED_APPS' list

          INSTALLED_APPS = [
              ...
              'newapp',
          ]

Edit 'myproject/settings.py' to add your new app to the 'TEMPLATES' list

          TEMPLATES = [
              ...
              'DIRS': [BASE_DIR,'templates'],
          ]


#### Define Models
I want to create a models for homepage and contact page.
Edit newapp/models.py:

    from django.db import models

    class Member(models.Model):
        firstname=models.CharField(max_length=100)
        lastname=models.CharField(max_length=100)
        country=models.CharField(max_length=100)

Save it, and do migrations

Open Terminal:

    python manage.py makemigrations

    python manage.py migrate

Here you can create superuser:

    python manage.py createsuperuser

and enter the required credentials.


#### Define admin
I want to create a views for homepage and contact page.
Edit newapp/admin.py:

    from django.contrib import admin
    from .models import Member

    class MemberAdmin(admin.ModelAdmin):
        list_display="firstname","lastname","country"

    admin.site.register(Member,MemberAdmin)

#### Define Views

I want to create a views for homepage and contact page.
Edit newapp/views.py:

        from django.shortcuts import redirect, render
        from .models import Member

        def index(request):
            mem=Member.objects.all()
            return render(request,'index.html',{'mem':mem})

        def add(request):
            return render(request,'add.html')

        def addrec(request):
            x=request.POST['first']
            y=request.POST['last']
            z=request.POST['country']
            mem=Member(firstname=x,lastname=y,country=z)
            mem.save()
            return redirect("/")

        def delete(request,id):
            mem=Member.objects.get(id=id)
            mem.delete()
            return redirect("/")

        def update(request,id):
            mem=Member.objects.get(id=id)
            return render(request,'update.html',{'mem':mem})

        def uprec(request,id):
            x=request.POST['first']
            y=request.POST['last']
            z=request.POST['country']
            mem=Member.objects.get(id=id)
            mem.firstname=x
            mem.lastname=y
            mem.country=z
            mem.save()
            return redirect("/")

#### Create Templates Folder

In Base Directory,

    mkdir templates

In that folder, create required html files. We need add,index and update html files now as per Views.

**index.html**

    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    {% load static %}
    <link rel="stylesheet" href="{% static 'style.css' %}">

    <body>
        <div class="container">
            <h1>Members</h1>
            <table id="customers" border="1">
                <thead>
                    <th>Id</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Country</th>
                    <th colspan="2" id="mid">Action</th>
                </thead>
                {% for x in mem %}
                <tr>
                    <td>{{x.id}}</td>
                    <td>{{x.firstname}}</td>
                    <td>{{x.lastname}}</td>
                    <td>{{x.country}}</td>
                    <td>
                        <a href="update/{{x.id}}"><button id="up">update</button></a>
                    </td>
                    <td>
                        <a href="delete/{{x.id}}"><button id="del">delete</button></a>
                    </td>
                </tr>
                {% endfor %}
            </table>
            <br><br>
            <a href="{% url 'add' %}"><button id="new3">Add Member</button></a>
        </div>
    </body>
    </html>  

**add.html**

    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    {% load static %}
    <link rel="stylesheet" href="{% static 'style.css' %}">

    <body>
        <div class="container1">
            <h1>Add Member</h1>
            <form action="{% url 'addrec' %}" method="post">
                {% csrf_token %}
                <label for="">First Name</label><br><br>
                <input type="text" name="first"><br><br>
                <label for="">Last Name</label><br><br>
                <input type="text" name="last"><br><br>
                <label for="">Country</label><br><br>
                <input type="text" name="country"><br><br>
                <button type="submit" id="new">Submit</button>
            </form>
        </div>
    </body>

    </html>

**update.html**

    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    {% load static %}
    <link rel="stylesheet" href="{% static 'style.css' %}">

    <body>
        <div class="container2">
            <h1>Update Member</h1>
            <form action="{% url 'uprec' mem.id %}" method="post">
                {% csrf_token %}
                <label for="">First name</label><br><br>
                <input type="text" name="first" value="{{mem.firstname}}"><br><br>
                <label for="">Last name</label><br><br>
                <input type="text" name="last" value="{{mem.lastname}}"><br><br>
                <label for="">Country</label><br><br>
                <input type="text" name="country" value="{{mem.country}}"><br><br>
                <button type="submit" id="new1">Update</button>
            </form>

        </div>
    </body>

    </html>

#### Set Up URLs

Create a urls.py file in the myapp directory (if it doesn't already exist) and define the URL patterns:

    from django.urls import path
    from . import views

    urlpatterns = [
        path('',views.index,name="index"),
        path('add/',views.add,name="add"),
        path("addrec/",views.addrec,name="addrec"),
        path('delete/<int:id>/',views.delete,name="delete"),
        path('update/<int:id>/',views.update,name="update"),
        path('update/uprec/<int:id>/',views.uprec,name="uprec")
    ]

Edit **mypro/urls.py** to include your app’s URLs:

    from django.contrib import admin
    from django.urls import path,include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path("",include('newapp.urls'))
    ]

                  RUN THE SERVER, HAPPY LEARNING!!
