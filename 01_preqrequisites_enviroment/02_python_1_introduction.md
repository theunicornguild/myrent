Hello, and welcome to this tutorial, we will be creating a semi-complicated django application that relays heavily on external API endpoints, the idea of this project is creating an automated way to manage rental payments, the user only has to add his/her renter’s information and automate the process of reminding and collecting monthly rent with a clear view of who paid and who has’t paid yet.

The technologies that will be used in this project are:

    - Django
    - Requests python package
    - Pilov SMS API
    - crontab linux command

The user will be able to add a renter using their phone number among other information. Using the pilov package we will be sending that renter an sms msg with a payment url that we generate using Tap’s API to collect the money. After the payment is finished the user can get a receipt of their payment and see its status.
