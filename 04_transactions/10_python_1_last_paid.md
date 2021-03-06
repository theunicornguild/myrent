### Trello

With this done we can go to our `Trello` board and move the card `As a user I can view newest successful transactions` to the `Doing` list

### last_paid field

go to `main/..models.py` and let’s edit the `renter` model

Let’s add a new field to the model and have it look like this

```python
    last_paid = models.CharField(max_length=250,null=True,blank=True)
```

Which is a normal character field is null by default

Then, let’s add a method to the renter model that looks like this

```python
   def paid(self):
       now = datetime.now()
       year, month, *args = now.timetuple()
       self.last_paid = "{0}-{1}".format(year,month)
       self.save()
```

This will take the self(the current object that we are dealing with from this model so any renter) and will take the current time and format it in a way similar to the one we have in the previous view and save it in the `last_paid` field.

Make sure we have the following imports at the top of the `main/models.py` file

```python
from django.db import models
from django_countries.fields import CountryField
from django.contrib.auth.models import User
from uuid import uuid4
import plivo
from datetime import datetime
```


### Migration

Now we can go to the terminal, make sure we are in the main folder of the project and both submit and make sure that the changes we did to the model can go through without issues to the database

Type the following

```bash
python manage.py makemigrations
python manage.py migrate
```

### Fixing the Form

After we added two new more fields we need to exlude them from the Renter form

So go to the `main/forms.py` file and add the following to the `exclude` variable

```python
       exclude = ('landlord','token','last_paid')
```
