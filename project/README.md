# In this exercise we will look at some useful considerations for managing OpenShift projects
## Disable self provisioning of new projects
```
$ oadm policy remove-cluster-role-from-group self-provisioner system:authenticated system:authenticated:oauth
```
## Set project request message
```
# vi /etc/origin/master/master-config.yaml
projectConfig:
  projectRequestMessagt "to request a new project, fill out the project request form located at
https://internal.example.com/openshift-project-request.:"
```
## Set default node selector for project
```
# vi /etc/origin/master/master-config.yaml
projectConfig:
  defaultNodeSelector: "type=user-node,region=east"
```
## Create project and set node selector for project
```
$ oadm new-project myproject --node-selector='zone=prague,region=europe'
```
## Set default project limits and limits based on user selector
```
# vi /etc/origin/master/master-config.yaml
admissionConfig:
  pluginConfig:
    ProjectRequestLimit:
      configuration:
        apiVersion: v1
        kind: ProjectRequestLimitConfig
        limits:
        - selector:
            level: admin 
        - selector:
            level: advanced 
          maxProjects: 10
        - maxProjects: 2  
```
## Update all masters and restart API and controller
```
# systemctl restart atomic-openshift-master-api
# systemctl restart atomic-openshift-master-controllers
```

