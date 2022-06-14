# Kubenetes Renew Certs Cluster

```

$ kubectl get nodes | grep control-plane
NAME            STATUS   ROLES                  AGE    VERSION
etxnpauatkm01   Ready    control-plane,master   150d   v1.20.13
etxnpauatkm02   Ready    control-plane,master   150d   v1.20.13
etxnpauatkm03   Ready    control-plane,master   150d   v1.20.13

$ kubectl drain <MASTER NODE> --ignore-daemonsets
$ sudo kubeadm certs renew all
$ sudo systemctl daemon-reload && sudo systemctl restart kubelet
$ kubectl uncordon <MASTER NODE>

$ sudo kubeadm certs check-expiration

```