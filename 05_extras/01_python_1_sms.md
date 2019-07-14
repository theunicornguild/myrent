### SMS Functionality

Finally, we need to handle the SMS functionality of the application, so whenever a new month have passed we send users an SMS with their payment link

Let’s go to the `main/models.py` file and add the following method to the `Renter` model

```python
   def sms(self):
       body = """Your paymet of {1} K.D is due, Pay using the following link {2}""".format(self.name, self.amount,self.generate_url())
       msg = client.messages.create(
           src='+32466900585',
           dst='+965'+str(self.phone),
           text=body,
       )
       return msg
```
# add a link to plivo, you can maybe make `plivo` API clickable
# plivo is asking for a work email, it isn't taking hotmail or gmail....so can't sign up

You can signup to Plivo here https://www.plivo.com/ , just keep in mind you need a `work email`, an alternative is using https://www.nexmo.com/ if will work the same way with some difference in how we handle the API and what we import etc

This method will define how we send our SMS to the client, we are using the `Plivo` API for sending SMS. After you signup there, you should get a number that you can use to send messeges from. 
 * Insert that phone number into the “src” field.
 * Create your special body for the SMS and format it in a certain way, you may notice we are using the other Renter method which is generate_url() and that method will return a url that we can use to share with the user for payment. 
 * We also defined an “dst” which stands for destination and gave it the international number for Kuwait in this case “+965”.

Now, above the `Renter` model let's configure `Plivo`

```python
plivo_id = "YOUR-ID"
plivo_token = "YOUR-TOKEN"
client = plivo.RestClient(plivo_id,plivo_token)
```

You will get both the `plivo_id` and the `plivo_token` from the Plivo API website

### Creating the SMS View

Next, let's go the the views at `main/views.py` and create a small function to allow the admin to send SMS messages to the renters. It's fairly simple as we will only run the `.sms()` method on a renter object.

```python
def sendPayment(request,id):
   renter = get_object_or_404(Renter,id=id)
   renter.sms()
   return redirect('list')
```

And we need to create a url for this new view, so go to `main/urls.py` and add the following url to the list of urls

```python
   path('sendPayment/<int:id>/', sendPayment, name='sendPayment'),
```
