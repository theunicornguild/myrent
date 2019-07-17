### First Migration

Now we can go to the terminal, make sure we are in the main folder of the project and both submit and make sure that the changes we did to the model can go through without issues to the database

Type the following

```bash
python manage.py makemigrations
python manage.py migrate
```

The first command will make the changes into a python file in the specific app’s `migrations` folder
The second command will take those changes and submits them into the database
