---
menu:
  main:
    parent: apps
title: Deploying Django
weight: 10
---

## Creating the App

There's no explicit command to create an app, but:

```bash
cf push APPNAME
```

will create or deploy the app APPNAME. But before you deploy, let's set up a few things.

### Ignoring files

1. Add `*.pyc` and `local_settings.py` to your `.gitignore`.
1. [Exclude your `.gitignore`d files from Cloud Foundry.]({{< relref "apps/deployment.md#excluding-files" >}})

### The Python runtime

Next, you want to tell Cloud Foundry which Python runtime to use. To do this, create a `runtime.txt` file, and put the full version of Python you want to deploy with in it. For instance,

```
python-3.4.1
```

### `requirements.txt`

There are a couple of packages used for the deploy that will be helpful. You'll want to add them to the `requirements.txt` for your project.

Below are the lines I added to my `requirements.txt` file. Later versions will probably work but haven't been tested.

```
whitenoise==1.0.6  #manages static assets
dj-database-url==0.3.0 #grabs environment variables and dumps them into a Django settings file
waitress==0.8.9 #a pure python WSGI server that is a replacement for gunicorn
```

### The `wsgi.py` file

When you create a Django project, a default `wsgi.py` file should be created for you. It usually looks like this:

```python
"""
WSGI config for PROJECTNAME project.

It exposes the WSGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/1.7/howto/deployment/wsgi/
"""

import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "PROJECTNAME.settings")

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

You'll need to add a few lines to import the `whitenoise` package and wrap the middleware around the wsgi application. This file will be used to start the wsgi server in your app. Instead of the above example, your wsgi.py should look like this:

```python
"""
WSGI config for PROJECTNAME project.

It exposes the WSGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/1.7/howto/deployment/wsgi/
"""

import os
from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "PROJECTNAME.settings")
# important that the whitenoise import is after the line above
from whitenoise.django import DjangoWhiteNoise

application = get_wsgi_application()
application = DjangoWhiteNoise(application)
```

The order here is important. The `DJANGO_SETTINGS_MODULE` environment variable must be set before importing `DjangoWhiteNoise`.

### The Procfile

The Procfile contains commands that Cloud Foundry will run to keep your site up. For just the Django site, create a file called `Procfile` and in it put

```
web: waitress-serve --port=$VCAP_APP_PORT APPNAME.wsgi:application
```

`APPNAME.wsgi` should be replaced with whatever the name of your project wsgi module is.

### The `manifest.yml`

The [manifest file](http://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html) tells `cf push` what to do with your app. Here's an example:

```yaml
---
# all applications use these settings and services
memory: 512M
instances: 1
applications:
- name: APPNAME
  path: .
  timeout: 180
```

As you can see, it specifies the number of instances, the memory allocated to the application, and the application itself.

### Create the App

To create and deploy your app:

```bash
cf push APPNAME --no-start
```

Normally, you would be able to view the site at `APPNAME.trial.cf.paas.alphagov.co.uk`, but we gave the command the `--no-start` flag. Before we start the app we need to set up the database.

## Setting Up the Database

**First, read [the database guide](/apps/databases/).** If you are using RDS, you'll need to change your `settings.py ` and set one environment variable to get the database up and connect. In your `settings.py` make sure you import the package we added to the `requirements.txt` file above:

```python
import dj_database_url
```

Then, you'll need to add a `DATABASES` setting like normal. I recommend adding this to the `settings.py` file so that your `local_settings.py` file will override the settings when you're working locally.

```python
DATABASES = {}
DATABASES['default'] =  dj_database_url.config()
```

### Running the app

Now try running the push command without the `--no-start` flag:

```bash
cf push APPNAME
```

It should now be running at `APPNAME.trial.cf.paas.alphagov.co.uk`!
