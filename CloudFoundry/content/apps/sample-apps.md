---
menu:
  main:
    parent: apps
title: Sample apps
weight: 5
---

## Deploying some sample apps

To make life a little easier for trialling our PaaS options, here’s a selection of sample applications which should help when getting started with the platforms.

**Note:** Because this is a shared environment if your attempt to create an app may fail if one already exists with the same name. You can simply append something unique - like your name - in order to overcome this. There are ways to configure domains that will allow apps with the same names across teams but this is a more advanced topic than this document is intended to cover.

### Spring Music:

A simple java app using the Spring Framework for managing your record collection. This application supports working with a variety of different database backends - the default deployment uses a simple in-memory database.

``` bash
git clone https://github.com/cloudfoundry-samples/spring-music.git
cd spring-music
./gradlew assemble
cf push
```

The app is deployed using an in-memory database with a ${random-word} in it’s URL:

The ```cf apps``` command will list the URLs associated with the app.
	
To switch from in-memory database to a postgresql backend:

``` bash
cf marketplace # view the services available
cf create-service Postgresql "Basic PostgreSQL Plan" my_spring_db
cf bind-service spring-music my_spring_db
cf restart spring-music # restart the app to detect the new db service
```

Now that the application is using a postgreSQL database backend, you may wish to scale up the number of deployed app instances as follows.

``` bash
cf scale -i 2 spring-music # run two instances of the app
```

You should now find that both instances of the app return the same data every time. The load balancer will round-robin connections between the different instances, so hitting shift-refresh in your browser a couple of times will switch between deployed instances.


### Flask SQLAlchemy Sample App:

 
 A simple microblog app built using Python/Flask using SQLAlchemy to support a postgres DB backend. The default **username / password** for the flask-sqlalchemy app is **admin / default**

 ``` bash
git clone https://github.com/alphagov/flask-sqlalchemy-postgres-heroku-example.git
cd flask-sqlalchemy-postgres-heroku-example
cf push flask_app --no-start
```

The Flask-sqlalchemy application requires a backend data service to run, so you will need to create a service instance of postgresql and bind it to the application as follows.

``` bash
cf marketplace # view the services available
cf create-service Postgresql "Basic PostgreSQL Plan" my_flaskapp_db
cf bind-service flask_app my_flaskapp_db
cf restart flask_app # restart the app to detect the new db service
```

The ```cf apps``` command will list the URLs associated with the app.
    
The flask-sqlalchemy application supports running multiple instances and can be scaled using:

``` bash
cf scale -i 3 flask_app # run three instances of the app
```

### Etherpad-lite:

Etherpad is a real-time collaborative document editing app built using nodeJS. The master branch of the github source for Etherpad was not working at the time this document was written, so you’ll need to download a tagged release from the releases page.

``` bash
wget https://github.com/cloudfoundry-community/etherpad-lite-cf/releases/download/1.5.7-cf/etherpad-lite-cf.zip
mkdir etherpad
cd etherpad
unzip ../etherpad-lite-cf.zip
cf push etherpad-lite -m 512M 
# The -m flag allows you to configures the amount of allocated memory
```

The ```cf apps``` command will list the URLs associated with the app.

The etherpad application does not support running more than one instance as there is no shared backend data service support, so if you attempt to scale this app to multiple instances then they will not all share the same data.

### Next steps...

Now that you're familiar with the platform why not try ticking off all the items on our <a href="/getting-started/bucketlist/index.html">list</a> of tasks to try out on the platform.