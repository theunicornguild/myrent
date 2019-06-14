### Creating Editing view

Last but not least is the edit renter page, this will be very similar to both the details view and the create view, we will see why now

```python
def editRenter(request,id):
   renter = get_object_or_404(Renter, id=id)
   if request.user != renter.landlord:
       return redirect('list')
   if request.method == "POST":
       form = RenterForm(request.POST,instance=renter)
       if form.is_valid():
           renter = form.save(commit=False)
           renter.landlord = request.user
           renter.save()
           return redirect('details', id=renter.id)
   else:
       form = RenterForm(instance=renter)
   return render(request, 'renter/edit.html', {'form': form})
```

We defined the function `editRenter` so that it takes an `id` as an argument. Then defined a variable `renter` which uses a function that we'll import shortly called `get_object_or_404()`. The main functionality of this function is to try and get an object from a specific model with a specific key/value pair and if it fails to get that specific item, it will just send the user to a 404 page (page not found).

In this case we provided the `Renter` model and for the key/value pair we need to match the id of the object with the id that the user provides so its gonna be `id=id`

After that we will check if the renter belongs to the user requesting to edit with the following code

```python
   if request.user != renter.landlord:
       return redirect('list')
```

The next part will look very similar to the create function as we will check if the request is a `POST` or not and depending on that the functionality will change

```python
   if request.method == "POST":
       form = RenterForm(request.POST,instance=renter)
       if form.is_valid():
           renter = form.save(commit=False)
           renter.landlord = request.user
           renter.save()
           return redirect('details', id=renter.id)
```

Now if the request was a `POST` it will fill the `RenterForm` with two things, the data that the user provided(which is whatever he edited from the user) and the instance that we want to edit (the renter that we got using the id), we check if the form is valid or not and if it was valid it will save the changes and redirect the user to the details page for the user he just edited

```python
   else:
       form = RenterForm(instance=renter)
   return render(request, 'renter/edit.html', {'form': form})
```

But if the request wasn’t a `POST` request, it will still take the id and get the object, fill the form with the object that we want to edit and send it to the `edit.html` template so the user can view the object before changing it

Finally, we need to import the `get_object_or_404()` function, so at the top of the `main/views.py` where we have the imports change the following import

```python
from django.shortcuts import render,redirect
```

To be

```python
from django.shortcuts import render,redirect,get_object_or_404
```

# Creating Edit Template

And let’s create the edit template now so go to `main/templates/renter` and create `edit.html`
And type the following

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<div class="col-12">

   <div class="card">
       <div class="card-body">
           <h4 class="card-title">Edit Renter</h4>

           <form method="post">
               {% csrf_token %} {{ form | crispy }}

               <button type="submit" class="btn btn-default">Edit</button>
           </form>
       </div>
   </div>
</div>
{% endblock %}
```
