---
menu:
  main:
    parent: getting-started
title: Concepts
weight: 100
---

Official glossary: http://docs.cloudfoundry.org/concepts/glossary.html

## Organizations

Cloud Foundry groups its users by [organizations](http://docs.cloudfoundry.org/concepts/roles.html#orgs), or "orgs" for short. Orgs group together users for management and present a shared perimeter for services, domains and quotas. When your account is created, it will be given permissions to an org and a personal space.

### Naming convention

Within the trial, the `ORGNAME` will correspond to a project/client.

### List available orgs

```bash
cf orgs
```

Only orgs where you've been assigned an org-role or those which contain a space where you've been assigned a space-role will appear.

### See details about a specific org

...including quotas, routing domains and which spaces it includes:

```bash
cf org ORGNAME
```

### Target an org

In order to work with spaces, you'll need to do this first:

```bash
cf target -o ORGNAME
```

## Spaces

Every application is scoped to a [space](http://docs.cloudfoundry.org/concepts/roles.html#spaces). Applications in the same space share a location for app development, deployment, and maintenance.

### Naming convention

Within the trial, the `SPACENAME` will correspond to an environment, e.g. `dev` or `prod`. If the org is more general (e.g. `devops`), the space may correspond to the name of the project (e.g. `hubot`).

### Management

To create a space:

```bash
cf create-space SPACENAME
```

**Note:**  To create a space within a given org you must have the `OrgManager` role. You can check to see which users and managers for your org with:

```bash
cf org-users ORGNAME
```

## Target

The Cloud Foundry CLI keeps a global state of whatever [organization]({{< relref "#organizations" >}})+[space]({{< relref "#spaces" >}}) you're interacting with. This is known as the "target", and is set via

```bash
cf target -o ORGNAME -s SPACENAME
```

## Manifest Files

Application manifests tell `cf push` what to do with applications. This includes everything from how many instances to create and how much memory to allocate to what services applications should use.

A manifest can help you automate deployment, especially of multiple applications at once.

### Example Manifest

You can deploy applications without ever using a manifest. The benefits manifests may provide include consistency and reproducibility. 

Manifests are written in YAML. The manifest below illustrates some YAML conventions, as follows:

- The manifest begins with three dashes.
- The `applications` block begins with a heading followed by a colon.
- The application `name` is preceded by a single dash and one space.
- Subsequent lines in the block are indented two spaces to align with `name`.

```
---
applications:
- name: nifty-gui
  memory: 512M
  host: nifty
```

A minimal manifest requires only an application `name`. To create a valid minimal manifest, remove the `memory` and `host` properties from this example.

Place your `manifest.yml` in the directory that contains the application files that you want to push and run `cf push`. `cf push` searches the current directory for a manifest unless you specify another path with the `-p` option

## Buildpacks

All apps need to use a 'buildpack' specific to their language, which sets up dependencies for particular language stacks. There are standard buildpacks for most lanugages, and they will usually be auto-detected by CF. Using the standard buildpacks is strongly encouraged. In the rare case where the buildpack does not get detected correctly, or to use a custom buildpack, it can be specified in the [manifest](http://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html) (as below) or with the `-b` flag on the `cf push` command.

    buildpack: python_buildpack

To see a list of buildpacks that are available on the platform you can use:

```bash
cf buildpacks
```
    
