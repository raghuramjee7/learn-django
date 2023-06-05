## Setup  

To begin with setting up your first Django project, use the following steps  

First, let us understand more about virtual environments.  

### Virtual Environment

Virtual environment is the way to create a isolated environment in your computer with all your packages, libraries in the same version without any conflicts with global versions.  

Python has **venv** , an inbuilt library that is used to create and maintain virtual environments. Let us look at the setup 

**Setup**: python -m venv <env_name>

It is a common practise to name the environment as .venv, so that it is hidden as well as the naming is clear.

After the venv is setup, use this statement to set execution policy "**Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser**" This policy is used for running scripts in windows

Once after it is created, the virtual environment needs to be activated. We have a command for it in windows - **.venv\Scripts\Activate.ps1**. This will activate the *.venv* environment that has been created, you can see that the terminal will now be of the form (.venv)  

To leave the virtual environment, use the **deactivate** command in the shell, you can use these 2 sets of commands to activate and deactivate virtual environments in venv.  

Once you ar inside the venv, you can install all the packages that you want to install, a good practise of installing packages is using the following format - **python -m pip install <package>**  




## First Django Project  

Let us go ahead and create a new django project, for this use the following command:  **django-admin startproject <project_name> .**

Here we end with a ".", this is just a structure related thing. If you did not add a period, then (lets say project name is zeus) django creates a zeus root folder, inside there will be a zeus subfolder and manage.py file and .venv, if you add the ".", the root folder won't get created.

Now, we can run the internal server by running **python manage.py runserver** command, this is done for internal development purpose only, but when we want to deploy, we will shift to a more robust WSGI server called *Gunicorn*.

We can use ctrl+C to exit from the internal deployment.

Since we have created a project with a given project name, we can take some time to explore the directory that django has provided for us, the different folders and files that are created when you run startproject. Let us take a deep dive into this.  

First a folder, and a file are created, the foldername is the same as the projectname that is given, and a file called *manage.py* is created, this along with the already existing *.venv* folder.
In the folder, there are 5 different .py files. We can take a look at each of them:  

1. \_\_init__.py - This initialises a package for the project. Without this, we might not be able to import stuff from this package.
2. asgi.py - It allows for Asynchronous Server Gateway Interface to be run.
3. urls.py - This tells django which pages to build for a URL request
4. wsgi.py - This is used for Web Server Gateway Interface, which allows django to serve web pages.
5. manage.py - This is not a part of the project but it is used to run various Django commands.

When we run the webserver commmand, we see a warning of unapplied migrations, this warning is saying that we have not migrated our initial database. We can get rid of this by **python manage.py migrate**. This command will create a SQLite database and migrated its built in apps that are provided to us, you can see a new file in the structure called *db.sqlite3*.

Let us take a quick look at HTTP Request/Response Cycle

### HTTP Request/Response Cycle

A HTTP request is sent to a URL by the client and the server responds with a response. 

Flow :  
HTTP Request -> URL -> Django combines database, logic, styling ->
HTTP Response

**MVC Vs MVT**

MVC stands for Model-View-Controller architecture, it seperates the data, logic and display of a website. Many web frameworks like Spring, Laravel and .NET, many use it. The components of MVC are
1. Model - Manages data and control logic
2. View - Sends data in a particular format
3. Controller - Takes user input and performs logic on it

Django loosely follows the MVC architecture, it has its own version called the MVT - Model View Template architecture. It actually has a extra structure called MVTU - an extra URL Configuration component. 

1. Model: Manages data and core business logic
2. View: Describes which data is sent to the user but not its presentation
3. Template: Presents the data as HTML with optional CSS, JavaScript, and Static Assets
4. URL Configuration: Regular-expression components configured to a view

Let us understand the django flow of a project.  

1. When you enter a URL, the URL pattern in urls.py is found. 
2. This pattern is linked to a view in views.py
3. This view has a model, from models.py, where is gets some data and the styling from the template which is returned to user. 

HTTP Request -> URL -> View -> Model and Template -> HTTP Response

Django uses the concept of projects and apps as a software practise. A high level project contains many apps and each app runs a functionality.

We can create an app in django with the following command: **python manage.py startapp <app_name>**

This creates an app folder inside the project folder with the following files  

1. \_\_init__.py
2. admin.py
3. apps.py
4. models.py
5. tests.py
6. views.py
7. migrations/ folder which has an \_\_init__.py file inside it.

Let us look at what each file does - 

1. admin.py is a configuration file for the built-in Django Admin app
2. apps.py is a configuration file for the app itself 
3. migrations/ keeps track of any changes to our models.py file so it stays in sync with our database
4. models.py is where we define our database models which Django automatically translates into database tables
5. tests.py is for app-specific tests
6. views.py is where we handle the request/response logic for our web app


Even though the app is created, django project doesnt know it exists, so we need to go to settings.py file in project and add it. 

In the settings.py file, find *INSTALLED\_APPS* and add *"pages.apps.PagesConfig",* to it, here *pages* is the appname which can change.

Go to the pages app folder, and inside go to apps.py, where you will find PagesConfig class, with its name and its model


## Creating a View

All views are created in views.py. Open that file and create a function that returns a HttpResponse object (django.HttpResponse) for a request.

In django, there are three types of views  
1. Function Based Views
2. Class Based Views
3. Generic Class Based Views

Next, need to create a new file called *urls.py* for the app, note that there also exists another *urls.py* for the whole project.

In the *urls.py* for the app, import the necessary variables like path and create  list variable called *urlpatterns*, in that add a *path(\<regex>, \<view function>, name = \<alias>)* and add the view to the variable.

Here, the regex pattern provided references to the view, and there is an optional named url pattern variable.

Next, go to urls.py in the project level and add this *path("", include("pages.urls")),* to the urlpatterns variable there. Here, pages.urls refers to the urls files of the app that has been created.

So, here the flow goes from top level urls.py to app level urls.py.

### GIT

1. Initialise the git repo inside the venv and add the *.venv/* folder to gitignore.
2. We can use the **pip freeze > requirements.txt** to generate a txt file of all the packages used in virtual environment.


## Lets explore pages app further

We can create views in two different ways  
1. You can create a templates folder in the app, and create a subfolder with app name and then put your template files there. eg: pages -> templates -> pages -> index.html
2. We can create a project level templates folder and put our templates there. We need to tweak our seeting.py file in django_project for this


Let us try the second appraoch.  
Create a new folder - mkdir templates  
Go to *settings.py* and find the *Templates* list variable, inside find the *DIRS* list and add **[BASE_DIR / "templates"],**  to that.

Create a new *home.html* in templates and add a new view to it.

### Class Based Views  

Earlier, django only supported function-based views, but the need for a more generic, extensible class based views gave developer more reusability and flexibility to work with them.

Open the *views.py* file in pages.  
There is a class in django.views.generic called TemplateView. We shall use that.

Import TemplateView, create a new class *HomePageView* that extends from it, and initialise **template_name = "home.html"**

Now we have to update the urls, here we have urls.py at a project level and urls.py at app level.  

In the project level urls.py we need to add urls.py from pages, and then we update urls in pages for the page to render.

Go to urls.py in pages and similar to how you updated last time, keep everything the same, except earlier you added the function name in urlpatterns, now you add **ClassName.as_view()** to it.

Similarly lets create an about page as well. For the path function, in earlier homepage, we left the first argument as "", but for about page, please add "about/" this will create a new url path for that page.

### Extending Tempaltes

We can extend templates in django, we could create a base.html sort of base template and extend the other templates on this. Lets create a base template that has home and about page in urls.

Lets look at the extending template part a bit.
We have a jinja syntax that can take care of this, to directly link urls use this :  
\<a href="{% url 'home' %}">Home\</a>

Here {% %} is the jinja syntax, using the *url* parameter we can directly link the page to home. Remember this "home" is the viewname you created in the urlpatterns template. 

We add the following at the end of the template  
{% block content %} {% endblock content %}

This part is where the extended tempaltes body beings, and the content is the name of the block.

Now you gotta update the templates home and about. Now that you have a base template, you can get rid off all the excess html stuff and add something like this.

{% extends "base.html" %}  
{% block content %}  
\<h1>Homepage\</h1>  
{% endblock content %}

Now the first part tells the template to extend from base template  
The second part is telling us where the block content begins. This is useful when you have mutliple blocks in base seperated by something, so you can extend individual blocks


## Testing

As we keep coding, we need to write test cases for each function. There are two types of testing :  
1. Unit Testing - This is to test individual pieces of code independently. 
2. Integration Testing - This is to test multiple pieces linked together.

We need to write more unit tests and less integration tests. We have a library in python called **unittest** that can handle it. It has a long list of *TestCase* instances and *assert* methods to take care of it.

Django has a seperate testing framework on top of unittest.Testcase. This includes a testclient - a dummy web browser and 4 test case classes  
1. SimpleTestCase - no database tests
2. TestCase - when database tests
3. TransactionTestCase - test database transactions
4. LiveServerTestCase - for testing with selenium

Simple Note: We have camelCase notation in django.test, not the usual python snake_case. This is becuase this unittest framework is brought from jUnit which uses camelCase and it remained the same.


In out pages app, we already have a tests.py file ready. Here is where we will add all the testcases.

The testing strategy is simple for now. We write tests for both the pages and check for their response. They should return a *200* status as sucess. That means that the pages are working fine.

class HomepageTests(SimpleTestCase):  
 def test_url_exists_at_correct_location(self):  
 response = self.client.get("/")  
 self.assertEqual(response.status_code, 200)  

To run the tests, use **python manage.py test**

We can also test the url name for each page. Use the same strategy but use a function called reverse (from django.urls) and pass the url name and see is we can fetch it. Add a new function for that test in the test class. 

We can also test if the correct template is returned for each page and the expected content is displayed. Use assertions like assertTemplateUsed and assertContains to verify them.  


### Production Deployment

To deploy our changes into production, we will use heroku. The setting up and creating everything in heroku is pretty straight forward, but we do need some extra setup from our side.  

Deployment Checklist  
1. install Gunicorn
2. create a requirements.txt file
3. update ALLOWED_HOSTS in django_project/settings.py
4. create a Procfile
5. create a runtime.txt file


Install Gunicorn with pip : **python -m pip install gunicorn==20.1.0**
Create requirements.txt : **python -m pip freeze > requirements.txt**
Add "*" in the ALLOWED_HOSTS list in django_project/settings.py. This now will allow all the domains. We learn about settin it up later.  
Create a Procfile in the base directory, next to manage.py. This procfile is specific to heroku. Add the following content to it  
**web: gunicorn django_project.wsgi --log-file -**
Create a runtime.txt to explicity set the python version there. Add this to it **python-3.10.2**

### Heroku Side changes

Here is the checklist for creating app in heroku  
1. create a new app on Heroku - **heroku create**. This also creates a remote called heroku for us. You can check this out by running **git remote -v**. Now we have a remote that we can push the changes to, similar to Git.
2. disable the collection of static files
3. push the code up to Heroku
4. start the Heroku server so the app is live
5. visit the app on Heroku’s provided URL


Will continue heroku later. Since its not a viable alternative at the moment. Shall come back to it later.


## Database Models

We need to create database models to display content from users. Djangos ORM will convert this to database. 


In the posts app, go to models.py, we have models already there from django.db. Create a new class. This will act as the database model.

class Posts(models.Model):    
    text = models.TextField()

Here, you have Posts class, and it has a field called *text*. This is a text field in the db.  

Now that there is a model created, you need to activate it. whenever you create or update an existing model, you need to update it in a two step process -  
1. create a migrations file with the *makemigrations* command. This creates a reference to any changes in the database - **python manage.py makemigrations posts**. If you dont specify appname, all the apps will be migrated.  
2. build the actual database with the migrate command - **python manage.py migrate**


### Using Django Admin Interface

To use djangos admin interface, first you need to create a superuser. To do that, use - **python manage.py createsuperuser** and set up user credentials.

By default, you will not see your app in the admin interface, it is similar to how a new app is added to INSTALLED_APPS in the main project, you also need to add the particular app to the admin interface.  

To so that, go to posts\admin.py and add the following statement to it **admin.site.register(Post)**. This is the process of registering models.  
Now, you can add new posts in the admin panel itself, but you have to notice that the name of the post is pretty autogenerated, to overcome this, you need to override some default functions and update them in the models.py file. This includes overrding functions like \_\_str\_\_, etc

Now we can setup templates. Earlier we used TemplateView to extend and create new templates, just like that there are many template instances which we can use. Now that we are moving to databases, we can use *ListView* to create a new views for database. Now you can setup *template_name* as well as *model*. 

Create templates folder and update django_project\settings.py to include that as well. 

You can use jinja templates to loop over the database data. Django provides us a context variable called <model_name>_list. That has all the data. Here the list is post_list and for each item there is a variable called text, this is printed in {{}} and the loop is taken in {%%} with a *for* and *endfor*.

Let us take a look at the testing for this. Since we have setup databases for our use, we can leverage them to create dummy data and test over it. Django provides functionality for this as well. use setUpTestData() for this and create tests over it. We can leverage this for it -

@classmethod
    def setUpTestData(cls):
        cls.post = Post.objects.create(text="This is test data")

We can also combine different unit tests into one functions, if they all check the same thing. More like a suite of tests in a single one.  


Now, we can move ahead and create a blog app, this is very similar to everything that was done, except for change in the ORM, we use a bit of more sophisticated methods to design the database, all of which can mostly be understood with some documentation.  

#### Static files 

Static files refer to js, css files that remain constant. Similar to how templates folder can be created, static folder can also be created -> either in each app, or at a project level. That is our choice.  
Create a folder named **static** next to templates.  
Add the following - *STATIC_URL = "/static/"*, *STATICFILES_DIRS = [BASE_DIR / "static"]* in django_project/settings.py
Inside the static folder, create a subfolder for css files and a new file inside css folder called base.css.  

We need to add this css file to the base.html base template, use {% load static %} at the top to use this static files and link the style sheet with *href ="{% static 'css/base.css' %}"*

Now we need to create a post_detail page, for this page, create a view, url and template. This post_detail one is the one used in db and we have a special instaince in DetailView that helps in simplifying.  So this detailview, needs a primary key to be passed, which django already creates for each db, which is a autoinceremtn number. Adding the pk path to urls.py will settle everything.  

To test for this, you can use use get_user_model and create an instance for testing user, and test the data accordingly.

If you want to create a new post, then you can use the *CreateView* model. This model is similar to the other instances, it takes another parameter called fields, which defines the names of the fields that are used in the createview instance. The rest of the procedure is the same. 

Now we can use forms as a part of our Django project, one important thing is to define the csrf_token in the form, this allows us to prevent CORS attacks on the form, the rest is the same. The form method is *POST*. If you wanna send data, then you use post, if you wanna get data, for example in a search engine, then you use the get command. 


Just like we have a createview, we also have an UpdateView template, this template allows us to update the given post, that can help us to edit it. 