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
##Attach subscription
```
[root@ose3 ~]# subscription-manager attach --auto

All installed products are covered by valid entitlements. No need to update subscriptions at this time.

Installed Product Current Status:
Product Name: Red Hat Enterprise Linux Server
Status:       Subscribed
```
##Enable required repositories
```
[root@ose3 ~]# subscription-manager repos \
    --enable="rhel-7-server-rpms" \
    --enable="rhel-7-server-extras-rpms"
```
##Disable firewalld
```
[root@ose3 ~]# systemctl disable firewalld
[root@ose3 ~]# systemctl stop firewalld
```
## Update system
```
[root@ose3 ~]# yum update -y
```
##Install Docker
```
[root@ose3 ~]# yum install -y docker
```
##Configure Docker
```
[root@ose3 ~]# vi /etc/sysconfig/docker
INSECURE_REGISTRY='--insecure-registry 172.30.0.0/16'
```
##Enable and start Docker
```
[root@ose3 ~]# systemctl enable docker
[root@ose3 ~]# systemctl start docker
```
##Clone git repository
```
[root@ose3 ~]# git clone https://github.com/ktenzer/ose-exercises.git
```
##Extract tar and copy oc command to /usr/local/bin
```
[root@ose3 ~]# cd ose-exercises/ose-allinone
[root@ose3 ~]# tar xvf oc-3.3.1.5-linux.tar.gz;mv oc /usr/local/bin
```
##If you don't have internet access import docker images
```
[root@ose3 ~]# gunzip oc_cluster_images.tar.gz
[root@ose3 ~]# tar xvf oc_cluster_images.tar 
[root@ose3 ~]# cd oc_cluster_images
[root@ose3 ~]# for p in `ls`;do docker load -i $p; done
```
## Start OpenShift Cluster
```
[root@ose3 ~]# /usr/local/bin/oc cluster up --image="registry.access.redhat.com/openshift3/ose"
```
## Start OpenShift Cluster and preserve data between restarts
```
/usr/local/bin/oc cluster up --image="registry.access.redhat.com/openshift3/ose" --host-data-dir /root/ose --use-existing-config
```
##Login to docker container running OpenShift
```
[root@ose3 ~]# docker ps
[root@ose3 ~]# docker exec -i -t <container id> /bin/bash
```

