### URL's

You may have noticed that we generated lots of functionality but we have no way of accessing all of this functions… because we still haven’t linked them to the application via url’s

Lets go to the `main` folder and create a new file and name it `urls.py`

And inside lets type the following

```python
from django.urls import path
from main.views import *


urlpatterns = [
   path('add/', newRenter, name='add'),
   path('list/', listRenters, name='list'),
   path('details/<int:id>/', detailRenter, name='details'),
   path('delete/<int:id>/', deleteRenter, name='delete'),
   path('edit/<int:id>/', editRenter, name='edit'),
]
```

In the 2nd import we are getting all of the functions that the `views.py` have and then in the `urlpatterns` we are creating urls for each of them

Each one has a url name , a function and a name to identify the url

The only major difference is when we get to the last 3 urls as we made them dynamic and can accept a variable which is an integer called `id`

```python
   path('details/<int:id>/', detailRenter, name='details'),
   path('delete/<int:id>/', deleteRenter, name='delete'),
   path('edit/<int:id>/', editRenter, name='edit'),
```

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
   path('', views.listRenters)
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

We “included” the urls in the `main/urls.py` and we have set a default url that any url will automaticly go to by typing the following

```python
   path('', views.listRenters)
```

So now the `listRenters` will be the default page that any user will go to automatically.
