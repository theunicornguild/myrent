### Tap

So let’s see the other function. The `tap()` functoin which might look confusing and long but keep in mind its just filling data in a dictionary.

```python
def tap(renter,url):
   payload =  {
           "amount": str(renter.amount),
           "currency":"KWD",
           "threeDSecure":"true",
           "save_card":"false",
           "description":
           "Rental Payment",
           "statement_descriptor":"Rent",
           "metadata":{
               "udf1":
               str(renter.phone),
               "udf2":
               str(renter.token)
               },
           "customer": {
               "first_name": str(renter.name),
               "phone": {
                   "country_code": "965",
                   "number": str(renter.phone)
               }
           },
           "source": {
               "id": "src_all"
           },
           "redirect": {
               "url": url
           }
       }
   payload = json.dumps(payload)
   headers = {
   'authorization': "Bearer sk_test_XKokBfNWv6FIYuTMg5sLPjhJ",
   'content-type': "application/json"
   }
   response = requests.request("POST", "https://api.tap.company/v2/charges", data=payload, headers=headers)
   jsonResponse = json.loads(response.text)
   return jsonResponse
```

So this function will take two things, a `Renter` object, and a `url`, and in this case the `url` is gonna lead to the `responsePage` view that we created.

It will then create a payload which is an object and fill the data using the renter object information, like the renter’s due amount his phone number, his token his name and so on. Finally, it will fill the redirect to the designed url.

It will then use `json.dumps` to format the payload in a way so that we can send it in a request, and then post that payload with some special headers to the tap api. Afterwards, getting whatever tap responds with and return it back ( in this case tap will give us a payment url to send to our renter. Keep in mind whenever we use `requests.request()` this is a hidden request that the normal user doesn’t see and its server to server communications)

After that, we need to see the list of transactions that has been made. So, let's create a couple of views. A view to show a list of transactions made by a specific renter, a view to show all the new transactions made this month, a view to show all the renters that did not pay rent this month, and finally a view to show how much we collected this month, so all valid transactions and the sum of them.
