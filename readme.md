## Running AB Testing on OpenShift

**Steps**

* Using the web console, create a new-app with the name `app-a` pointing to a gitrepo. Go to `Advanced Options` and uncheck the route creation option so that the app does not create a route. A custom route will be added in the next step.
* Add a route by clicking on the `Create Route` option next to service `app-a`. Give a common name for the route as it will a frontend for multiple versions of the application, _e.g._ `appab-abdemo.apps.testv3.osecloud.com`
*  Annotate the route to use `roundrobin` load balancing method with the following command:

  ``` 
  oc annotate route/appab haproxy.router.openshift.io/balance=roundrobin
  ```

* Test your app and check the results. Run `curl` in a loop and notice the output "VERSION 1".

  _e.g._
   ```
   for i in {1..20}; do curl  http://appab-abdemo.apps.testv3.osecloud.com; echo ""; done
   ```

* Edit the code to make changes. In my case I would change the version to number 2.
* Using Web Console add a new-app with name `app-b` without a route. Verify that service `app-b` got created.
* Now edit the route to split traffic between services `app-a` and `app-b`. You can change the percentages and test.
* Test using `curl` from your workstation.

  _e.g._
   ```
   for i in {1..20}; do curl http://appab-abdemo.apps.testv3.osecloud.com/; echo ""; done
   ```
