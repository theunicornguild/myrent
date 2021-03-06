
### Trello

With this done we can go to our `Trello` board and move the card `Pay functionality` to the `Doing` list


### Creating Payment View

Let’s start doing that! Go to `transactions/views.py` and being typing

```python
def pay(request,token):
   renter = get_object_or_404(Renter, token=token)
   url = request.build_absolute_uri('/')[:-1].strip("/") + reverse('response')
   response = tap(renter,url)
   paymentUrl = response['transaction']['url']
   return redirect(paymentUrl)
```

This will be our first view and we will explain what’s going on here

This view takes a `token` as a parameter

```python
def pay(request,token):
```

And then we will try to get a specific renter using that token

```python
   renter = get_object_or_404(Renter, token=token)
```

After that it will generate a url to send to the tap function along with the renter, but the tap function is something we haven’t done yet,
the tap function will take a Renter object and a url and it will send an API request to Tap's API and generate a url that we can send to our customers to pay through.

If you notice we created the url using this function `request.build_absolute_uri()` and if you want to learn more about it you can check out the Django documentation right here https://docs.djangoproject.com/en/2.2/ref/request-response/

But a rough explanination of this function is that it takes the url that the request has for example, if your accessing this view using this url `http://127.0.0.1:8000/url` it will return that same url, but if you access it using this url `http://localhost:8000/url` it will also return the `localhost` instead of the IP address depending on how the user is accessing the url, and after that we strip the url so it removes everything after the first backslash `/` and give us a url that we can add stuff to for example `http://localhost:8000`

```python
   url = request.build_absolute_uri('/')[:-1].strip("/") + reverse('response')
   response = tap(renter,url)
```

And `reverse` is a function that takes a url name and generates the url that has that name

After the tap function returns a response we will get a specific url and we need to redirect the user to that url. This might seem too much by now and a bit confusing but trust me we will go through everything

Common questions would be, What is a token ? Where did the tap function come from?

And we will handle them one by one right after you import the required stuff at the top of your `views.py` file in the `transactions` folder

```python
from django.shortcuts import render,redirect,get_object_or_404
from main.models import Renter
from django.urls import reverse
from transactions.models import Transaction
import json
import requests
from django.utils import timezone
from django.db.models import Q
from datetime import datetime
```

So the first question was what is this token ? and how is it related to a user… well, we are going to create a new field in the renter model and call it token, and token is going to be a unique identifier to a specific user just like an ID but it changes whenever you finish a payment. That way whenever we want the user to pay his rent we can give him a special url with his token. He can use that url to pay only once, both to stop him from duplicating transactions and to make it more secure so no one can enumerate(guess) user id’s and fake-pay using those to see rent amount or other sensitive information.

### Creating a URL

After that, let’s create URL’s for the views that we created. Create a new file and name it `urls.py` in the `transactions` folder

And type the following

```python
from django.urls import path
from transactions.views import *
urlpatterns = [
   path('pay/<str:token>/', pay, name='pay'),
]
```
