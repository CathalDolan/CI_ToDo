![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)
python3 -m http.server
python3 manage.py runserver
To run a backend Python file, type `python3 app.py`, if your Python file is named `app.py` of course.
ctrl C to stop server

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
8. Run initial migration. In console window type python3 manage.py migrate (To see what will be migrated, put --plan on the end then rerun with out it on the end)

# Create a Super User

A superuser has admin access to the database
1. In console window type python3 manage.py createsuperuser
2. Add username
3. Add email address (can be left blank)
4. Add password (won't be visible)
5. Confirm password (won't be visible)

# Create git Dev branch

All work is done on the dev (development) branch.
1. Ensure master is up to date. In console window, type git pull origin master
2. Create new branch. In console window type git checkout -b dev
3. Add to brnach. In console window type git add . (. commits everything. rather than ., specifiy a specific folder or file if required)
4. Commit to branch. In console window type git commit -m "Comment: What's being committed"
5. Push to Github. In console window type got push origin dev
6. Swicth between branches by typing git checkout branch_name

# Create a Model/database

1. In in App, open models.py
2. Create a new table by creating a new class: class table_name():
3. Inherit the base model class by putting models.Model between the parenthisis
4. Define the table's attributes: attribute_name = models.field_type
5. To see a list of model attributes: https://www.geeksforgeeks.org/django-model-data-types-and-fields-list/
6. Note: To make fields compulsory we include null=False, blank=False
7. Attribute example: "done = models.BooleanField(null=False, blank=False, default=False)"
8. Save all changes and additions
9. Migrate the model. In console window type python3 manage.py makemigrations
10. Do a dry run first to check what is being migrated by putting --dry-run on the end. Don't include for actual migration
11. Apply migration. In terminal window type python3 manage.py migrate ((include --plan on end for a dry run))
12. Register the modal. In the App folder, open admin.py
13. Import the model: from .models import class_name
14. Register the model: admin.site.register(class_name)
15. Import the class into views so that we can access the data on front end. In App views.py add from .models import class_name
16. In views.py function, create an instance of the data via a variable var_name = class_name.object.all()
17. In views.py create context to all us access data in html via a template variable, context = {'name': var_name}
18. Add the context as a thrid parameter in the return render
15. To view migrations status', type python3 manage.py show migrations

# Manually Add Data to an Existing table

1. Log into the Admin area as the Super User
2. Go to the table to be edited and click "Add Item" from top right corner
3. Add data and click "Save"
4. Display actual name as opposed to default object string method name: In App models.py, go o the class and ass a function:
    def __str__(self):
        return self.name
