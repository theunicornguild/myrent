### Renter's model

Here we will be creating the model for the `Renter` objects, it's the first model that we create and it will handle saving Renter's to our database 
Go to the folder `main` and open up `models.py`.

We are gonna copy/paste this in and explain it as we go,

```python
class Renter(models.Model):
    phone = models.IntegerField(unique=True)
    amount = models.FloatField()
    adressName = models.CharField("Address name",max_length=1024)
    name = models.CharField("Full name",max_length=1024)
    address1 = models.CharField("Address line 1",max_length=1024)
    address2 = models.CharField("Address line 2",max_length=1024,null=True,blank=True)
    zip_code = models.CharField("ZIP / Postal code",max_length=12,null=True,blank=True)
    city = models.CharField("City",max_length=1024)
    country = CountryField("Country")
    def __str__(self):
        return str(self.phone)
```

So, for each renter we would like to have some specific information about them, but the unique identifier for each renter should be their phone number with some extra stuff that can be optional to fill in.

```python
class Renter(models.Model):
```

We defined the class name as Renter

```python
    phone = models.IntegerField(unique=True)
```

We defined a field on this model with the name phone and it should be an Integer(number) and unique so that no 2 renters can have the same phone number

```python
    amount = models.FloatField()
```

This is the amount that the renter will be charged, for the sake of currencies we use a float field

```python
    adressName = models.CharField("Address name",max_length=1024)
    name = models.CharField("Full name",max_length=1024)
    address1 = models.CharField("Address line 1",max_length=1024)
    address2 = models.CharField("Address line 2",max_length=1024,null=True,blank=True)
    zip_code = models.CharField("ZIP / Postal code",max_length=12,null=True,blank=True)
    city = models.CharField("City",max_length=1024)
```

For the following fields they are all Character fields, but some of them can be optional by defining null=True,blank=True. On the other hand, if we remove them we can make sure that the user has to input them.
We also defined something called max_length=1024 that will insure that the maximum amount of characters that a user can fill a field with in 1024 characters ( think twitter and their 140 character per tweet rule )

```python
    country = CountryField("Country")
```

Is a field that automatically generates all of the countries to make it easier to select one,

We need to go to the top of the page and add the following line

```python
from django_countries.fields import CountryField
```

Finally we defined a function with the name **str** that takes the object itself as a variable, this function will overwrite the default value when calling an object, so whenever we call an object it will return a stringified version of the persons phone number.

```python
    def __str__(self):
        return str(self.phone)
```

### ForignKey's

Now you might notice that a `Renter` doesn’t have any kind of connection to any other model, But he should have some connection to the user that added that Renter so he can have some kind of control to either delete/edit/view/list the renters that belong to him

Let’s go back to the `models.py` file and add those two lines,

```python
from django.contrib.auth.models import User
```

Add this line to the top of the file

```python
    landlord = models.ForeignKey(User,on_delete=models.CASCADE,related_name="landlord")
```

Add this as a new field in the `Renter` model

So what we did here is import the user model provided by django, And then created a new field on the Renter model named `landlord`

The landlord field will function as a bridge that connects both the User and the Renter, And because we created the field as a `ForeignKey` in the `Renter` side of things a user now can have many renter objects, but each renter object must have one user only
