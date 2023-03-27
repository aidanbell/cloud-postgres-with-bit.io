# Cloud Postgres w/ Bit.io

## Introduction:

For our unit 2 projects, we were able to use MongoDB's official cloud services to host the database for our fullstack applications. Unfortunately, there's no official Postgresql hosting services that we can use. Heroku does offer us a Postgres database, but it comes at a cost. 

Luckily, there's a wonderful free service that allows us to spin up a maximum of 3 Postgres databases that we can connect our Django app to, saving us some money on the Heroku side of things. Enter: [bit.io](https://bit.io/)

## Setting up a database

After we create a new account with bit.io, you'll be prompted to create a new database. This process is made very easy by, but we'll go through it together. Once we've added in the name of our database, we can continue on the free tier, and click Create Database to spin up our new SQL DB!

![Create new db](https://i.imgur.com/4ZsH03L.png)

Once the database has been provisioned, we can click on the Connect link in our nav bar to begin the process of connecting our cloud db to our Django application. In the next window, we'll see that Bit.io has already created a series of credentials for us to use! We're going to need to copy 5 of these values to somewhere that won't be pushed up to our repo (sounds like a job for our `.env`!)

![connect page](https://i.imgur.com/Om1KNR9.png)

Make sure we copy all of these values in correctly so our application has no issues getting to our data. We'll need:

```
DB_NAME=<Database name>
DB_USER=<Username>
DB_PASSWORD=<API Key/Password>
DB_HOST=<Hostname>
DB_PORT=<Port>
```

## Configuring our Django App

Once all of our environment variables are set in, we can plug them into our `settings.py` where we connect to our Database.

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST'),
        'PORT': os.environ.get('DB_PORT')
    }
}
```

And we're all set! We should be able to spin up our Django server, and connect to our bright and shiny new database... But only after we perform one crucial step....

(I'm sure you can figure that part out! Or at least follow the error messages to a solution!)

## Deploying to Heroku!

One final note! When we deploy our apps, Heroku will notice that we're using a Postgres database, and will spin us up a local db automatically. If this didn't cost us any money, then we wouldn't need bit.io! We can see this in the output of our heroku push:
```
remote:  !     The following add-ons were automatically provisioned: heroku-postgresql. These add-ons may incur additional cost, which is prorated to the second. Run `heroku addons` for more info.
```
So let's get rid of that. If we head on over to our heroku dashboard for our app, we'll be able to see the guilty party in our Addons section:

![Heroku Postgres](https://i.imgur.com/UTIAxjk.png)

By clicking "Configure Addons", we'll easily be able to delete our provisioned Postgres DB, and bring our total cost back down to $0.00!

![Heroku cashmoney](https://i.imgur.com/tzU3YPQ.png)

Of course, we need to make sure that our environment variables have been added into Heroku, but after that, we should be all set!