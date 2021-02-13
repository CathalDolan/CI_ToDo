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

# Forms

Add {% csrf_token %} immeditaley beneath the form tag. This is a security precaution the prevents data being submitted from outside our site.

1. Create new file in the App folder, forms.py
2. In new file: from django import forms
3. In new file, import database class: from .models import class_name
4. Create form class and inherit django's ModelForm from django's forms
    class ItemForm(forms.ModelForm):
        class Meta:
            model = Item
            fields = ['name','done']
5. To allow the form to be used as a template variable, it needs to be imported into views.py 
    from .forms import class_name

# Injecting Database Content into html
This is done via views

1. Create a variable to contain the form data
2. create a context: context = { 'name': name }
3. Return the context in the return render  
    form = ItemForm()
    context = {
        'form': form
    }
4. In html file where form data is to be rendered, we still require the form tags (and button) and put the template variable between it:
    <form method="POST" action="add">
        {% csrf_token %}
        {{ form }}
        <div>
            <p>
                <button type="submit">Add Item</button>
            </p>
        </div>
    </form>
5. For the Post request, we also need to create a variable: form = form_class_name(request.POST)
6. We need to validate the form using is_valid method: if form.is_valid():
7. This is proceeded by a built in save method: form.save
8. This is then redirected to the html
    def add_item(request):
    if request.method =="POST":
        form = ItemForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('get_todo_list')

    form = ItemForm()
    context = {
        'form': form
    }
    return render(request, 'todo/add_item.html', context)
9. We can format the form in html using built in python methods such as as_p to set paragraphs
    <form method="POST" action="add">
        {% csrf_token %}
        {{ form.as_p }}
        <div>
            <p>
                <button type="submit">Add Item</button>
            </p>
        </div>
    </form>

# Deployment

1. Register on Heroku.com or login to an existing account
2. If not already installed on IDE to allow cli, do so:
    $ curl https://cli-assets.heroku.com/install.sh | sh
3. To login via cli:
    heroku login
3.1 If that fails, type npm install -g heroku followed by heroku login -i
4. To see all Heroku commands: heroku in cli
5. To get help, type app name followed by help
6. Migrate database from sqlLite to server based db, e.g. Postgres or ClearDB SQL by installing:
    pip3 install physcopg2-binary
7. Install gunicorn to replace dev server when app is deployed and act as web server:
    pip3 install gunicorn
8. Create a requirements.txt file to tell Heroku what it needfs to install for our app to work:
    pip3 freeze --local > requirements.txt
9. Create Heroku App:
    heroku apps:create app_name --region eu (Name must start with a letter, end with a letter or digit and can only contain lowercase letters, digits, and dashes)
10. Connect Django app to Heroku Postgres:
    - Login to Heroku, open desired App and click on Resources tab
    - In "Add-ons" field, enter Postgres
    - Select Heroku Postgres from presented options
    - On pop-up, leave on "Hobby" setting and click Submit Order Form
    - Postgres will now be visible in the Resources view
11. We need to connect Django App to the new remote DB:
    - In cli type pip3 install dj_database_url
    - Add to requirements with pip3 freeze --local > requirements.txt
    - Get remote database url with heroku config
    - Go to settings.py in the project folder
    - Copy DATABASES and paste it beneath, this is needed for teh Development Environment later on
    - Change 'default' to: 'default': dj_database_url.parse('url obtained in cli from heroku config command')
        - This was changed to 'default': dj_database_url.parse(os.environ.get('DATABASE_URL')) sameish as ALLOWED_HOSTS
        - 
    - Comment out the original DATABASES (It's for sql lite)
    - Import: At the top of the page import dj_database_url
    - Run migrations in cli: python3 manage.py migrate
12. Update .gitignore (if already created. If not create a new file within the App called .gitignore)
    - *.sqlite3
13. Push to Heroku:
    - Add, Commit and Push to GitHub
    - git push heroku master (if on master branch only) Order
    - git push heroku <dev-branch>:name
    - An error will appear after first one (I think if there is no CSS or JS files to push) so type: heroku config:set DISABLE_COLLECTSTATIC=1
    - create a Procfile in project folder. Remember capital P
    - IN Procfile add web: gunicorn django_todo.wsgi:application
14. Add host to settings.py. In "ALLOWED HOSTS" add heroku app url, inbetween single commas. No https//, just the url`
        - this was changed to os.environ.get('HEROKU_HOSTNAME))
        - In Heroku got to Settings and config Vars
        - Add a variable called HEROKU_HOSTNAME
        - In teh corresponding fields add the url minus the https://
        - Click Add
    - Commit and push again to heroku
15. Set-up Automatic pushes from Github to Heroku
    - Go to App Heroku Dashboard and click Deplay tab
    - Go to Deployment Method tab and select GitHub
    - Login to Github if requested to do so
    - Seacrh for Repo name
    - Once found click Connect
    - Under Automatic deploys choose branch and select auto deploy????
16. - Create a Development Environment
    - In settings.py beneath the imports, add development = os.environ.get('DEVELOPMENT', False)
    - Set DEBUG to = development. This means it is only on in dev and not on Heroku
    - Under Databases, uncomment original DATABSES and put into an if statement with the other database
    - Set development environment variable to true
        - Go to workspaces page
        - Click account in top right and then settings
        - Create new variable called DEVELOPMENT and set value to True
        - Restart workspace
        - Under allowed hosts, create an if statement to select host depending on environment

# Secret Key
    - Set default to an empty string
    - Generate a key (eg - https://miniwebtool.com/django-secret-key-generator/) and copy it
    - Create a new variable in the development environment called SECRET_KEY and paste it in (dev environ in the IDE settings)
    - Restart the workspace
    - Create a Secret Key for Heroku and add it to COnvig Var
    


# GitHub Merge Dev & Master branch
1. Login to Github and open required project
2. If present click on "Compare & pull request"
3. Somethging else...
4. Confirm Merge
5. Merge Success
