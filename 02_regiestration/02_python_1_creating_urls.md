
### Trello

Go to `Trello` board and move the card `As a user I can login` and `As a user I can logout` to the `Doing` list


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


### Trello & Git Workflow

With this done we can go to our `Trello` board and move the card `As a user I can login` and `As a user I can logout` to the `Review` list

Open the terminal in this current project and type the following

```bash
git add .
```

This will add the changes we made to the github repo

```bash
git commit -m "As a user I can login and logout"
```

