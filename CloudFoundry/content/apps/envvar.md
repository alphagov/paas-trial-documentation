---
menu:
  main:
    parent: apps
title: Environment Variables
weight: -1
---

Environment variables are the means by which the Cloud Foundry runtime communicates with a deployed application about its environment. 

## Setting environment variables in the manifest file

You can set environment variables in your app by adding an `env` block to the `manifest.yml` file for your app. The `env` block consists of a heading, then one or more environment variable/value pairs.

For example:

```bash
  ...
  env:
    RAILS_ENV: production
    RACK_ENV: production
```

`cf push` deploys the application to a container on the server. The variables belong to the container environment.

Environment variables interact with manifests in the following ways:

- When you deploy an application for the first time, Cloud Foundry reads the variables described in the environment block of the manifest, and adds them to the environment of the container where the application is deployed.
- When you stop and then restart an application, its environment variables persist.


## Setting environment variables during runtime

To set environment variables you can use the `cf set-env` command.

```bash
cf set-env APP-NAME ENV-NAME ENV-VALUE
```

Viewing the environment variables that have been set for an app can be done using `cf env` and you can unset variables using `cf unset-var`.

Further documentation can be found [here](http://docs.run.pivotal.io/devguide/deploy-apps/environment-variable.html).