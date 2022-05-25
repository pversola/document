# Update Patch Linux

## Standard

### Ubuntu

```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo systemctl reboot
```

### CentOS

Ensure you have the yum security plugin

```
$ rpm -qa | grep -i yum-plugin-security
$ yum install yum-plugin-security
```

Check if there are updates

```
$ yum --security check-update
```

Update to specify security updates only.

```
$ yum update --security
```

## Kubernetes Cluster

1. Check OS Kernel and Runtime [On MasterNode]

```
$ kubectl get nodes -o wide
NAME            STATUS   ROLES                  AGE    VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
etxnpauatkm01   Ready    control-plane,master   162d   v1.20.13   10.255.6.11   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm02   Ready    control-plane,master   162d   v1.20.13   10.255.6.12   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm03   Ready    control-plane,master   162d   v1.20.13   10.255.6.13   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw01   Ready    worker                 162d   v1.20.13   10.255.6.21   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw02   Ready    worker                 162d   v1.20.13   10.255.6.22   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw03   Ready    worker                 162d   v1.20.13   10.255.6.23   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
```

2. Unjoin node [On MasterNode]

```
$ kubectl drain <NODE NAME> --ignore-daemonsets
```

if error check message and follow command

```
$ kubectl drain <NODE NAME> --ignore-daemonsets
...
error: cannot delete Pods with local storage (use --delete-emptydir-data to override): kube-system/metrics-server-698995547f-dx7v8
```

```
$ kubectl drain <NODE NAME> --ignore-daemonsets --delete-emptydir-data

```

3. Check hold package k8s [On Node]

```
$ sudo apt-mark showhold
kubeadm
kubectl
kubelet
```

if not hold **PLEASE** run Command

```
$ sudo apt-mark hold kubelet kubeadm kubectl
```

4. Update Patch [On Node]

```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo systemctl reboot
```

5. Check OS Kernel and Runtime again [On MasterNode]

```
$ kubectl get nodes -o wide
NAME            STATUS                     ROLES                  AGE    VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
etxnpauatkm01   Ready                      control-plane,master   162d   v1.20.13   10.255.6.11   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm02   Ready                      control-plane,master   162d   v1.20.13   10.255.6.12   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm03   Ready,SchedulingDisabled   control-plane,master   162d   v1.20.13   10.255.6.13   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkw01   Ready                      worker                 162d   v1.20.13   10.255.6.21   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw02   Ready                      worker                 162d   v1.20.13   10.255.6.22   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw03   Ready                      worker                 162d   v1.20.13   10.255.6.23   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6

```

6. Join node

```
$ kubectl uncordon <NODE NAME>
```

```
$ kubectl get nodes -o wide
NAME            STATUS   ROLES                  AGE    VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
etxnpauatkm01   Ready    control-plane,master   162d   v1.20.13   10.255.6.11   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm02   Ready    control-plane,master   162d   v1.20.13   10.255.6.12   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkm03   Ready    control-plane,master   162d   v1.20.13   10.255.6.13   <none>        Ubuntu 20.04.4 LTS   5.4.0-109-generic   cri-o://1.20.7
etxnpauatkw01   Ready    worker                 162d   v1.20.13   10.255.6.21   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw02   Ready    worker                 162d   v1.20.13   10.255.6.22   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
etxnpauatkw03   Ready    worker                 162d   v1.20.13   10.255.6.23   <none>        Ubuntu 20.04.3 LTS   5.4.0-107-generic   cri-o://1.20.6
```
