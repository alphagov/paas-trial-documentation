---
menu:
  main:
    parent: apps
title: Databases
weight: 10
---

## Services

The easiest way to set up a database is via [services](http://docs.cloudfoundry.org/devguide/services/), which are managed by Cloud Foundry itself. To see what database options are available, run `cf marketplace`. You will now see a list of `service`s, each with a corresponding list of `plan`s.

To start a database service instance within Cloud Foundry (where `DB_NAME` is something like `APP_NAME-ENV-db`):

```bash
cf create-service <SERVICE> <PLAN> <DB_NAME>
cf bind-service <APP_NAME> <DB_NAME>
cf restage <APP_NAME>
```

To use the `redis` service with `myapp` in your staging environment, for example, you would do something like:

```bash
cf create-service redis shared-vm myapp-staging-redis
cf bind-service myapp myapp-staging-redis
cf restage myapp
```

`cf bind-service` will provide a `DATABASE_URL` environment variable for your app, which is then picked up by the `restage`. Note that for a Rails app, `bind-service` will [overwrite your `database.yml`](http://docs.cloudfoundry.org/buildpacks/ruby/ruby-service-bindings.html#rails-applications-have-autoconfigured-database-yml). The full documentation for managing service instances is [here](https://docs.cloudfoundry.org/devguide/services/managing-services.html).


