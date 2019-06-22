### Exploring the project

We can see there are a few folders that we created beforehand for you:

    - `accounts` will handle the user login / register / password reset functionalities
    - `main` will handle the renter models and views to display/add/delete/list renters
    - `transactions` will handle the payment gateway that we use in this case Tap

And in some of these applications, we provided template folder.

### Setting up the URL's

Before we start I’ll need you to replace everything in the `urls.py` file inside `myrent` folder with the following.

```python
from django.contrib import admin
from django.urls import path, include
from django.conf.urls.static import static
from django.conf import settings
from django.shortcuts import redirect
urlpatterns = [
   path('admin/', admin.site.urls),
   path('accounts/', include('accounts.urls')),
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

We used a function called `include` to indicate that the specific url will refer to an application and in this case whenever someone visits /accounts/ he will be redirected to the urls residing in the file that we will create right now.

### Creating new URL`s

Let’s go to the `accounts` application, first create a new file and name it `urls.py` .

Django provides us with lots of functions out of the box like a complete user model and all of its functionalities so we will be using most of these

Lets type the following in the newly created `urls.py` .

```python
from django.contrib.auth import views
from django.urls import path

urlpatterns = [
        path('login/', views.LoginView.as_view(template_name='registration/login.html'), name='login'),
        path('logout/', views.LogoutView.as_view(template_name='registration/logout.html'), name='logout'),
        path('password-reset/', views.PasswordResetView.as_view(template_name='registration/reset_form.html'), name='password_reset'),
        path('password-reset/done/', views.PasswordResetDoneView.as_view(template_name='registration/reset_done.html'), name='password_reset_done'),
        path('password-change/', views.PasswordChangeView.as_view(template_name='registration/change_form.html'), name='password_change'),
        path('password-change/done/', views.PasswordChangeDoneView.as_view(template_name='registration/change_done.html'), name='password_change_done'),
]
```

You can see that most of the normal functions that are required to create a user / reset the password / change the password are provided here but we are missing the signup function in that list of functions so we will have to create one on our own.

You can see we used `as_view` and that is a special case for something called `class based views`, you can take a look here https://docs.djangoproject.com/en/2.2/topics/class-based-views/

And in this case this class based view takes a specific template to render, which is `template_name`

### Importing in the Views

Navigate to `accounts/views.py` .
And let’s start by importing the stuff that we will need

```python
from django.contrib.auth import login, authenticate
from django.contrib.auth.forms import UserCreationForm
from django.shortcuts import render, redirect
```

We import `login` and `authenticate`

- `Login` is a function that takes two parameters, a request and a user object, it will then add cookies to that specific request owner to indicate that he is now logged in as the user object provided
- `Authenticate` is a function that takes to parameters, a username and a password. Since django encrypts passwords by deafault when signing up, it will be impossible for us to know whether the user is inputing the correct password. So, django provided us with this function which takes the password provided as a parameter and encrypt it to compare it with the encrypted password as the one in the user object that has the username that matches the username provided as the first parameter for the function. Then, It will return the user object, if not it will return None.

We imported something called `UserCreationForm` which is also provided by django. It's a form with three fields, a username, a password1, and a password2. The username is self-explanatory, the password1 and password2 fields are both the password and confirm password fields as a form of security to check if they both match before submittig it as the final password.

Then we imported `render` and `redirect`:

- `Render` will accept 3 parameters, a request object, a html template and a dictionary, the dictionary can be empty or filled with data depending on the need of the function
- `Redirect` will take either a url name or a url path for example `/` and sends the user to that page/url path

### Creating signup view

Let’s type in this function underneath the imports:

```python
def signup(request):
   if request.method == 'POST':
       form = UserCreationForm(request.POST)
       if form.is_valid():
           form.save()
           username = form.cleaned_data.get('username')
           raw_password = form.cleaned_data.get('password1')
           user = authenticate(username=username, password=raw_password)
           login(request, user)
           return redirect('/')
   else:
       form = UserCreationForm()
   return render(request, 'registration/signup.html', {'form': form})
```

Let's explain this function one line at a time:

```python
def signup(request):
```

We defined a function called signup and it takes one parameter which is request

```python
   if request.method == 'POST':
```

Then we check the request object for its method, if the method is a POST method it will initiate the following

```python
       form = UserCreationForm(request.POST)
```

Using the variable `form` it will fill the UserCreationForm with data from the request.POST which holds the username, password that are filled in the form in the template

```python
       if form.is_valid():
```

It will check if that form is valid or not, depending on that it will do the following, being valid is matching the structure that we set in the form `UserCreationForm` in this case matching the model structure

```python
           form.save()
```

We will take that object from the form if it was valid and save it as an actual object in the database as a User object

```python
           username = form.cleaned_data.get('username')
           raw_password = form.cleaned_data.get('password1')
```

We will get both the username and the password from the request object and save them into those two variables `username` and `raw_password`

```python
           user = authenticate(username=username, password=raw_password)
```

This will use the authenticate function and if the username/password are correct it will return the user object

```python
           login(request, user)
```

We will use the login function providing it the request and the user object from the authenticate function

```python
           return redirect('/')
```

After that it will redirect the user to the home page

```python
   else:
       form = UserCreationForm()
   return render(request, 'registration/signup.html', {'form': form})
```

But if the request is not a POST request, it will create an empty form and save it to a variable called form.

Then it will render the signup.html page and provide it the form as a context to the html page so the django templating system can use it

The templates for the accounts application should be premade for you in the `accounts/templates/registration/` folder while the `base.html` and the `404.html` pages are at`accounts/templates/` folder
