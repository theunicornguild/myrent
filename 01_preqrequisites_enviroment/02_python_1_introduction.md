### Introduction 

Hello, and welcome to this tutorial, we will be creating a semi-complicated django application that relies heavily on external API endpoints, the idea of this project is creating an automated way to manage rental payments, the user only has to add his/her renter’s information and automate the process of reminding and collecting monthly rent with a clear view of who paid and who has’t paid yet.

The technologies that will be used in this project are:

    - Django
    - Requests python package
    - Pilov SMS API
    - crontab linux command

The user will be able to add a renter using their phone number among other information. Using the pilov package we will be sending that renter an sms msg with a payment url that we generate using Tap’s API to collect the money. After the payment is finished the user can get a receipt of their payment and see its status.

### Overview 

in this project we will learn how to use some advanced to intermidate consepts in Django and Python to finish our project, We will be using the django auth model to generate our login/registration system which will allow users to signup login or even reset their password in the case they forgot it, after that we will create a Renter model that will hold each renter’s information and then link that renter to a specific user, that user will then be the landlord and will have complete control over the user information like rent address etc… 

After we create the model we will then create pages to allow users to create their renters or even edit/delete them if they wish then we will create a page to show the full details of a specific renter.

We will also create a superuser in this Django application which will serve as the owner of this application.

Then we will create a Transaction model that will have the information about a specific attempt of payment from a renter to a user either success or fail, we will link that transaction to a renter and then give the user the ability to see those transactions as a list or in specific detail, we will also create a new field in the renter model to indicate when was the last month that this renter has paid, and it should change whenever the renter has created a successful payment transaction.

Using this new field that we created we will also filter all those renters who failed to pay this month so the user can inqure the reason of the late payment/none payment. 

And for transactions to go through we will hide the user with a unique token that refreshes to a new one whenever there is a new payment so we can always provide them with a new payment url.

Some of the extra stuff that we will be doing include sending the user an SMS message at the beginning of every month using the Plivo API and creating a custom django manage command which is accessable using `manage.py` to send all the renters who haven’t recived an SMS this month a new message.

We will also create a corntab job which will run automaticly every month and it will trigger that custom django command that we created to send those messages on its own and finally the system should be working on its own in no time

hope you enjoy this guide!
