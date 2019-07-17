### Trello

With this done we can go to our `Trello` board and move the card `Create a custom django command to send renters their sms` to the `Doing` list

### Creating a custom command

And now we will create a custom manage.py function! This function will be run automatically at the start of every month. To do that let’s go to the main folder and then create another folder inside of it and call it `management` and inside the `management` folder create another folder and name it `commands`. Then, inside the commands folder create a file and name it `smser.py`.

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

When you start it using `python manage.py smser`, this function will grab all the renter objects and for each one of them it will call the `.sms()` method on them to automate sending the SMS messeges.

### Trello & Git Workflow

With this done we can go to our `Trello` board and move the card `Create a custom django command to send renters their sms` to the `Review` list

Open the terminal in this current project and type the following

```bash
git add .
git commit -m "Django custom command"
```