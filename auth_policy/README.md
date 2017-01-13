# Is this exercise we will look at authorization policies
## Show cluster roles and their rule sets
```
$ oc describe clusterPolicy default
```
## Show cluster bindings showing what users are associated to roles
```
$ oc describe clusterPolicyBindings :default
```
## Show local roles and their rule sets for project
```
$ oc describe policyBindings :default -n myproject
```
## Show which users can perform action on resource
```
$ oadm policy who-can edit pep-dev
```
## Create custom role
```
$ oc get clusterrole view -o yaml > clusterrole_view.yaml
$ cp clusterrole_view.yaml localrole_exampleview.yaml
$ vim localrole_exampleview.yaml
# 1. Update kind: ClusterRole to kind: Role
# 2. Update name: view to name: exampleview
# 3. Remove resourceVersion, selfLink, uid, and creationTimestamp
$ oc create -f path/to/localrole_exampleview.yaml -n <project_you_want_to_add_the_local_role_exampleview_to>
```

