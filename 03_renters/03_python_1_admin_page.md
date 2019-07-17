### Adding Renter to the admin page

Now that is done let’s go in the same folder to the `admin.py` file, we will import the `Renter` model and register it to be displayed in the Django Admin Interface

Let’s replace the existing code with the following

```python
from django.contrib import admin
from main.models import Renter

admin.site.register(Renter)
```
