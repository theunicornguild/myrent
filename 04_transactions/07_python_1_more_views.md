### Transaction List
#how is this our first view?
Let’s go to `transactions/views.py` and create our first view,
#write which view this is

```python
def transactionList(request,id):
   renter = get_object_or_404(Renter, id=id)
   if request.user != renter.landlord:
       return redirect('list')
   transactions = Transaction.objects.filter(renter=renter)
   context = {
       'transactions': transactions
   }
   return render(request,'transactions/list.html', context)
```

This view will accept two things, the `request` and the `id` of a speicifc renter, it will try to get the renter or else it will redirect to a 404 page

Next, it will check if the user who requested that specific renter is the `landlord` who created the `renter` or not. If he's not, he will be redirected to the list.

If he was their renter we will get all transactions made by that user using the following query

```python
   transactions = Transaction.objects.filter(renter=renter)
```

Then we will attach it to the context and render the following page ‘transactions/list.html’

We’ve already imported everything at the start of making this view so everything should work smoothly but we haven’t created the `list.html` page yet

### Creating a template

So go to `transactions/templates/transactions/list.html` and type the following

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<section data-section-id="2" data-component-id="15_05_awz" data-category="admin" class="py-4">
               <div class="container-fluid">
                 <h2 class="mb-4" data-config-id="header">Transactions</h2>
                 <div class="table-responsive">
                   <table class="table">
                     <thead>
                       <tr>
                         <th scope="col">Status</th>
                         <th scope="col">Name</th>
                         <th scope="col">Phone Number</th>
                         <th scope="col">Rent</th>
                         <th class="text-center" scope="col">Date</th>
                         <th class="text-center" scope="col">Tap ID</th>
                       </tr>
                     </thead>
                     <tbody data-config-id="table_05">
                       {% for transaction in transactions %}
                       <tr>
                         <td>{% if transaction.status == 'CAPTURED' %} <span class="badge badge-success">Success</span> {% else %} <span class="badge badge-danger">Failed</span> {% endif %}</td>
                         <td>{{transaction.renter.name}}</td>
                         <td>{{transaction.renter.phone}}</td>
                         <td>{{transaction.amount}}</td>
                         <td class="text-center">{{transaction.date | date}}</td>
                         <td class="text-center">
                           {{transaction.tapId}}
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

### New Transactions

The next view is the new transactions one, let’s type the following in the same file

```python
def newTransactions(request):
   user = request.user
   transactions = Transaction.objects.filter(renter__landlord=user,date__gte=timezone.now().replace(day=1, hour=0, minute=0, second=0, microsecond=0))
   context = {
       'transactions': transactions
   }
   return render(request,'transactions/list.html', context)
```

This will take one thing only which is the `request`, from that we will grab the user and filter using the following query

```python
    Transaction.objects.filter(renter__landlord=user,date__gte=timezone.now().replace(day=1, hour=0, minute=0, second=0, microsecond=0))
```

What this does is, it looks thru all `transactions` and gets all the renters that belong to a specific landlord in this case the person who requested this page. Then, it will filter the transaction by the date which they were initiated at, it will get the dates that are greater “>” than the current date but replacing the day with the first day of the month and the hours/minutes/seconds/microseconds are replaced with 0 so it can make sure all the transactions are made at the start of the month exactly.

After that it will add those transactions into the context and sends it to the same list that we used earlier.
