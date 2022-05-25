# Install Docker Engine

### Uninstall old version

**_Centos 7_**

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

**_Cenos 8 or Fedora_**

```
$ sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

**_Ubuntu_**

```
$ sudo apt remove docker \
                  docker-engine \
                  docker.io \
                  containerd \
                  runc
```

### Setup repository

**_Centos 7_**

```
$ sudo yum -y install yum-utils device-mapper-persistent-data lvm2

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

**_Cenos 8 or Fedora_**

```
$ sudo dnf -y install dnf-plugins-core

$ sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

**_Ubuntu_**  
Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```
$ sudo apt update
$ sudo apt -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Add Docker’s official GPG key:

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Verify that you now have the key with the fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

```
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### INSTALL DOCKER ENGINE

**_Centos 7_**

```
$ sudo yum -y install docker-ce docker-ce-cli containerd.io
```

**_Cenos 8 or Fedora_**

```
$ sudo dnf -y install \
  https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.10-3.2.el7.x86_64.rpm

$ sudo dnf -y install docker-ce docker-ce-cli
```

**_Ubuntu_**

```
$ sudo apt update
$ sudo apt -y install docker-ce docker-ce-cli containerd.io
```

install **_Specific version_**

> version string (2nd column) starting at the first colon (:), up to the first hyphen, separated by a hyphen (-)

**_Centos 7_**

```
$ yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64            3:19.03.3-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.2-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.1-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.12-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.2.ce-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.1.ce-3.el7                   docker-ce-stable

$ sudo yum -y install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

**_Cenos 8 or Fedora_**

```
$ sudo dnf list docker-ce --showduplicates | sort -r

docker-ce.x86_64            3:19.03.3-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.2-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.1-3.el7                    docker-ce-stable
docker-ce.x86_64            3:19.03.12-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.2.ce-3.el7                   docker-ce-stable
docker-ce.x86_64            18.06.1.ce-3.el7                   docker-ce-stable


$ sudo dnf -y install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING>
```

**_Ubuntu_**

```
$ apt-cache madison docker-ce
  docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.1~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.0~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  ...

$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

### Post Install

```
$ sudo usermod -aG docker <USERNAME>

$ sudo chmod 666 /var/run/docker.sock

$ sudo systemctl start docker

$ sudo systemctl enable docker
```

### Install Docker Compose

```
$ sudo curl -L \
  "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/bin/docker-compose

$ sudo chmod +x /usr/local/bin/docker-compose
```

---

### TIP 1: Configuring proxy for docker

```
$ sudo vi /etc/hosts
34.228.211.243 registry-1.docker.io/v2/
```

```
$ sudo mkdir -p /etc/systemd/system/docker.service.d
$ sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://wsus:wsusupdate@192.168.1.2:8080"
Environment="HTTPS_PROXY=http://wsus:wsusupdate@192.168.1.2:8080"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
```

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```
