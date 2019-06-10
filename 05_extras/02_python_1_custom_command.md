### Creating a custom command

And now we will create a custom manage.py function! This function will be run automatically at the start of every month and to do that let’s go to the main folder and then create another folder inside of it and call it `management` and inside the management folder create another folder and name it `commands`, then inside the commands folder create a file and name it `smser.py`

The final path should look like this `main/management/commands/smser.py`

Type the following inside that newly created file

```python
from django.core.management.base import BaseCommand, CommandError
from main.models import Renter

class Command(BaseCommand):
   help = 'Sends SMS messages to all Renters'

   def handle(self, *args, **kwargs):
       renters = Renter.objects.all()
       for renter in renters:
           renter.sms()
       self.stdout.write(self.style.SUCCESS('Successfully sent SMS to : %s renters') % len(renters))
```

This function when you start it using `python manage.py smser` will grab all the renter objects and for each one of them it will start the .sms() method on them to automate sending SMS messages
