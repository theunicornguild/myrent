### Trello

With this done we can go to our `Trello` board and move the card `As a user I can check renters who did not pay` to the `Doing` list

### Failed To Pay View

Next is the `renters` who failed to pay this month

Let’s type the following code in `transactions/views.py`

```python
def failToPay(request):
   user = request.user
   now = datetime.now()
   year, month, *args = now.timetuple()
   last_paid = "{0}-{1}".format(year,month)
   renters = Renter.objects.filter(landlord=user).exclude(last_paid=last_paid)
   context = {
       'renters': renters
   }
   return render(request, 'transactions/failToPay.html', context)
```

This will function kinda like the previous function but will only target the ones who failed to pay.

So, we get the user who initiated the request, and we get the current time.

From the time we get the year and the month and we set those to a variable called last_paid we then go to all the renters and issue this query

```python
   renters = Renter.objects.filter(landlord=user).exclude(last_paid=last_paid)
```

In here we say, give us all the renters that belong to this `landlord(this user)` but exclude the ones who has the `last_paid` field that equals the `last_paid` that we created which is this year/month combo for example `2019-05`.

Then, we send that to the context of this html page

```python
   return render(request, 'transactions/failToPay.html', context)
```

### Failed to Template

Next, let's create this template. Go to `transactions/templates/transactions` and create a new file and name it `failToPay.html`
And type the following inside

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<section data-section-id="2" data-component-id="15_05_awz" data-category="admin" class="py-4">
               <div class="container-fluid">
                 <h2 class="mb-4" data-config-id="header">Failed to pay</h2>
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
                         <td class="text-center"><span class="badge badge-danger">UnPaid</span></td>
                         <td class="text-center">
                                       <a  href={% url 'edit' id=renter.id %} class="btn btn-primary"><i class="fa fa-edit"></i></a>
                                       <a  href={% url 'details' id=renter.id %} class="btn btn-secondary"><i class="fa fa-info-circle"></i></a>
                                       <a  href={% url 'delete' id=renter.id %} class="btn btn-danger"><i class="fa fa-trash"></i></a>
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

### Creating a URL

After that, let's go to `urls.py` in the `transactions` folder

And add the following to the `urlpatterns`

```python
   path('failToPay/', failToPay, name="failToPay"),
```

it should look like this

```python
from django.urls import path
from transactions.views import *
urlpatterns = [
   path('pay/<str:token>/', pay, name='pay'),
   path('response/', responsePage, name='response'),
   path('transactions/<int:id>/', transactionList, name='transactions'),
   path('newTransactions/', newTransactions, name='newTransactions'),
   path('failToPay/', failToPay, name="failToPay"),

]
```

But you might be wondering… we don’t have a `last_paid` field on the renter model, well you are right and we are going to create it right now !

### Trello & Git Workflow

With this done we can go to our `Trello` board and move the card `As a user I can check renters who did not pay` to the `Review` list

Open the terminal in this current project and type the following

```bash
git add .
git commit -m "As a user I can check renters who did not pay"
```