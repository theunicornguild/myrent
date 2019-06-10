### Creating Details View

Since we finished displaying all of the `renters` now we need to create a details page to show all of the information of a selected renter as the list will only show small amounts of data

Lets type the following code

```python
def detailRenter(request,id):
   renter = Renter.objects.get(id=id)
   if request.user != renter.landlord:
       return redirect('list')
   return render(request,'renter/details.html', {'renter': renter})
```

Notice that the `detailRenter` function takes more than just the request this time, it takes the `id` of the renter object that we want to pull from the database.

```python
def detailRenter(request,id):
```

And next it will pull the renter object from the database by using the Renter model

```python
   renter = Renter.objects.get(id=id)
```

We specified that we need an object which have the same id as the one we provided
A note, the first id is the name of the field while the 2nd id is the variable name that we have

Next we don’t need to check if the user is authenticated but we check if the user that request the detail of the renter is the same user that created him

```python
   if request.user != renter.landlord:
       return redirect('list')
```

And if he isn’t it will redirect him back the list page

Last we render the details.html template page with the renter that we got

### Creating details template

So let’s create that page at `main/templates/renter/` and name it `details.html`
And type the following code in

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<div class="col-12">
 <div class="card">
   <div class="card-body">
     <h4 class="card-title">Renter Information</h4>
     <ul class="list-group">
         <li class="list-group-item">
       <a href={% url 'transactions' id=renter.id %} class="btn btn-warning">Transaction List</a>
       <a href={% url 'edit' id=renter.id %} class="btn btn-primary">Edit</a>
       <a href={% url 'delete' id=renter.id %} class="btn btn-danger">Delete</a>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Id: <span class="text-secondary">{{ renter.id }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Phone #: <span class="text-secondary">{{ renter.phone }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Name: <span class="text-secondary">{{ renter.name }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Rent: <span class="text-secondary">{{ renter.amount }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Address Name:
           <span class="text-secondary">{{ renter.adressName }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Address line 1:
           <span class="text-secondary">{{ renter.address1 }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Address line 2:
           <span class="text-secondary">{{ renter.address2 }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           ZIP / Postal Code:
           <span class="text-secondary">{{ renter.zip_code }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           City: <span class="text-secondary">{{ renter.city }}</span>
         </p>
       </li>
       <li class="list-group-item">
         <p class="text-primary">
           Country: <span class="text-secondary">{{ renter.country }}</span>
         </p>
       </li>
     </ul>
   </div>
 </div>
</div>
{% endblock %}
```
