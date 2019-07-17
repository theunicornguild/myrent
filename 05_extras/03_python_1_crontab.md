### Trello

With this done we can go to our `Trello` board and move the card `Create a cronjob to run the custom django command every start of a month` to the `Doing` list

### Automation

For the final stage, after you finish deployment for your server, as the user who controls the django project `(NOT ROOT)` do the following:

Type

```bash
export VISUAL=nano; crontab -e
```

Scroll to the bottom of the file and type the following

```bash
0 0 1 * * /home/django/env/bin/python /home/django/myrent/manage.py smser
```

This will create a cron `scheduled` job to run every start of a month at the hour/minute 00:00 (midnight) and will be using the python version you used in your `virtualenv` to trigger the `manage.py` to send sms messeges

The numbers might be confusing but if you want you can create your own scheduled job using this website https://crontab.guru/
It will give you the string to put before the function that you want to do regularly

For example:

- Anything you copy from the website like `5 4 * * *` which runs every day at 04:05
- Then we add the command that we want to run so it looks like this `5 4 * * /home/django/env/bin/python /home/django/myrent/manage.py migrate` as an example

`Important notes: Make sure the path to the python executable in your virtualenv is correct and make sure the manage.py path for your project is correct or else this will not work`

### Trello & Git Workflow

With this done we can go to our `Trello` board and move the card `Create a cronjob to run the custom django command every start of a month` to the `Review` list
