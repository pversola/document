# Troubleshoot kubectl connection refused

You really would rather be doing something else…  
… but you’re here now because you can’t connect to your cluster. Here are a few pointers that could help. Let’s take a dive into the things that might stop you from connecting to your pods!!

First — check your component status

```
kubectl get componentstatuses

NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health": "true"}
```

then check your ports to see what’s listening:

```
sudo netstat -lnpt|grep kube
## check your kube-apiserver at 6443 see if it's listening
netstat -a | grep 6443
```

Check the firewall, might want to turn it OFF for just a bit to rule it out:

```
## RHEL and CENTOS
sudo systemctl status firewalld
sudo systemctl stop firewalld
## turn on:
sudo systemctl start firewalld

## UBUNTU
sudo ufw status verbose
sudo ufw disable
## turn on
sudo ufw enable
## make sure your kube-apiserver can get through port 6443
sudo ufw allow 6443/tcp
## try telnet from another host to your apiserver
telnet MASTER-IP 6443
```

Check your kubeconfig file and, well, first make sure you have one!

```
## go to your home directory, and check to see if .kube is there
cd ~
ls -la
```

If you don’t have a .kube or config file, you’ll need to move the one that was installed into your home directory (there’s workarounds but this is the most common method)

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
## try your get pods command now
kubectl get pods
```

If that didn’t work… Let’s check your config file… your .kube directory should be something like this:

```
cd .kube
ls -la
user1@ubuntu:~/.kube$ ll
total 24
4 drwxrwxr-x  4 user1 user1 4096 Mar 21 17:15 .
4 drwxr-xr-x 52 user1 user1 4096 Mar 20 21:23 ..
4 drwxr-xr-x  3 root  root  4096 Jan 31 22:15 cache
8 -rw-------  1 user1 user1 5683 Mar  8 21:26 config
4 drwxr-xr-x  3 root  root  4096 Mar 21 17:04 http-cache
```

Check the server address of your master node that you are running kubectl from:

```
ifconfig
## should see an entry for your wireless card or ethernet
wlp1s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.7  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 2605:6000:e88f:7c00:d872:ac7a:d4d5:fd4f  prefixlen 64  scopeid 0x0<glo
```

This address and port 6443 is the address of the API for kubernetes that kubectl accesses EVERY time you use kubectl. It’s a privileged port that requires keys to get in that are inside the kube config file. Check out your kube config file and notice a couple of things:

```
vi config
## brief response:
server: https://192.168.0.7:6443
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
- cluster:
    certificate-authority-data:  BUNCH OF GARBLED NEEDY SHIT
- name: kubernetes-admin
user:
    client-certificate-data:  MORE GARBLED NEEDY SHIT
```

If you restarted your host, you probably have a different IP address than the one that was used when issuing your certificates. You could use the hostname instead in the kube config file:

```
## find the hostname of your box
hostname
## response:
ubuntu.local
## edit your config file
vi config
## brief response:
server: https://ubuntu.local:6443
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
```

Let’s break some things and find out what the response is!!
If the wrong key is there, you can’t get in

```
## let's mess up one of the keys and see...
## from this:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUS
## to this:
- cluster:
    certificate-authority-data: XS0tLS1CRUdJTiBDRVJUS
## response:
Unable to connect to the server: x509: certificate signed by unknown authority
## and then:
## from this:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBD
## to this:
- name: kubernetes-admin
  user:
    client-certificate-data: XS0tLS1CRUdJTiBD
## response:
error: tls: failed to find any PEM data in certificate input
```

If the wrong IP is there…

```
## let's change the IP to the wrong one:
## from this:      server: https://192.168.0.7:6443
## to this:     server: https://192.168.0.100:6443

## response:
Unable to connect to the server: dial tcp 192.168.0.100:6443: connect: no route to host
```

What if you had a missing config file, or, wrong file path or need to assign a config file from another path manually:

```
## rename the config file and try to access kubectl:
mv config config.bak
kubectl get pods
The connection to the server localhost:8080 was refused - did you specify the right host or port?
## now try it with the kubeconfig flag with the "wrong file".  As long as you specify a working config file you, should work.
kubectl --kubeconfig ./config.bak get pods
NAME                           READY   STATUS    RESTARTS   AGE
busybox                        1/1     Running   0          41h
```

What if you restart the kubelet on the MASTER node!!!?!?
This is semi catastrophic actually, however, check this response out:

```
### do not do this on a dev or production install, ONLY try this on a sandbox install, and be ready to reinstall if this goes south

sudo systemctl stop kubelet
kubectl get pods
##response:
The connection to the server localhost:8080 was refused - did you specify the right host or port?
Now... you can try all kinds of things, but the one that might actually work immediately is this:
sudo systemctl start kubelet
## then issue these commands:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl get pods
##response:
NAME                           READY   STATUS    RESTARTS   AGE
busybox                        1/1     Running   0          42h
```

If you are running kube-apiserver as a container, it’s actually in docker and you can STOP docker and kill the container process… let’s do it and see what happens!!

```
sudo systemctl stop docker
kubectl get pods
## response:
The connection to the server 192.168.0.7:6443 was refused - did you specify the right host or port?
```

It can’t find the API located at port 6443… Hmmm… So this demonstrates that this particular error is related to the API and the docker container running it. So if docker is down, or if anything blocking my access to docker is interrupted, it will throw this type of error.

Let’s bring it back up:

```
sudo systemctl start docker
## give it a few minutes to start all the kubernetes system containers, remember, you don't have any application containers on the control node, usually only kube-system stuff.
## this won't affect your worker nodes or the containers on them, to them it's like a disconnect to the api and nothing more, they will remain functioning as normal unless they need something critical from the api.
kubectl get pods
##response:
NAME                           READY   STATUS    RESTARTS   AGE
busybox                        1/1     Running   0          42h
```

Hopefully you didn’t have to read all the way down to this line, but in case you did, I hope you learned some new tricks with k8s and perhaps some more insight into which connectivity issues stem from which root problems.
