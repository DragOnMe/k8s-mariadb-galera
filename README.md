# Kubernetes Galera cluster

Modified from github.com/adfinis-sygroup/openshift-mariadb-galera

## Requirements
- Kubernetes v1.11.x to v1.13+
- At least 1 default storage class(Tested NFS) for PV
- Galera Cluster should be deployed on ns-galera namespace. This can be modified my editing galera_ns.yaml if needed.

  To make storage-class nfs default, you need to do this
```
  kubectl patch storageclass nfs -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## General informations

### Environment variables and volumes

The image recognizes the following environment variables

|  Variable name         | Description                               |
| :--------------------- | ----------------------------------------- |
|  `MYSQL_USER`          | User name for MySQL account to be created |
|  `MYSQL_PASSWORD`      | Password for the user account             |
|  `MYSQL_DATABASE`      | Database name                             |
|  `MYSQL_ROOT_PASSWORD` | Password for the root user (optional)     |

Mount information for MySQL data directory volume

| Volume mount point       | Description          |
| :----------------------- | -------------------- |
|  `/var/lib/mysql`        | MySQL data directory |


## Usage in Kubernetes

This image runs on kubernetes as well. Last tested version is v1.13.3

This YAML generates 1 headless service and statefulset and 1 general k8s service.

### Create cluster
```bash
$ kubectl create -f galera_ns.yaml
$ kubectl apply -f galera_k8s.yml
```

### Cleanup cluster
```bash
$ kubectl delete -f galera_k8s.yaml
$ for i in $(seq 0 2); do kubectl delete persistentvolumeclaim datadir-mysql-$i; done
```

or

```bash
$ kubectl delete statefulset galera-ss -n $YOUR_NAMESPACE
$ kubectl delete svc galera-svc -n $YOUR_NAMESPACE
$ kubectl delete svc galera-hs -n $YOUR_NAMESPACE
$ for i in $(seq 0 2); do kubectl delete persistentvolumeclaim datadir-mysql-$i; done
```

and then, to delete the namespace

```bash
$ kubectl delete -f galera_ns.yaml
```

## Building
```bash
$ git clone https://github.com/DragOnMe/k8s-mariadb-galera
$ cd k8s-mariadb-galera-centos
$ sudo docker login -u $YOUR-DOCKERHUB-USERNAME
$ sudo make image
$ sudo make push
```
Technical informations how the image works in detail can be found
[here](k8s-mariadb-galera-centos/README.md)
