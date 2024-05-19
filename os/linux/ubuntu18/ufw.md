## Application Profile

```
$ cat <<EOF | sudo tee /etc/ufw/applications.d/openssh-server
[OpenSSH]
title=Secure shell server, an rshd replacement
description=OpenSSH is a free implementation of the Secure Shell protocol.
ports=42200/tcp
EOF


$ cat <<EOF | sudo tee /etc/ufw/application.d/kube-master
[KubeMaster]
EOF
```

## How to deny ICMP

```
$ sudo sed -i '/ufw-before-input.*icmp/s/ACCEPT/DROP/g' /etc/ufw/before.rules
```
