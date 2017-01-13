#In this exercise we will show how ops can provide an image containing os, app and it's dependencies that can be used by dev.
## CREATE PROJECTS
### Create operations and development projects for our ruby application.
```
# oc login -u admin
# oc new-project ruby-ops
# oc new-project ruby-dev
```
##SETUP USERS
###Create a user for development and operations.
```
# htpasswd /etc/origin/master/htpasswd dev
# htpasswd /etc/origin/master/htpasswd ops
```
##Create ops and dev users
```
# htpasswd /etc/origin/master/htpasswd dev
# htpasswd /etc/origin/master/htpasswd ops
```
##Enable permissions.
###Create three groups that allow operations to edit the ruby-ops project, allow development to view the ruby-ops project and also edit the ruby-dev project. In addition the ruby-dev project needs permission to pull images from the ruby-ops project.

##Create groups and add users to correct groups.
```
# oadm groups new ops-edit && oadm groups new dev-view && oadm groups new dev-edit
# oadm groups add-users ops-edit ops && oadm groups add-users dev-view dev && \
oadm groups add-users dev-edit dev
```
##Associate groups to projects and setup pull permissions to allow ruby-dev to pull images from ruby-ops.
```
# oadm policy add-role-to-group edit ops-edit -n ruby-ops && \
# oadm policy add-role-to-group view dev-view -n ruby-ops && \
# oadm policy add-role-to-group edit dev-edit -n ruby-dev && \
# oadm policy add-role-to-group system:image-puller system:serviceaccounts:ruby-dev -n ruby-ops
```
##Operations Ruby Environment

###As ops user create a ruby runtime image using application test code.
```
# oc login -u ops
# oc project ruby-ops
# oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-hello-world.git
```
##Application requires database and service name called database.
```
# oc new-app mysql-ephemeral -p DATABASE_SERVICE_NAME=database
# oc env dc database --list | oc env dc ruby-hello-world -e -
```
##Development Ruby Environment

###As dev user pull the operations ruby runtime image and build using latest code from different Github branch or project.
```
# oc login -u dev
# oc project ruby-dev
# oc new-app ruby-ops/ruby-22-centos7:latest~https://github.com/ktenzer/ruby-hello-world.git
```
##Application requires database and service name called database.
```
# oc new-app mysql-ephemeral -p DATABASE_SERVICE_NAME=database
# oc env dc database --list | oc env dc ruby-hello-world -e -
```
