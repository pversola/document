# GlusterFS

# Install

First provision 3 virtual machine with Ubuntu 20.04

| Hostname | IP address     | CPU | Memory | Remark                   |
| -------- | -------------- | --- | ------ | ------------------------ |
| node1    | 192.168.200.31 | 2   | 4      | Kubernetes control plane |
| node2    | 192.168.200.32 | 2   | 4      | Kubernetes control plane |
| node3    | 192.168.200.33 | 2   | 4      | Kubernetes control plane |

### Step 1: Update Ubuntu Server (On all nodes)

```
$ sudo apt update
$ sudo apt -y upgrade && sudo systemctl reboot
```

Fix Hosts (All hosts)

```
sudo -i
echo '192.168.200.31 node1' >> /etc/hosts
echo '192.168.200.32 node2' >> /etc/hosts
echo '192.168.200.33 node3' >> /etc/hosts
```

### bricks (all nodes)

```
$ sudo mkdir -p /gtfs/vms
```

create and format partition

```
$ fdisk /dev/sdb
$ mkfs.xfs -f -i size=512 /dev/sdb1
```

mount

```
$ sudo vi /etc/fstab
/dev/sdb1 /gtfs/vms xfs defaults 0 0


$ mount -arw
```

```
$ sudo apt -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
    net-tools \
    telnet

$ sudo add-apt-repository ppa:gluster/glusterfs-9

$ sudo apt update
$ sudo apt -y install glusterfs-server
```

check

```
$ sudo systemctl enable glusterd --now
$ sudo gluster help
$ sudo gluster volume info
```

node1

```
$ sudo gluster peer probe node2
$ sudo gluster peer probe node3
```

check status

```
$ gluster peer status
```
