### Creating a superuser

Let’s create a superuser and run the server by typing

```bash
python manage.py createsuperuser
<fill the information>
python manage.py runserver
```

Navigate to http://127.0.0.1:8000/admin , And after logging in you should be able to see a Renter section that you can view/list/add/delete renters from.

But that won’t be useful for us as we want normal users to add their own renters and list them/ manipulate their data, So next we will be created a complete CRUD (Create/Read/Update/Delete) view’s to do all of that functionality from a normal user side
