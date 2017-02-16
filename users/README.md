# In this exercise we will create users, groups and bind roles to project in OpenShift

##Create new project
```
$oc new-project project1
```
#Create group
```
$oadm groups new dev
```
#List users and find your user
```
$oc get users
```
#Look at identities
```
$oc get identities
```
#Add your user to the group dev
```
$oadm groups add-users dev user
```
#Bind role edit to the group dev. Default roles that can be assigned to projects are view, edit and admin.
```
$oadm policy add-role-to-group edit dev -n project1
```
#Create group from yaml
```
apiVersion: v1
kind: Group
metadata:
  name: ops-edit
users:
- ops
- ops-admin
```
#create role binding from yaml
```
apiVersion: v1
groupNames:
- ops-edit
kind: RoleBinding
metadata:
  name: edit
  namespace: project1
roleRef:
  name: edit
subjects:
- kind: Group
  name: ops-edit
```
