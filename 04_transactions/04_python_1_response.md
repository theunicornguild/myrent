### Creating Response View

Go to `transactions/views.p`y and start coding the following, keep in mind this might be a big function and we will only explain the important stuff you can skim thru it and it should be fairly similar to what we have been doing so far

```python
def responsePage(request):
   tapId = request.GET.get('tap_id')
   context = {}
   transaction = Transaction.objects.filter(tapId=tapId).first()
   if transaction is not None:
       context['transaction'] = transaction
   if tapId and transaction is None:
       jsonResponse = checkpayment(tapId)
       renter = Renter.objects.get(token=jsonResponse.get('metadata').get('udf2'))
       transaction = Transaction(
                       tapId=jsonResponse.get('id'),
                       status=jsonResponse.get('status'),
                       amount=jsonResponse.get('amount'),
                       currency=jsonResponse.get('currency'),
                       transactionId=jsonResponse.get('transaction').get('authorization_id'),
                       trackId=jsonResponse.get('reference').get('track'),
                       paymentId=jsonResponse.get('reference').get('payment'),
                       renter=renter
                       )
       transaction.save()
       renter.paid()
       renter.generate_url()
       context['transaction'] = transaction
   return render(request, 'transactions/result.html', context)
```

Let’s start explaining what is happening here. - The function expects a `tap_id` get query - It will find the transaction that belongs to that `tap_id` - If the tapId is not null, and the transaction is null it will start the following code - It will define a variable called `jsonResponse`, which will hold the output of the function `checkpayment` that takes the `tap_id` as a parameter - At this point `jsonResponse` will have lots of information regarding the payment that we created, and we need to take them one by one to organize our data - We will get the `renter` that initiated the payment using the token, the `jsonReponse` should have something called udf2 which stands for user defined field, in here we can add extra information to the payment, and in this instance we added the unique token that we generated - Then we will create a new `Transaction` object from the data that we got from `jsonResponse` - We will save the `transaction` and initiate 2 methods on the renter, which are `renter.paid()` and `renter.generate_url()` - `renter.paid()` is yet to be created we will fill the gap on that later - `renter.generate_url()` will change the token that the user has so it should be a unique id again - After that we will render the results using `transactions/result.html`

### Creating a response template

So lets create this html template, go to `transactions/templates` and create a folder called `transactions` and inside of it create a file called `result.html`
The final path should be `transactions/templates/transactions/result.html`
And type the following

```html
{% extends 'base.html' %} {% load crispy_forms_tags %} {% block main %}
</div>
<div class="col-12">
   <div class="card">
       <div class="card-body">
           <h4 class="card-title">Payment Information</h4>
           <p>Here are the details of your order:</p>
           <h5
           style="color: {% if transaction.status == 'CAPTURED' %} green {% else %} red {% endif %}"
           >
               Status: {{ transaction.status }}
           </h5>
           <p>
               <strong>Date: </strong>{{ transaction.date }}<br />
               <strong>Transaction ID: </strong>{{ transaction.transactionId }}<br />
               <strong>Tracking ID: </strong>{{ transaction.trackId }}<br />
               <strong>Payment ID: </strong>{{ transaction.paymentId }}<br />
               <strong>Total: </strong>{{ transaction.amount }}<br />
               <strong>Phone: </strong>{{ transaction.renter.phone }}<br />
           </p>
       </div>
   </div>
</div>
{% endblock %}
```
