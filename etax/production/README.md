## Environment

### Prod
> GMC ip address `172.1.254.206:9961`  
> HSM ip address `172.1.254.204:1792`

---

### -RD
***DC***    
> ip address DNAT `10.255.1.189` => Production  
> ip address DNAT `10.255.1.178` => Test  
> ip address SNAT `10.255.1.183`  

***DR***  
> ip address DNAT `10.255.2.189` => Production  
> ip address DNAT `10.255.2.178` => Test  
> ip address SNAT `10.255.2.183`   

#### How to route table
`vi /etc/sysconfig/network-scripts/route-eth0`  

```
172.1.248.0/24  via 10.255.6.62
172.1.254.0/24  via 10.255.6.62
10.255.1.0/24   via 10.255.6.62
```
---

## Standard Server Name

No | Detail | Lenght 
---| --- | ---
1 | Project Name | 3
2 | Site Name | 3
3 | Environment | 3
4 | Service | 2
5 | Running | 2

Example
 - Project: ETAX --> ETX
 - Site: NIPA CLOUD --> NPA
 - Environment: Develop | UAT | PRODUCTION --> PRD
 - Serveice: Database --> DB
 - Running: 1 --> 01

 Host Name is `ETXNPAPRDDB01` 

---
## Server

No | Host Name | OS | CPU | MEMORY | DISK | IP Addr | EXT IP Addr | Remark
--- | --- | --- | --- | --- | --- | --- | --- | ---
1 | [ETXNPAPRDLB01](./production/ETXNPAPRDLB01.md) | Centos 7 | 2 | 4 GB | 80 GB | 10.148.2.11 | 103.74.255.47 | Proxy
2 | [ETXNPAPRDLB02](./production/ETXNPAPRDLB02.md) | Centos 7 | 2 | 4 GB | 80 GB | 10.148.2.12 | 103.74.255.74 | Proxy
3 | [ETXNPAPRDAP01](./production/ETXNPAUATAP01.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.11 | 103.74.255.161| K8s Master, Worker
4 | [ETXNPAPRDAP02](./production/ETXNPAUATAP02.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.12 | 103.74.255.150 | K8s Master, Worker
5 | [ETXNPAPRDAP03](./production/ETXNPAUATAP03.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.13 | 103.74.255.2 | K8s Worker
6 | [ETXNPAPRDAC01](./production/ETXNPAPRDAC01.md) | Window Server 2016 | 2 | 4 GB | 80 GB | 10.148.3.111 | -- | Web Archive
7 | [ETXNPAPRDFS01](./production/ETXNPAPRDFS01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.11 |-- | File Server
8 | [ETXNPAPRDDB01](./production/ETXNPAUATDB01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 | 103.74.254.82 | MsSQL
9 | [ETXNPAPRDDB02](./production/ETXNPAUATDB02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 | 103.74.254.220 | MsSQL
10 | [ETXNPAPRDBK01](./production/ETXNPAPRDBK01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.111 |-- | Backup
11 | [ETXNPAPRDMT01](./production/ETXNPAPRDMT01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 |-- | Monitor
12 | [ETXNPAPRDLG02](./production/ETXNPAPRDLG02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 |-- | Log


## ALL VM Imperment

### ***How to custom SSH port***
edit sshd config  
`vi /etc/ssh/sshd_config`

```
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
Port <PORTNUMBER>
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

.
.
.
#

MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com

#

Protocol 2
```  


Restart service
`systemctl restart sshd`  

SELinux  
`semanage port -a -t ssh_port_t -p tcp #PORTNUMBER`  

edit firewall config  
`vi /usr/lib/firewalld/services/ssh.xml`  

```
<port protocol="tcp" port="<PORTNUMBER>"/>
```

---

### ***How to enable SNMP***
install packages  
`sudo yum install net-snmp net-snmp-utils -y`  

Enable service  
`sudo systemctl enable snmpd`

Backup config   
`sudo cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.orig`

Edit config  
`sudo vi /etc/snmp/snmpd.conf`
```
#       sec.name  source          community
com2sec notConfigUser  default       <PASSWORD CREDENTIAL>

.
.
.

# Make at least  snmpwalk -v 1 localhost -c public system fast again.
#       name           incl/excl     subtree         mask(optional)
# view    systemview    included   .1.3.6.1.2.1.1
# view    systemview    included   .1.3.6.1.2.1.25.1.1
view    systemview    included   .1

```

password is `Dbam0n_etaxCSP`

Restart service  
`sudo systemctl restart snmpd`

---

### ***How to config nfs server***
#### Server
install package  
`yum install -y nfs-utils`  
enable and start service  
`systemctl start nfs-server`  
`systemctl enable nfs-server`  

`mkdir -p /mnt/nfs/share`  
`chown nfsnobody:nfsnobody /mnt/nfs/share`  
`chmod -R 775 /mnt/nfs/share`

Exporting directory  
`vi /etc/exports`
```
/mnt/nfs/share  *(rw,sync,no_subtree_check,no_root_squash,insecure)
```
`exportfs -rv`

check export mount  
`showmount -e`

#### Client
install package  
`yum install -y nfs-utils`  
enable and start service  
`systemctl start nfs-server`  
`systemctl enable nfs-server` 

check nfs server  
`showmount <IP NFS SERVER>`

mount  
`mount -t nfs <IP NFS SERVER>:<SERVER MOUNT PATH> <LOCAL PATH>`  
check mount status  
`mount | grep mydata`  

Reference:  
https://medium.com/@myte/kubernetes-nfs-and-dynamic-nfs-provisioning-97e2afb8b4a9  
https://medium.com/@thanwa/kubernetes-%E0%B8%97%E0%B8%B3%E0%B8%84%E0%B8%A7%E0%B8%B2%E0%B8%A1%E0%B9%80%E0%B8%82%E0%B9%89%E0%B8%B2%E0%B9%83%E0%B8%88%E0%B9%80%E0%B8%A3%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%87-volume-%E0%B8%81%E0%B8%B1%E0%B8%99%E0%B8%8B%E0%B8%B1%E0%B8%81%E0%B8%AB%E0%B8%99%E0%B9%88%E0%B8%AD%E0%B8%A2-ea6538b50663  

---

### ***How to config insecure registries for Kubernetes***
#### All nodes
Configure insecure registry for Docker (all node) 
Ex. (35.180.127.175 is public ip registries)  
`vi /etc/docker/daemon.json`
```
{
  "insecure-registries": ["139.5.146.7:5000"]
}
```

update docker config  
`systemctl reload docker`  
`systemctl restart docker`

Run docker login  
Ex (Credentioal u:admin p:password)  
`docker login 139.5.146.7:5000`

#### Master all
Create the secret  
`kubectl create secret docker-registry reposecret --docker-server=139.5.146.7:5000 --docker-username=cwnadmin --docker-password=Ch@nwan1ch`

Verify  
`kubectl get secret reposecret --output=yaml`  
or  
`kubectl get secret reposecret --output="jsonpath={.data.\.dockerconfigjson}" | base64 --decode` 

Reference:  
https://medium.com/@alombarte/setting-up-a-local-kubernetes-cluster-with-insecure-registries-f5aaa34851ae  
https://medium.com/@oktayesgul/how-to-configure-goharbor-as-an-insecure-registry-for-kubernetes-e58cd93526a8

:heavy_check_mark:
:x: