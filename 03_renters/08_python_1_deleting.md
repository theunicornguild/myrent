### Creating Delete View

Now we finished the detail page for the renters and we can continue on to create a page that deletes a `renter` object

Lets type the following code

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

To delete the renter object and then redirect the user to the list
