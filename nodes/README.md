# In this exercise we will look at how to manage nodes in OpenShift environment
## List nodes
```
$ oc get nodes
```
## Describe node
```
$ oc describe node node1.v1tt5gv0hqjudfkkftdccsm4cc.ax.internal.cloudapp.net
```
## Label node
```
$ oc label node <node> region=europe
```
## List pods running on a node
```
$ oadm manage-node node1.v1tt5gv0hqjudfkkftdccsm4cc.ax.internal.cloudapp.net --list-pods
```
## Enable or disable scheduler
## For maintenance purposes disabling scheduling guarantees that new pods wont be scheduled on node
```
$ oadm manage-node <node1> --schedulable=false
$ oadm manage-node <node1> --schedulable
```
## Evacuate nodes
## To see what pods would be evacutated from node you can perform dry run
## --selector=<node_selector> can be used to select nodes forn evacuation based on their selector
```
$ oadm manage-node <node> --evacuate --dry-run
```
