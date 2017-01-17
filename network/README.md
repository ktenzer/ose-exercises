#In this exercise we will see how to configure SDN and isolate pods at project level
##Configure SDN Subet Plugin
###SDN Subnet Plugin allows all pods to communicate with other pods accross projects
### The maste-config.yaml needs to be updates on all masters and node-config.yaml on all nodes.
```
# vi /etc/origin/master/master-config.yaml
networkConfig:
  clusterNetworkCIDR: 10.128.0.0/14 
  hostSubnetLength: 9 
  networkPluginName: "redhat/openshift-ovs-subnet" 
  serviceNetworkCIDR: 172.30.0.0/16  
# vi /etc/origin/nodes/node-config.yaml
networkConfig:
  mtu: 1450 
  networkPluginName: "redhat/openshift-ovs-subnet" 
```
##Restart OpenShift Services (multi master)
```
# systemctl restart atomic-openshift-master-api
# systemctl restart atomic-openshift-master-controllers
```
##Configure SDN Multitenant Plugin
###SDN Multitenant Plugin uses vxlan ids to segregate pods at project level. Each project has its own network
###namespace and vxlan id. In order for pods and services to access one another accross projects, access needs to be enabled.
### The maste-config.yaml needs to be updates on all masters and node-config.yaml on all nodes.
```
# vi /etc/origin/master/master-config.yaml
networkConfig:
  clusterNetworkCIDR: 10.128.0.0/14 
  hostSubnetLength: 9 
  networkPluginName: "redhat/openshift-ovs-subnet" 
  serviceNetworkCIDR: 172.30.0.0/16  
# vi /etc/origin/nodes/node-config.yaml
networkConfig:
  mtu: 1450 
  networkPluginName: "redhat/openshift-ovs-subnet" 
```
##Restart OpenShift Services (multi master)
```
# systemctl restart atomic-openshift-master-api
# systemctl restart atomic-openshift-master-controllers
```
##Enable network access between two projects
```
# oadm pod-network join-projects --to=project1 project2 project3 project4
```
##Isolate projects
```
# oadm pod-network isolate-projects project1 project5
```
##Enable global access to projects
```
# oadm pod-network make-projects-global project6
```
