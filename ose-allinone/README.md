#In this exercise we will setup an allinone OpenShift environment. There are many ways to create an allinone environment. Using the OpenShift Ansible installer, the container development kit and containerized running in docker using oc cluster up. The easiest is to run OpenShift as docker container and that is what we will do in this exercise.

## Register at https://developers.redhat.com/
## Download RHEL 7.3 at https://developers.redhat.com/downloads/
## Install RHEL 7.3 ISO in Virtual Machine with 8GB memory
## Log into RHEL 7.3
## Register using your developer subscription
```
[root@ose3 ~]# subscription-manager register
Registering to: subscription.rhn.redhat.com:443/subscription
Username: keith.tenzer@mydomain.com
Password: 
The system has been registered with ID: a05ce453-3883-4d73-b2f7-625fab64c05b
```
## Update system
```
[root@ose3 ~]# yum update
```
##Install Docker
```
[root@ose3 ~]# yum install -y docker
```
##Configure Docker
##Copy oc command to /usr/local/bin
## Start OpenShift CLuster
```
[root@ose3 ~]# /usr/local/bin/oc cluster up --image="registry.access.redhat.com/openshift3/ose"
```

