![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)
python3 -m http.server
python3 manage.py runserver
To run a backend Python file, type `python3 app.py`, if your Python file is named `app.py` of course.

# Creating  Django Project

1. Install Django using PIP (Pip Installs Packages). In console window type pip3 install django
2. Create Project. In console window type django-admin startproject project_name . (Full stop to put into current directory)

# Creating an App in Django

1. Create app by typing into the console window python3 manage.py startapp app_name

# Render to Frontend

1. In the App folder, go to views.py and create required function
    def fn_name(request):
        return render(request, 'app_name/html_page_name.html')
2. In project folder, go to urls.py:
-- Import the function: from app_folder.views.py import fn_name
-- Create Url Pattern: path('url/', fn_name, name='') (To create home or default landing page, leave url blank, just '')
3. In the App folder, create a new folder called templates
4. Inside the new templates folder, create a new folder named to match the App app_folder
5. Inside this new folder, create a new html file (named appropriately)
6. In new html file, create basic structure
7. In project folder, go to settings.py. Add the app to INSTALLED_APPS by typing 'app_name' 