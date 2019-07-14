### Creating a Form

Now let’s create the form to add Renters, so in the `main` folder create a file and name it `forms.py` with the following information

```python
from django import forms
from main.models import Renter

class RenterForm(forms.ModelForm):

   class Meta:
       model = Renter
       exclude = ('landlord',)
```

In this case we imported the model that we would like to create a form for and then we created a class and named it `RenterForm` which inherits from `forms.ModelForm`

And if you notice we have something called `class Meta:` that holds two variables, the `model` that we want to extend as the data source for our form and an `exclude` variable to name the specific variables that we don’t want the user to have control over in this example its `landlord` and two other fields that we are yet to add into the model

### Creating a View

Next we will go to the `views.py` file in the `main` folder, and in there we need to make the first function to create a renter.

Let’s type the following code and explain it along the way.

```python
from main.forms import RenterForm
from django.shortcuts import render,redirect
def newRenter(request):
   if not request.user.is_authenticated:
       return redirect('/')
   if request.method == "POST":
       form = RenterForm(request.POST)
       if form.is_valid():
           post = form.save(commit=False)
           post.landlord = request.user
           post.save()
           return redirect('/')
   else:
       form = RenterForm()
   return render(request, 'renter/add.html', {'form': form})
```

We defined a function and named it `newRenter` and then we added a very important if statement which checks if the user is authenticated or not, as we only allow authenticated users to create renter’s

```python
   if not request.user.is_authenticated:
       return redirect('/')
```

Then we check if the request method is `POST`. If it was `POST`, then it will create a variable and name it `form` and use a form called `RenterForm` to fill it with whatever data the `request.POST` had. Afterwards, it would check if the information provided to the form was valid, and what we mean by valid here is that whatever necessary fields that the form requires are provided and are in the correct syntax so the model can fill the information and save it in the database

```python
       if form.is_valid():
```

And if the form was valid we will start creating an object from the data provided, but because we need to relate a User to whatever `Renter` object that we create we have to save the object in a different way like so

```python
           post = form.save(commit=False)
           post.landlord = request.user
           post.save()
          return redirect('/')
```

Since we checked that the user was actually authenticated we can then use `commit=False` to pause the actual saving of the object to the database and create a dummy object with the information that it has and saved that dummy object in a variable called `post`. After that, we can assign it a `landlord` which is the `request.user` and finally save the object to be sent to the database and afterwards redirect the user to the main page

```python
   else:
       form = RenterForm()
   return render(request, 'renter/add.html', {'form': form})
```

But in the case that the request isn’t `POST` then it will generate an empty form and send it as context to the user when it directs him to the add renter template page.

We will be creating this template in the next step so hold on.

### Creating a template

Now go to `main` and create a folder and call it `templates` then inside of that newly created folder create a folder and call it `renter` inside of it create a new file and name it `add.html` .

It should look like this `main/templates/renter/add.html`.

and type the following inside the file.

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<div class="col-12">

   <div class="card">
       <div class="card-body">
           <h4 class="card-title">Add a Renter</h4>

           <form method="post">
               {% csrf_token %} {{ form | crispy }}

               <button type="submit" class="btn btn-default">Add</button>
           </form>
       </div>
   </div>
</div>
{% endblock %}
```

Its a simple django template that uses the form that we sent through the context, then we used crispy forms to make the form a little bit prettier all while extending from the base.html file at the block main location

Now we have successfully created a function that can create Renter objects and relate them to the current logged in user, we can now start listing the renters that the current user “owns”

### Url

Let's go to the `main` folder and create a new file and name it `urls.py`

And inside lets type the following

```python
from django.urls import path
from main.views import *


urlpatterns = [
   path('add/', newRenter, name='add'),
]
```

In the 2nd import we are getting all of the functions that the `views.py` have and then in the `urlpatterns` we are creating urls for each one of them

Each one has a url name , a function and a name to identify the url

### Including to the original urls.py

Now we need to import this `urls.py` file and use it, and to do so let’s get back to the `urls.py` file in the `myrent` folder

And type the following code in

```python
from django.contrib import admin
from django.urls import path, include
from django.conf.urls.static import static
from django.conf import settings
from django.shortcuts import redirect
from main import views
urlpatterns = [
   path('admin/', admin.site.urls),
   path('accounts/', include('accounts.urls')),
   path('', include('main.urls')),
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

We “included” the urls in the `main/urls.py`
