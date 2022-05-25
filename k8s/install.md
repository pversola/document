# Install k8s

Step 1: Update Ubuntu Server

```
$ sudo apt update
$ sudo apt -y upgrade && sudo systemctl reboot
```

Add host

```
$ sudo -i
# echo '<IP ADDRESS ENDPOINT> <Host NAME ENDPOINT>' >> /etc/hosts
# echo '<IP ADDRESS NODE1> <Host NAME NODE1>' >> /etc/hosts
# echo '<IP ADDRESS NODE2> <Host NAME NODE2>' >> /etc/hosts
...
```

Configure Aunthentication

```
### All nodes ###
# ssh-keygen -t rsa
# cat /root/.ssh/id_rsa.pub

### In first node copy SSH Key from all nodes paste in authorized_keys file ###
# vi /root/.ssh/authorized_keys
or
# echo '<AUTHORIZED KEY>' >> /root/.ssh/authorized_keys

### Other node copy authorized_keys from first node ###
# scp root@<IP FIRST NODE>:/root/.ssh/authorized_keys /root/.ssh/authorized_keys
```

Step 2: Install kubelet, kubeadm and kubectl

```
$ sudo apt update
$ sudo apt -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
    net-tools \
    nfs-common \
    telnet

$ curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

$ sudo apt update

$ sudo apt install -y kubelet kubeadm kubectl

$ sudo apt-mark hold kubelet kubeadm kubectl
```

Step 3: Disable Swap

```
$ sudo sed -ri '/\sswap\s/s/^#?/#/' /etc/fstab
$ sudo swapoff -a
```

Step 4: Install Container runtime

```
$ cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

$ sudo modprobe overlay
$ sudo modprobe br_netfilter

### Setup required sysctl params, these persist across reboots. ###
$ cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

### Apply sysctl params without reboot ###
$ sudo sysctl --system
```

Install CRI-O

```
$ export OS=xUbuntu_20.04
$ export VERSION=1.20

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /
EOF

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list
deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /
EOF

$ curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -

$ curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers-cri-o.gpg add -

$ sudo apt update
$ sudo apt -y install \
    cri-o \
    cri-o-runc
```

Start CRI-O:

```
$ sudo systemctl daemon-reload
$ sudo systemctl enable crio --now

$ cat <<EOF | sudo tee /etc/crio/crio.conf.d/02-cgroup-manager.conf
[crio.runtime]
conmon_cgroup = "pod"
cgroup_manager = "cgroupfs"

[crio.image]
registries = [
"quay.io",
"docker.io"
]
EOF

$ cat <<EOF | sudo tee /etc/default/kubelet
KUBELET_EXTRA_ARGS=--feature-gates="AllAlpha=false,RunAsGroup=true" --container-runtime=remote --container-runtime-endpoint='unix:///var/run/crio/crio.sock' --runtime-request-timeout=5m
EOF

$ sudo systemctl restart crio kubelet
$ sudo kubeadm config images pull
```

Step 5: Initialize master node

master single node

```
$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket=unix:///var/run/crio/crio.sock
```

multi node

```
$ sudo kubeadm init --control-plane-endpoint "<ENDPOINT IP>:<PORT>" --pod-network-cidr=10.244.0.0/16 --upload-certs


$ sudo kubeadm init \
  --pod-network-cidr "10.244.0.0/16" \
  --service-dns-domain "k8s-master" \
  --control-plane-endpoint "k8s-master:6443" \
  --upload-certs
```

```
$ sudo kubeadm init --control-plane-endpoint "<ENDPOINT IP>:<PORT>" --apiserver-advertise-address=192.168.200.12 --pod-network-cidr=10.244.0.0/16 --upload-certs
```

disabled scheduling from the cluster

```
$ kubectl taint nodes --all node-role.kubernetes.io/master-
```

Setup environment for kubectl command.

master

```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

worker

```
$ mkdir -p $HOME/.kube
$ scp
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Step 6: Install network plugin (CNI) configuration on Master

```
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

Check cluster status: `$ kubectl cluster-info`  
Check all pods are running in kube-system: `watch kubectl get pods -n kube-system`  
Confirm master node is ready: `$ kubectl get nodes`

**_FIX:_** remove old calico configs from kubernetes without `kubeadm reset`

```
$ sudo ip route flush proto bird
$ sudo ip link list | grep cali | awk '{print $2}' | cut -c 1-15 | xargs -I {} ip link delete {}
$ sudo modprobe -r ipip
$ sudo rm /etc/cni/net.d/10-calico.conflist && rm /etc/cni/net.d/calico-kubeconfig
$ sudo service kubelet restart
```

Step 7: Add worker nodes

```
$ kubeadm join <ENDPOINT IP>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<DISCOVERY TOKEN>
```

Configure label

```
kubectl label node <WORKER NODE> kubernetes.io/role=worker1
kubectl label node <WORKER NODE> kubernetes.io/role=worker2
```

Allow requires port

- **Control-plane nodes**

```
$ sudo ufw allow 6443/tcp
$ sudo ufw allow 2379:2380/tcp
$ sudo ufw allow 10250/tcp
$ sudo ufw allow 10251/tcp
$ sudo ufw allow 10252/tcp
```

- **Worker nodes**

```
$ sudo ufw allow 10250/tcp
$ sudo ufw allow 30000:32767/tcp
```

```
$ docker info | grep -i cgroup
$ sudo cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

$ sudo sed -i "s/cgroup-driver=systemd/cgroup-driver=cgroupfs/g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

$ systemctl daemon-reload
$ systemctl restart docker
$ systemctl restart kubelet

$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

$ kubectl get nodes
$ kubectl taint nodes --all node-role.kubernetes.io/master-
```

Install Helm

```
$ curl -fsSL https://baltocdn.com/helm/signing.asc | sudo apt-key add -
$ cat <<EOF | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
deb https://baltocdn.com/helm/stable/debian/ all main
EOF
$ sudo apt update
$ sudo apt install -y helm
```

Install Dashboard

```
$ helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
$ helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
EOF

-------------------------------------------------------------------

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF

$ echo "" ; kubectl get secret -n kubernetes-dashboard $(kubectl get serviceaccount admin-user -n kubernetes-dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode ; echo ""
```

```
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"k8s-app":"kubernetes-dashboard"},"name":"kubernetes-dashboard","namespace":"kubernetes-dashboard"},"spec":{"ports":[{"port":443,"targetPort":8443}],"selector":{"k8s-app":"kubernetes-dashboard"}}}
  creationTimestamp: "2021-05-11T05:48:15Z"
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
  resourceVersion: "14246"
  uid: 3d0f319a-cbd4-46cf-b75a-2784c1b0075e
spec:
  clusterIP: 10.110.230.88
  clusterIPs:
  - 10.110.230.88
  ports:
  - nodePort: 32000
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
```

Install Prometheus and Grafana

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus
helm install grafana grafana/grafana

$ cat <<EOF | kubectl apply -f -
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
reclaimPolicy: Delete
EOF

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: prometheus-volume
spec:
  storageClassName: local-storage
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
EOF

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: prometheus-volume
spec:
  storageClassName: ""
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
EOF

$ cat <<EOF | kubectl apply -f -
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
EOF

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: prometheus-volume
spec:
  storageClassName: local-storage
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
EOF
```

Install HPA

```
$ kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Depending on your cluster setup, you may also need to change flags passed to the Metrics Server container. Most useful flags:

`--kubelet-preferred-address-types` - The priority of node address types used when determining an address for connecting to a particular node (default [Hostname, InternalDNS, InternalIP, ExternalDNS, ExternalIP])  
`--kubelet-insecure-tls` - Do not verify the CA of serving certificates presented by Kubelets. For testing purposes only.  
`--requestheader-client-ca-file` - Specify a root certificate bundle for verifying client certificates on incoming requests.

```
$ kubectl edit deploy -n kube-system metrics-server
[...]
spec:
  [...]
  template:
    [...]
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        image: k8s.gcr.io/metrics-server/metrics-server:v0.4.2

[...]
```

```
$ kubectl create deployment php-apache --image=k8s.gcr.io/hpa-example
$ kubectl patch deployment php-apache -p='{"spec":{"template":{"spec":{"containers":[{"name":"hpa-example","resources":{"requests":{"cpu":"200m"}}}]}}}}'
$ kubectl create service clusterip php-apache --tcp=80
$ kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
$ kubectl get hpa
$ kubectl run --generator=run-pod/v1 -i --tty load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://php-apache; done

$ kubectl get hpa -w

kubectl delete hpa,service,deployment php-apache
kubectl delete pod load-generator
```

How to manual join node **(Token Expire)**  
Step 1: Get Token

```
$ sudo kubeadm token list
$ sudo kubeadm token create --print-join-command
```

Step 2: Get Discovery Token CA cert Hash

```
$ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

Step 3: Get API Server Advertise address

```
$ kubectl cluster-info
Kubernetes control plane is running at https://<HOST>:<PORT>
KubeDNS is running at https://<HOST>:<PORT>/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

Setp 4: Join a new Kubernetes Worker Node a Cluster

```
$ kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

Step 5: Removing a Worker Node from the Cluster

```
$ kubectl drain  <node-name> --delete-local-data --ignore-daemonsets
$ kubectl cordon <node-name>
$ sudo kubeadm reset
```
