### Listing Renters

So in the `views.py` file in the `main` folder lets create the following function and explain how it works

```python
def listRenters(request):
   user = request.user
   if not user.is_authenticated:
       return redirect('login')
   user = request.user
   renters = Renter.objects.filter(landlord=user)
   context = {
       'renters': renters
   }
   return render(request, 'renter/list.html', context)
```

I will skip the parts that we previously explained up to this point so if you need to you can check the previous functions for more information

In here we defined two variables that we will use to help us list all users that we own, first a `user` variable and a `renters` variable, the `user` variable will contain the current logged in user which is `request.user` while renters will go thru all the `Renter` objects and find the ones that belong to this current user and save that `QuerySet` to the `renters` variable

- A QuerySet is django’s way of grouping in a list of objects that it grabbed from the database and it doesn’t have the same methods that a normal array have so dealing with it can be a little bit different.

Because we are using the `Renter` model we have to import it. So, at the top of the `views.py` file import `Renter` like the following

```python
from main.models import Renter
```

# Creating a list template

Then lets create a new file in the `main/templates/renter` folder and name it `list.html`

Inside that file lets type the following

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<section data-section-id="2" data-component-id="15_05_awz" data-category="admin" class="py-4">
               <div class="container-fluid">
                 <h2 class="mb-4" data-config-id="header">Renters</h2>
                 <div class="table-responsive">
                   <table class="table">
                     <thead>
                       <tr>
                         <th scope="col">Id #</th>
                         <th scope="col">Name</th>
                         <th scope="col">Phone Number</th>
                         <th scope="col">Rent</th>
                         <th class="text-center" scope="col">Status</th>
                         <th class="text-center" scope="col">Actions</th>
                       </tr>
                     </thead>
                     <tbody data-config-id="table_05">
                       {% for renter in renters %}
                       <tr>
                         <td>#{{renter.id}}</td>
                         <td>{{renter.name}}</td>
                         <td>{{renter.phone}}</td>
                         <td>{{renter.amount}}</td>
                         <td class="text-center">{% if last_paid == renter.last_paid %}<span class="badge badge-success">Paid</span>{% else %}<span class="badge badge-danger">UnPaid</span>{% endif %}</td>
                         <td class="text-center">
                                       <a  href={% url 'edit' id=renter.id %} class="btn btn-primary"><i class="fa fa-edit"></i></a>
                                       <a  href={% url 'details' id=renter.id %} class="btn btn-secondary"><i class="fa fa-info-circle"></i></a>
                                       <a  href={% url 'delete' id=renter.id %} class="btn btn-danger"><i class="fa fa-trash"></i></a>
                                       {% if last_paid != renter.last_paid %}
                                       <a  href={% url 'sendPayment' id=renter.id %} class="btn btn-warning"><i class="fa fa-envelope"></i></a>
                                       {% endif %}

                         </td>
                         {% endfor %}
                       </tr>
                     </tbody>
                   </table>
                 </div>
               </div>
             </section>

{% endblock %}

```

You will notice we have various urls in this page that won’t work and will probably cause errors if you decided to run the application at this point so if you want to run it at this point you need to comment out or remove this portion of the code

```html
                         <td class="text-center">
                              {% if last_paid == renter.last_paid %}
                                <span class="badge badge-success">Paid</span>
                              {% else %}
                                <span class="badge badge-danger">UnPaid</span>
                              {% endif %}
                         </td>
                         <td class="text-center">
                               <a  href={% url 'edit' id=renter.id %} class="btn btn-primary"><i class="fa fa-edit"></i></a>
                               <a  href={% url 'details' id=renter.id %} class="btn btn-secondary"><i class="fa fa-info-circle"></i></a>
                               <a  href={% url 'delete' id=renter.id %} class="btn btn-danger"><i class="fa fa-trash"></i></a>
                               {% if last_paid != renter.last_paid %}
                                  <a  href={% url 'sendPayment' id=renter.id %} class="btn btn-warning"><i class="fa fa-envelope"></i></a>
                               {% endif %}
                         </td>
```

But the main thing about this page is displaying the contents of each renter and adding some actions to allow the landlord to interact with each renter quickly

### Urls

Let's go to the `main` folder and edit `urls.py`

And inside lets add the following code to the `urlpatterns`

```python
urlpatterns = [
   path('add/', newRenter, name='add'),
   path('list/', listRenters, name='list'),
]
```

And to make our list page the default page that the application goes to we will do the following

let’s get back to the `urls.py` file in the `myrent` folder

And type the following code in

```python
  urlpatterns = [
    ...
    path('', views.listRenters)
  ]
```

And make sure this `path` is the last one in the urlpatterns so it matches with any request that the user makes `THIS IS VERY IMPORTANT`
