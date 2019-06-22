### Collected View

Now let’s go back to the `transactions/views.py` to finish the last view we have there

As we said, this last view will grab everyone who paid, show their transactions and their total

Let’s type the following

```python
def collected(request):
    user = request.user
    transactions = Transaction.objects.filter(renter__landlord=user,date__gte=timezone.now().replace(day=1, hour=0, minute=0, second=0, microsecond=0)).filter(status="CAPTURED")
    total = 0
    for transaction in transactions:
        total = total + float(transaction.amount)
    context = {
        'total': total,
        'transactions': transactions
    }
    return render(request, 'transactions/collected.html', context)
```

In this view we take the user who initiated the request, then we use this query

```python
    Transaction.objects.filter(renter__landlord=user,date__gte=timezone.now().replace(day=1, hour=0, minute=0, second=0, microsecond=0)).filter(status="CAPTURED")
```

We take all the transactions, then grab the ones where the landlord is the person who initiated this request. After that, we check for the date if it was within the beginning of this month. Finally, we check if the status on this transactions is “CAPTURED” which is the default for a successful payment using Tap's gateway.

We set a variable called total to be = 0

And for every transaction we increase the total by the amount that the transaction was paid, but we have to turn it into a float first because of change that might occur, for example 200.500KD etc

We take both the transactions and total and we send them to the html template `transactions/collected.html`

### Creating the template

So, let’s create this template, go to `transactions/templates/transactions/` and create the file `collected.html` and inside it type the following

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<section data-section-id="2" data-component-id="15_05_awz" data-category="admin" class="py-4">
               <div class="container-fluid">
                 <h2 class="mb-4" data-config-id="header">Collection</h2>
                 <h3 class="mb-4" >Total: {{total}} K.D</h3>
                 <div class="table-responsive">
                   <table class="table">
                     <thead>
                       <tr>
                         <th scope="col">Id #</th>
                         <th scope="col">Name</th>
                         <th scope="col">Phone Number</th>
                         <th scope="col">Rent</th>
                         <th class="text-center" scope="col">Actions</th>
                       </tr>
                     </thead>
                     <tbody data-config-id="table_05">
                       {% for transaction in transactions %}
                       <tr>
                         <td>#{{transaction.renter.id}}</td>
                         <td>{{transaction.renter.name}}</td>
                         <td>{{transaction.renter.phone}}</td>
                         <td>{{transaction.renter.amount}}</td>
                         <td class="text-center">
                                       <a  href={% url 'edit' id=transaction.renter.id %} class="btn btn-primary"><i class="fa fa-edit"></i></a>
                                       <a  href={% url 'details' id=transaction.renter.id %} class="btn btn-secondary"><i class="fa fa-info-circle"></i></a>
                                       <a  href={% url 'delete' id=transaction.renter.id %} class="btn btn-danger"><i class="fa fa-trash"></i></a>
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

After that, let’s create URL’s for the views that we created. Create a new file and name it `urls.py` in the `transactions` folder

And type the following

```python
from django.urls import path
from transactions.views import *
urlpatterns = [
   path('pay/<str:token>/', pay, name='pay'),
   path('response/', responsePage, name='response'),
   path('transactions/<int:id>/', transactionList, name='transactions'),
   path('newTransactions/', newTransactions, name='newTransactions'),
   path('failToPay/', failToPay, name="failToPay"),
   path('collected/', collected, name="collected")
]
```
