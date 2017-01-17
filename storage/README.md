#In this exercise we will learn about persistent storage
##Create new project
```
#oc new-project demo
```
##Deploy ruby application
```
#oc new-app https://github.com/openshift/ruby-hello-world
```
#List pods
```
#oc get pods
NAME                     READY     REASON     RESTARTS AGE
ruby-hello-world-1-build 0/1       ExitCode:0 0        3m
ruby-hello-world-3-3gtig 1/1       Running    0        1m
```
#Add persistent database
```
#oc process -n openshift mysql-persistent -v DATABASE_SERVICE_NAME=database | oc create -f -
```
#Set environment parameters from ruby app
```
#oc env dc database --list | oc env dc ruby-hello-world -e -
```
##List pods
```
#oc get pods
NAME                     READY     REASON     RESTARTS AGE
database-1-2gv6j         1/1       Running    0        1m
ruby-hello-world-1-build 0/1       ExitCode:0 0        3m
ruby-hello-world-3-3gtig 1/1       Running    0        1m
```
#List persistent volumes
```
#oc get pv
NAME   LABELS CAPACITY    ACCESSMODES STATUS    CLAIM           REASON
pv0001 <none> 10737418240 RWO         Available 
pv0002 <none> 10737418240 RWO         Available 
pv0003 <none> 10737418240 RWO         Available 
pv0004 <none> 10737418240 RWO         Available 
pv0005 <none> 10737418240 RWO         Available 
pv0006 <none> 10737418240 RWO         Available 
pv0007 <none> 10737418240 RWO         Available 
pv0008 <none> 10737418240 RWO         Available 
pv0009 <none> 10737418240 RWO         Available 
pv0010 <none> 10737418240 RWO         Available 
pv0011 <none> 10737418240 RWO         Available 
pv0012 <none> 10737418240 RWO         Available 
pv0013 <none> 10737418240 RWO         Available 
pv0014 <none> 10737418240 RWO         Available 
pv0015 <none> 10737418240 RWO         Available 
pv0016 <none> 10737418240 RWO         Bound      demo/database
pv0017 <none> 10737418240 RWO         Available 
pv0018 <none> 10737418240 RWO         Available 
pv0019 <none> 10737418240 RWO         Available 
pv0020 <none> 10737418240 RWO         Available
```
##Enter key/value into application using url
```
http://ruby-hello-world.<domain>
```
#Delete database pod
```
#oc delete pod database-1-2gv6j
```
##Get the key you stored previously. Your data persisted.
