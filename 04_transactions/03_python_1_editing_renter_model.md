### Adding Token Field

Let’s go to `main/models.py` to add the `token` field

At the top of the page add this new import

```python
from uuid import uuid4
```

And in the `Renter` model add this field

```python
   token = models.CharField(max_length=250,default=uuid4())
```

And to generate a new token let’s create a method for the `Renter` model

```python
   def generate_url(self):
       self.token = uuid4()
       self.save()
       return "https://myrent.codeunicorn.io/pay/{0}".format(self.token)
```

`a method is a function relating to the class so it should be indented inside of the class and not outside !`

This url is our demo website and we are appending the unique token at the end of the url, it might differe in the case of Development vs Production as the url might be `http://127.0.0.1:8000/pay/` in the case of Development

