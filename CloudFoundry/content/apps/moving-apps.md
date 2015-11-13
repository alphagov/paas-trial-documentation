---
menu:
  main:
    parent: apps
title: Moving apps between spaces
weight: 10
---

If you have an app that exists in one org/space but you need to move it to another:

1. Deploy the new application instance using the appropriate steps on the [cloning]({{< relref "apps/cloning.md" >}}) page.
    * Make sure to run `cf target -o <NEW_ORG> -s <NEW_SPACE>` before running `cf push`.
    * If you keep the app name the same, you may need to use a different `host` to avoid route conflicts.
1. Go back to the old space.

    ```bash
    cf target -o <OLD_ORG> -s <OLD_SPACE>
    ```

1. If you are changing orgs, remove the [domain](http://docs.cloudfoundry.org/devguide/deploy-apps/domains-routes.html#delete-domains)/[route](http://docs.cloudfoundry.org/devguide/deploy-apps/domains-routes.html#delete-route).

    ```bash
    cf delete-domain <DOMAIN>
    # or
    cf delete-route <SHARED_DOMAIN> -n <HOST>
    ```

1. Go back to the new space.

    ```bash
    cf target -o <NEW_ORG> -s <NEW_SPACE>
    ```

1. Delete the old app.

    ```bash
    cf target -o <OLD_ORG> -s <OLD_SPACE>
    cf delete <APP_NAME>
    ```
