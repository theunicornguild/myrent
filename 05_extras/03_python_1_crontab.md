### Automation

For the final stage after you finish deployment for your server, as the user who controls the django project `(NOT ROOT)` do the following:

Type

```bash
export VISUAL=nano; crontab -e
```

Scroll to the bottom of the file and type the following

```bash
0 0 1 * * /home/django/env/bin/python /home/django/myrent/manage.py smser
```

This will create a cron `scheduled` job to run every start of a month at the hour/minute 00:00 (midnight) and will be using the python version you used in your `virtualenv` to trigger the `manage.py` to send sms messages

`Important notes: Make sure the path to the python executable in your virtualenv is correct and make sure the manage.py path for your project is correct or else this will not work`
