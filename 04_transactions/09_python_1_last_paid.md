### last_paid field

go to `main/..models.py` and let’s edit the `renter` model

Let’s add a new field to the model and let’s have it look like this

```python
    last_paid = models.CharField(max_length=250,null=True,blank=True)
```

Which is a normal character field which is null by default

Then let’s add a method to the renter model that looks like this

```python
   def paid(self):
       now = datetime.now()
       year, month, *args = now.timetuple()
       self.last_paid = "{0}-{1}".format(year,month)
       self.save()
```

This will take the self(the current object that we are dealing with from this model so any renter) and will take the current time and format it in a way similar to the one we have in the previous view save it in the `last_paid` field

Make sure we have the following imports at the top of the `main/models.py` file

```python
from django.db import models
from django_countries.fields import CountryField
from django.contrib.auth.models import User
from uuid import uuid4
import plivo
from datetime import datetime
```
