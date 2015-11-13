---
menu:
  main:
    parent: apps
title: Using apt-get
weight: 10
---

The Govt Trial Cloud Foundry does not allow the use of `sudo` inside of buildpacks. If your app depends on a library that is `apt-get` installable, use the CF-flavor of the [`apt-buildpack`](https://github.com/pivotal-cf-experimental/apt-buildpack). This works great with [`buildpack-multi`](http://documentation.trial.cf.paas.alphagov.co.uk/apps/multi-buildpack-deploys/).
