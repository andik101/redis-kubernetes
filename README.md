# redis-kubernetes
Manifests for redis to deploy in kubernetes cluster

[Redis](http://redis.io/) is an advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.

## Introduction

This repo maintains the configuration yaml files for [Redis](https://github.com/bitnami/bitnami-docker-redis) deployment on a [Kubernetes](http://kubernetes.io) cluster.

## Installing the Redis 

To install the redis in kubernetes cluster:

```bash
$ kubectl apply -f redis-headless-configmap.yaml
$ kubectl apply -f redis-headless-service.yaml
$ kubectl apply -f redis-configmap.yaml
$ kubectl apply -f redis-service.yaml
$ kubectl apply -f redis-statefulset.yaml
```

The command deploys Redis on the Kubernetes cluster in the default configuration. 
The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: Your data are mount in a k8s cluster volume named: redis-ssd *as you can check the redis-statefulset.yaml

## Uninstalling the Redis

To uninstall/delete the `redis` deployment:

```bash
$ kubectl delete -f redis-headless-configmap.yaml
$ kubectl delete -f redis-headless-service.yaml
$ kubectl delete -f redis-configmap.yaml
$ kubectl delete -f redis-service.yaml
$ kubectl delete -f redis-statefulset.yaml
```

The commands remove Redis Kubernetes components associated with, but does not delete the data because they are stored in PVC.
If you will reinstall or upgrade the Redis, your data will be automatically backup.

If you delete the StorageClass or PVC name: redis-ssd, all your data will be lose.


The docker image used for this deployment is the Bitnami maintained Redis chart

```


### Cluster topologies

#### Default: Master

This configurtion does installing the redis with `cluster.enabled=false`, it will deploy a Redis master StatefulSet (only one master node allowed). Two services will be exposed:

   - Redis Master service: Points to the master, where read-write operations can be performed
   > **Tip**: Redis service: Exposes port 6379

For perations, access the service using port 6379, and query the current master using (redis-cli or k8s exex)

In case the current master crashes, the Cluster containers will elect a new master node.


### Using password
In this case is not used a username and password for accessin the redis service.

To use a password file for Redis you need to create a secret containing the password.


## Persistence

By default, the redis mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/) at the `/data` path. The volume is created using dynamic volume provisioning. 
Also a Persistent Volume Claim is cereated and it is specifed during installation and mounted in the hostPath.


