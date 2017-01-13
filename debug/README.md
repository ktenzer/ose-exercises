# In this exercise we will look at how to debug running pods

In this case we're connecting to a running JBoss EAP 7.

## Deploy Java CDI REST Service

see https://github.com/j1cken/java-rs-ex

```bash
$ oc new-app jboss-eap70-openshift~https://github.com/j1cken/java-rs-ex.git --name=helloworld
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
