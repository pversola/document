# Command Kubernetes

### Namespace

```
### Get Namespace ###
$ kubectl get ns

### Create Namespace ###
$ kubectl create ns <NAME>

### Remove Namespace ###
$ kubectl delete ns <NAME>

### Set environment ###
$ export NAMESPACE=<namespace>
```

### Deployment

```
### Get deplyment all ###
$ kubectl get deployment -A

### Get deployment with namespace ###
$ kubectl -n $NAMESPACE get deployment

### Get Images with namespace ###
$ kubectl -n $NAMESPACE get deployment -o=custom-columns='NAME:metadata.name,IMAGES:spec.template.spec.containers[*].image,READY:status.readyReplicas'
```

### Pods

```
### Get all pods ###
$ kubectl get pods -A

### Get pods with namespace ###
$ kubectl -n $NAMESPACE get pods
```

### Service

```
### Get all service ###
$ kubectl get svc -A

### Get service with namespace ###
$ kubectl -n $NAMESPACE get svc
```

#### NodePort

```
$ kubectl -n ebts expose deployment <DOEPLOYMENT_NAME> --type=NodePort --name=vault-api --cluster-ip=<CLUSTER_IP> --port=<PORT> --target-port=<TARGET_PORT>
```

Ex.

```
$ kubectl expose deployment nginx --type=NodePort --name=nginx
$ kubectl expose deployment nginx --type=NodePort --name=nginx --port=80 --target-port=80
```

### Config Map

```
### Get all config map ###
$ kubectl get cm -A

### Get config map with namespace ###
$ kubectl -n $NAMESPACE get cm
```
