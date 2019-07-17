### Creating Transaction Model

Now let's handle how transactions are created, So move to the `transactions` folder and open up `models.py` to create our transaction model

Keep in mind we are using `Tap` as our payment gateway for this guide and stuff might be different if you plan on using other payment gateways

Let’s start off by importing the Renter model from the main application so we can link it to the transaction

```python
from django.db import models
from main.models import Renter
```

Then let's type the following and as always anything new will be explained along the way

```python
class Transaction(models.Model):
   tapId = models.CharField(max_length=250,null=True,blank=True)
   status = models.CharField(max_length=250,null=True,blank=True)
   amount = models.CharField(max_length=250,null=True,blank=True)
   currency = models.CharField(max_length=250,null=True,blank=True)
   transactionId = models.CharField(max_length=250,null=True,blank=True)
   trackId = models.CharField(max_length=250,null=True,blank=True)
   paymentId = models.CharField(max_length=250,null=True,blank=True)
   renter = models.ForeignKey(Renter,on_delete=models.CASCADE,related_name="transaction")
   date = models.DateTimeField(auto_now=False,auto_now_add=True)

   def __str__(self):
       return str(self.renter.amount)
```

You can notice that we defined multiple fields and almost all of them are optional except for the `renter`. The reason behind that is because when a `renter` initiates a payment he might not continue doing so and we are filling this `transaction` model after either a payment succeded or failed.

### Migration

Now we can go to the terminal, make sure we are in the main folder of the project and both submit and make sure that the changes we did to the model can go through without issues to the database

Type the following

```bash
python manage.py makemigrations
python manage.py migrate
```
