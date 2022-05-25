### LABEL

#### Show label

```
$ kubectl get nodes --show-labels
NAME            STATUS   ROLES                  AGE    VERSION    LABELS
master01        Ready    control-plane,master   285d   v1.20.10   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=etxnpaprdkm01,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=
master02        Ready    control-plane,master   285d   v1.20.10   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=etxnpaprdkm02,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=
...

```

#### Set Label

```
$ kubectl label nodes <NODE_AME> <LABEL>=<VALUE>
```

Ex.

```
$ kubectl label nodes worker1 node-role.kubernetes.io/worker=
$ kubectl label nodes worker1 kubernetes.io/hsm=
$ kubectl label nodes worker1 kubernetes.io/ebxml=
```

#### Create a pod that gets scheduled to specific node

```
apiVersion: apps/v1
kind: Deployment
spec:
...
  template:
  ...
    spec:
    ...
      nodeSelector:
        kubernetes.io/os: linux
```

or

```
apiVersion: apps/v1
kind: Deployment
spec:
...
  template:
  ...
    spec:
    ...
      nodeSelector:
        node-role.kubernetes.io/worker: ""
```

#### Remove Label

```
kubectl label nodes <NODE NAME> <LABEL>-
```

Ex.

```
kubectl label nodes worker1 node-role.kubernetes.io/worker-
kubectl label nodes worker1 kubernetes.io/hsm-
kubectl label nodes worker1 kubernetes.io/ebxml-
```
