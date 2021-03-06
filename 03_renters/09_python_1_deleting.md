
### Trello

With this done we can go to our `Trello` board and move the card `As a user I can delete renter` to the `Doing` list


### Creating Delete View

Now we finished the detail page for the renters and we can continue on to create a page that deletes a `Renter` object

Lets type the following code in the `views.py` file in the `main` folder.

```python
def deleteRenter(request,id):
   renter = Renter.objects.get(id=id)
   if request.user != renter.landlord:
       return redirect('list')
   renter.delete()
   return redirect('list')
```

In here we grabbed the `renter` object that is required, we checked that the user owns the current renter and then we issues a command

```python
   renter.delete()
```

This command deleted the renter object and then redirects the user to the list

### Urls

Let's go to the `main` folder and create a new file and name it `urls.py`

And inside lets add the following code to the `urlpatterns`

```python
urlpatterns = [
   path('add/', newRenter, name='add'),
   path('list/', listRenters, name='list'),
   path('details/<int:id>/', detailRenter, name='details'),
   path('delete/<int:id>/', deleteRenter, name='delete'),
]
```

### Trello & Git Workflow

With this done we can go to our `Trello` board and move the card `As a user I can delete renter` to the `Review` list

Open the terminal in this current project and type the following

```bash
git add .
git commit -m "As a user I can delete a renter"
```