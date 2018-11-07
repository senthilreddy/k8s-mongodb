# PersistentVolume

$ kubectl create -f mongodb-pv.yml 
persistentvolume/mongodb-pv created

$ kubectl get pv 
NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                                STORAGECLASS   REASON   AGE
mongodb-pv   1Gi        RWO,ROX        Retain           Available                                                    15s

# PersistentVolumeClaim

$ kubectl create -f mongodb-pvc.yml
persistentvolumeclaim/mongodb-pvc created

$ kubectl get pvc
NAME         STATUS   VOLUME             CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mongodb-pvc  Bound    mongodb-pv         1Gi        RWO,ROX                       14s

# Pod

$ kubectl create -f mongodb.yml 
pod/mongodb created

$ kubectl get pod
NAME      READY   STATUS      RESTARTS   AGE
mongodb   1/1     Running     0          25s

# Testing

$ kubectl exec -it mongodb mongo
MongoDB shell version v4.0.4
connecting to: mongodb://127.0.0.1:27017
Implicit session: session { "id" : UUID("faf1d24b-660b-4111-96c4-a5b801389c28") }
MongoDB server version: 4.0.4
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2018-12-15T06:05:14.166+0000 I STORAGE  [initandlisten] 
2018-12-15T06:05:14.166+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-12-15T06:05:14.166+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-12-15T06:05:15.431+0000 I CONTROL  [initandlisten] 
2018-12-15T06:05:15.431+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-12-15T06:05:15.431+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-12-15T06:05:15.431+0000 I CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> use mystore
switched to db mystore
> db.foo.find()
> db.foo.insert({name:'foo'})
WriteResult({ "nInserted" : 1 })
> db.foo.find()
{ "_id" : ObjectId("5c149a59ee3005c103bd549d"), "name" : "foo" }

$ kubectl delete pod mongodb
pod "mongodb" deleted

$ kubectl create -f mongodb.yml 
pod/mongodb created

$ kubectl exec -it mongodb mongo
MongoDB shell version v4.0.4
connecting to: mongodb://127.0.0.1:27017
Implicit session: session { "id" : UUID("b05c75b4-8fbf-4479-b046-6369bf3577d6") }
MongoDB server version: 4.0.4
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2018-12-15T06:09:52.327+0000 I STORAGE  [initandlisten] 
2018-12-15T06:09:52.327+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-12-15T06:09:52.327+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-12-15T06:09:53.618+0000 I CONTROL  [initandlisten] 
2018-12-15T06:09:53.618+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-12-15T06:09:53.618+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-12-15T06:09:53.618+0000 I CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> use mystore
switched to db mystore
> db.foo.find()
{ "_id" : ObjectId("5c149a59ee3005c103bd549d"), "name" : "foo" }

# hostPath

$ kubectl get pod -o=custom-columns=NODE:.spec.nodeName,NAME:.metadata.name --all-namespaces
NODE           NAME
k8s-worker     mongodb

## Data is located in k8s-worker under /tmp/data/mongodb-data


