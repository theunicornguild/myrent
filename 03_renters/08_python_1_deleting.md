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
