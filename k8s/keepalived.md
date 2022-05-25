Install Required Package

```
sudo apt update
sudo apt install -y linux-headers-$(uname -r)
```

Install Keepalived and haproxy

```
sudo apt -y install keepalived haproxy
```

Config Keepalived on Primary Node

```
$ cat <<EOF | sudo tee /etc/keepalived/keepalived.conf
vrrp_script haproxy-check {
    script "killall -0 haproxy"
    interval 2
}

vrrp_instance VI_1 {
    interface eth0
    state MASTER
    priority 200

    virtual_router_id 33
    advert_int 1

    unicast_src_ip <Primary IP Address>
    unicast_peer {
        <Secondary IP Address>
    }

    virtual_ipaddress {
        <Virtual IP Address>
    }

    authentication {
        auth_type PASS
        auth_pass 1234
    }

    track_script {
        haproxy-check
    }
}
EOF
```

Config Keepalived on Secondary Node

```
$ cat <<EOF | sudo tee /etc/keepalived/keepalived.conf
vrrp_script haproxy-check {
    script "killall -0 haproxy"
    interval 2
}

vrrp_instance VI_1 {
    interface eth0
    state BACKUP
    priority 100

    virtual_router_id 33
    advert_int 1

    unicast_src_ip <Secondary IP Address>
    unicast_peer {
        <Primary IP Address>
    }

    virtual_ipaddress {
        <Virtual IP Address>
    }

    authentication {
        auth_type PASS
        auth_pass 1234
    }

    track_script {
        haproxy-check
    }
}
EOF
```

Start KeepAlived Service

```
$ sudo service keepalived start
```

Check Virtual IPs

```
$ sudo ip addr show eth0

```
