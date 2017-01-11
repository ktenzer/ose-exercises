#Quota Exercise
The purpose of this exercise is to understand how quotas and limit ranges work
in OpenShift. Follow below steps to create quota and limit ranges.


##Create new project
```
oc new-project my-ruby
```
##Add Limit Range called resource-limits. Set CPU maximum to 2 cores and Memory maximum to 1Gi for the resource type pod.
```
$oc create -f limit-ranges.yaml -n my-ruby
$oc describe limits resource-limits
```
##Add project quota called compute resources that sets following hard limits:
```
$oc create -f compute-resources.yaml -n my-ruby
$oc describe quota compute-resources -n my-ruby
```
##Deploy ruby application called hello-world from following Git repository: https://github.com/ktenzer/ruby-hello-world
##Scale Ruby application to 3 pods. 
```
$oc scale --replicas=3 rc my-ruby-1
```

