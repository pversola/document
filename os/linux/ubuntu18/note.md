# Ubuntu

### Update server

```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo systemctl reboot
```

### How to configure network

`$ sudo netplan generate`  
`$ sudo vi /etc/netplan/*.yaml`

#### Static IP Address

```
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.120.10/24, ]
      gateway4:  192.168.120.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

- **eth0** - network interface name
- **dhcp4** and **dhcp6** - dhcp properties of interfact for IPv4 and IPv6
- **address** - sequence of static address to the interface.
- **gateway4** - IPv4 address for default gateway
- **nameserver** - sequence of IP address of nameserver.

#### Dynamic DHCP IP Address

```
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: yes
      dhcp6: yes
```

Save the file and exit. Then apply the recent network changes using following netplan command.  
`$ sudo netplan apply`

### How to configure filewall

command **ufw**  
`$ sudo ufw status`  
`$ sudo ufw enable`  
`$ sudo ufw allow http`  
`$ sudo ufw allow 22/tcp`  
`$ sudo ufw allow from <IP ADDRESS> to any port 22`

### How to install python

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt update
$ sudo apt install python3.8
$ sudo apt install python3-pip
```

### How to install ansible

You can clear ansible from the cache with:
`hash -d ansible`

```
$ pip3 install ansible
```

### Service

`$ sudo service --status-all` Disply list all service  
`$ sudo service start <SERVICE NAME>`  
`$ sudo service stop <SERVICE NAME>`  
`$ sudo service restart <SERVICE NAME>`

### Base64

```
export Message=""

### Encrypt ###
echo $Message | base64 -w0

### Decrypt ###
echo $Message | base64 -d

unset Message
```

### How to kill Zombie Processes

```
$ ps axo stat,ppid,pid,comm | grep -w defunct
$ sudo kill -9 <PID>
```

## FILE AND DIRECTORY

### Delete old than 30

```
$ find <PATH> -type d -mtime +30 -exec sudo rm -rf {} \;
```

or

```
for p in $(find <PATH> -maxdepth 2 -type d -mtime +30 | grep /output  | awk '{print $1}'); do sudo rm -rf $p; done
```

### Count file in directory

```
find <PATH> -type f | wc -l

find <PATH> -maxdepth 1 | wc -l
```

### Check size

```
$ du -sh <PATH>
$ du -ah <PATH>
$ du -ah -d 1 <PATH>  //
```

for p in $(find /mnt/ebts/shared/jobfiles/jobs -maxdepth 2 -type d -mtime +20 | grep /output | awk '{print $1}'); do sudo rm -rf $p; done
for p in $(find /mnt/ebts/shared/jobfiles/jobs -maxdepth 2 -type d -mtime +20 | grep /working | awk '{print $1}'); do sudo rm -rf $p; done

for p in $(find /mnt/ebts/files/core/SFB/ -maxdepth 1 -type d -mtime +20 | grep /output | awk '{print $1}'); do sudo rm -rf $p; done
