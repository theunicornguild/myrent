### Introduction 

Hello, and welcome to this tutorial, we will be creating a semi-complicated django application that relies heavily on external APIs. You will be creating an automated rental payments management system. The user only has to add their renters/tenants information and automate the process of "reminding" and "collecting" the monthly rent, with a clear view of who paid and who hasn't.

The technologies that will be used in this project are:
    - Django
    - Python's requests library
    - Pilov SMS API
    - Crontab linux command

Using the pilov API, we will be sending the renter an sms message with a payment url that we will be generating using TAP API to collect the money. After the payment is completed, the user will receive a receipt and see their payment's status.

### Overview 

In this project we will learn how to use some intermidate to advanced concepts in Python and Django to complete our project. We will be using the Django Auth Model to create our authentication system, which will allow users to signup/ login and reset their password (in the case they forgot it). Next, we will create a `Renter` model that will hold each renter’s information. These created renter objects will be related to the landlord (which is a user). Once we're finished creating the models, we will then create pages that will allow the landlords to create/edit/delete their renters. We'll also see how to create a `superuser`, a user which will have complete admin privileges over the application. 

Then we will create a `Transaction` model, which will store the information about all the payment attempts from a renter to a user, whether they are either successful or failures. Those transactions will be connected to the renter and to allow the landlord the ability to see/track those transactions.

We will also create provide the landlords with the ability to filter all renters/tenants who have failed to pay, so they can inqure the reason of the tardiness/absence of the payment. 

Some of the extra features we will be working on, includes:
1. Sending the user an SMS message at the beginning of every month.
2. Create a corntab job which will run automaticly every month and it will trigger the sending of those SMS messages.

Enjoy this guide!
