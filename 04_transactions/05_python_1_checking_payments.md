### Checking payments

So far we haven’t explained 3 things, a function called `tap()`, a function called `checkpayment()` and a method on the renter model which is called `paid()`

Let’s start with `checkpayment()`, type the following code in the `transactions/views.py` file

```python
def checkpayment(tapId):
   url = "https://api.tap.company/v2/charges/{0}".format(tapId)
   headers = {'authorization': 'Bearer sk_test_XKokBfNWv6FIYuTMg5sLPjhJ'}
   response = requests.request("GET", url, headers=headers)
   jsonResponse = json.loads(response.text)
   return jsonResponse
```

This function takes a `tapId` and sends it to the tap api with some special headers (the headers in this case are test api keys if you decide to use tap as your go to payment gateway they will provide special api keys for you) , after that it will get the response from tap and turn it into json format and return it back
Pretty simple huh?
