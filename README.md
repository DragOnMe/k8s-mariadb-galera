# Kubernetes Galera cluster
  modified from github.com/adfinis-sygroup/openshift-mariadb-galera

## Requirements
- Kubernetes 1.13+
- At least 1 default storage class(Tested NFS) for PV

  To making storage-class nfs default, you need to do this
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
$ kubectl create -f galera.yml
```

### Cleanup cluster
```bash
$ kubectl delete statefulset mysql
$ kubectl delete svc galera
$ kubectl delete pod mysql-0 mysql-1 mysql-2
$ for i in $(seq 0 2); do kubectl delete pvc datadir-mysql-$i; done
```


## Building
```bash
$ git clone https://github.com/DragOnMe/k8s-mariadb-galera
$ cd k8s-mariadb-galera-centos
$ make
```
Technical informations how the image works in detail can be found
[here](k8s-mariadb-galera-centos/README.md)
