---
menu:
  main:
    parent: apps
title: Deploying Static Sites
weight: 10
---

There is a built-in buildpack specifically for static sites - it requires that you add a single file to the root of the project to indicate that this is a static site:


Create a file that tells Cloud Foundry that this is a static site:

```
$ touch Staticfile
```

Create `index.html`:

```
$ touch index.html
```

Add some markup:

```html
<html>
  <head>
    <title>Static Site</title>
  </head>
  <body>
    <p>Welcome to the static site!</p>
  </body>
</html>
```

Create a `manifest.yml` that uses the `staticfile-buildpack`:

```yml
---
applications:
- name: my-static-site
  memory: 64M
  env:
    FORCE_HTTPS: true
```

If the static content is included in a different folder, you can add a `path` declaration. E.g., `path: dist` or `path: assets`.

Deploy:

```
$ cf push
```

