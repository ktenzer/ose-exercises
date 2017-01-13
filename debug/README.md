# In this exercise we will look at how to debug running pods

In this case we're connecting to a running JBoss EAP 7.

## Deploy Java CDI REST Service

see https://github.com/j1cken/java-rs-ex

```bash
$ oc new-app jboss-eap70-openshift~https://github.com/j1cken/java-rs-ex.git --name=helloworld
--> Found image 987ac22 (5 months old) in image stream "jboss-eap70-openshift" in project "openshift" under tag "latest" for "jboss-eap70-openshift"

    JBoss EAP 7.0
    -------------
    Platform for building and running JavaEE applications on JBoss EAP 7.0

    Tags: builder, javaee, eap, eap7

    * A source build using source code from https://github.com/j1cken/java-rs-ex.git will be created
      * The resulting image will be pushed to image stream "helloworld:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "helloworld"
    * Ports 8080/tcp, 8443/tcp, 8778/tcp will be load balanced by service "helloworld"
      * Other containers can access this service through the hostname "helloworld"

--> Creating resources with label app=helloworld ...
    imagestream "helloworld" created
    buildconfig "helloworld" created
    deploymentconfig "helloworld" created
    service "helloworld" created
--> Success
    Build scheduled, use 'oc logs -f bc/helloworld' to track its progress.
    Run 'oc status' to view your app.
$ oc expose svc/helloworld
route "helloworld" exposed
$
```

## Debug REST Call

```bash
$ oc env dc/helloworld DEBUG=true DEBUG_PORT=8787
deploymentconfig "helloworld" updated
$ oc patch dc/helloworld -p '{"spec":{"template":{"spec":{"containers":[{"name":"helloworld","ports":[{"name":"debug","containerPort":8787,"protocol":"TCP"}]}]}}}}' --type=strategic
"helloworld" patched
$ oc port-forward $(oc get po|grep -e '^hello'|grep Runni|awk '{print $1;}') 8787:8787
Forwarding from 127.0.0.1:8787 -> 8787
Forwarding from [::1]:8787 -> 8787
```

## Housekeeping

```bash
$ oc delete all -l app=helloworld
```
