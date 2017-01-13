# In this exercise we will connect to a MongoDB instance from NodeJS

When running we connect with a MongoDB client to the running MongoDB instance.

## Deploy with template
```bash
$ oc new-app --template=nodejs-mongodb-example
--> Deploying template "nodejs-mongodb-example" in project "openshift"

     nodejs-mongodb-example
     ---------
     An example Node.js application with a MongoDB database

     * With parameters:
        * Name=nodejs-mongodb-example
        * Namespace=openshift
        * Memory Limit=512Mi
        * Memory Limit (MongoDB)=512Mi
        * Git Repository URL=https://github.com/openshift/nodejs-ex.git
        * Git Reference=
        * Context Directory=
        * Application Hostname=
        * GitHub Webhook Secret=fiC7E0tbaPj3KygIsBV6Wc8Ny0jxULRyyKHx2d38 # generated
        * Generic Webhook Secret=Ot6GVbSgB2pKEBIhOGFqtMYp8DV46Xj3ipS7BhvJ # generated
        * Database Service Name=mongodb
        * MongoDB Username=user4YG # generated
        * MongoDB Password=84KjvqEkSLaWU0Vl # generated
        * Database Name=sampledb
        * Database Administrator Password=XoMr1gYI6XDyatuE # generated
        * Custom NPM Mirror URL=

--> Creating resources with label app=nodejs-mongodb-example ...
    service "nodejs-mongodb-example" created
    route "nodejs-mongodb-example" created
    imagestream "nodejs-mongodb-example" created
    buildconfig "nodejs-mongodb-example" created
    deploymentconfig "nodejs-mongodb-example" created
    service "mongodb" created
    deploymentconfig "mongodb" created
--> Success
    Build scheduled, use 'oc logs -f bc/nodejs-mongodb-example' to track its progress.
    Run 'oc status' to view your app.
$
```

## Connect to MongoDB with MongoDB client
```bash
$ oc port-forward $(oc get po|grep -e '^mongo'|grep Runni|awk '{print $1;}') 27017:27017
Forwarding from 127.0.0.1:27017 -> 27017
Forwarding from [::1]:27017 -> 27017
$ oc env dc/mongodb --list
# deploymentconfigs mongodb, container mongodb
MONGODB_USER=user4YG
MONGODB_PASSWORD=84KjvqEkSLaWU0Vl
MONGODB_DATABASE=sampledb
MONGODB_ADMIN_PASSWORD=XoMr1gYI6XDyatuE
$ mongo -u user4YG -p84KjvqEkSLaWU0Vl sampledb
MongoDB shell version: 3.2.8
connecting to: sampledb
> show collections;
counts
> db.counts.find();
{ "_id" : ObjectId("58791632abe5350019a5f4e2"), "ip" : "10.1.5.1", "date" : 1484330546928 }
> exit
bye
$
```
