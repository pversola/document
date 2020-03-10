# ETXNPAUATAP01

[Back](../README.md)
## Instancse
Detail | Description
--- | ---
OS | Centos 7
CPU | 4
MEMORY | 8 GB
DISK | 80 GB
HostName | ETXNPAUATAP01
Internal IP | 10.148.1.16
External IP | 139.5.254.128
Visual IP   | --

---

## Storage

No | Name | Types | Size | Mouth
--- | --- | --- | --- | ---

---

## Firewall

No | Service | Protocal | Port | Remote
--- | --- | --- | --- | ---

---

## Imprement

`yum update -y`

### Uninstall old version:  
```
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```

### Setup Repository
1. Install require package.  
`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`

1. Set up the **stable** repository.   
`sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
`

### Install Docker Engine - Comunity
1. Install the latest version of Docker Engine - Community and containerd
`sudo yum install docker-ce docker-ce-cli containerd.io`
1. Start Docker.  
`sudo systemctl start docker`  
1. Enable Docker.  
`sudo systemctl enable docker`  
1. If you use Docker as a **non-root** user.  
`sudo usermod -aG docker <USER NAME>`

### Remove Docker
1. Uninstall the Docker package:  
`sudo yum remove docker-ce`  
1. delete all images, containers, and volumes:  
`sudo rm -rf /var/lib/docker`

### Config registry
```
cat > /etc/docker/daemon.json <<EOF
{
  "metrics-addr" : "0.0.0.0:50000",
  "experimental" : true,
  "insecure-registries" : ["139.5.146.7:5000"]
}
EOF
```
Login registry 139.5.146.7:5000
`docker login -u cwnadmin 139.5.146.7:5000`
---

[Back](../README.md)