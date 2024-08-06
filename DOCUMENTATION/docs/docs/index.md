# Welcome All

This is the documentation for Django.

## Batteries-Included Philosophy

Django follows a "batteries-included" philosophy, meaning it comes with a wide range of built-in features and tools that handle common web development tasks, such as:

- Authentication
- URL routing
- ORM (Object-Relational Mapping)
- Form handling and validation
- Templating engine
- Admin interface

## Installation

#### Set Up a Virtual Environment
It's a good practice to use a virtual environment for your Python projects to manage dependencies separately.

*Windows*:

    python -m venv myenv

*macOS/Linux*:

    python3 -m venv myenv
#### Activate the virtual environment

*Windows*:

    myenv\Scripts\activate

*macOS/Linux*:

    source myenv/bin/activate

#### Install DJango
With your virtual environment activated, install Django using pip:

    pip install django

To verify that Django is installed correctly, run the following command:


    python -m django --version

#### Create a Django Project
Create a new Django project using the django-admin command:


    django-admin startproject projectname

This creates a new directory named 'projectname' with the following structure:

    projectname/

      manage.py

      projectname/

        __init__.py

        settings.py

        urls.py

        wsgi.py

        asgi.py

#### Run the Development Server
Navigate to the project directory and run the development server to ensure everything is set up correctly:

    cd myproject
    python manage.py runserver

You should see output indicating the server is running. Open a web browser and go to http://127.0.0.1:8000/. You should see the Django welcome page.

#### Create a Django App
**NOTE**:Inside your project directory, create a new Django app:

    python manage.py startapp myapp

This creates a new directory named myapp with the following structure:

    myapp/

     __init__.py

     admin.py

     apps.py

     models.py

     tests.py

     views.py

     migrations/

        __init__.py

#### Configure the App
Add your new app to the INSTALLED_APPS list in projectname/settings.py:

          INSTALLED_APPS = [
              ...
              'myapp',
          ]






