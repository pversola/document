## Uninstall old versions

```
$ sudo apt remove docker docker-engine docker.io containerd runc
```

## Step 1: Setup the repository

Update the `apt` package index and install packages allow `apt` to use repository over HTTPS.

```
$ sudo apt update
$ sudo apt install -y \
  ca-certificates \
  curl \
  gnupg \
  lsb-release
```

Add Docker GPG key.

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Set up the stable repository

```
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Step 2: Install Docker Engine

Update the `apt` package index and install the latest version of Docker Engine and containerd

```
$ sudo apt update
$ sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### Uninstall Docker Engine

```
$ sudo apt purge docker-ce docker-ce-cli containerd.io
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

## Step 3: Post-installation steps for Linux

Manage Docker as a non-root user

```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
```

## Step 4: Install Docker compose V2

```
$ mkdir -p ~/.docker/cli-plugins/
$ curl -SL https://github.com/docker/compose/releases/download/v2.0.1/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
$ chmod +x ~/.docker/cli-plugins/docker-compose
$ docker compose version
```

### Uninstall Docker Compose

```
sudo rm ~/.docker/cli-plugins/docker-compose
```
