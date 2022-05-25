# ETXNPAPRDLB01

## Imprement

### ***Install MS SQL Server 2019*** 
`sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo`  

`sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo`

`sudo yum install -y mssql-server`  

`sudo /opt/mssql/bin/mssql-conf setup`  

`sudo systemctl status mssql-server`  

`sudo firewall-cmd --permanent --add-port=1433/tcp `  
`sudo firewall-cmd --permanent --add-service=high-availability`  
`sudo firewall-cmd --reload`

 
`sudo yum remove mssql-tools unixODBC-utf16 unixODBC-utf16-devel`  
`sudo yum install -y mssql-tools unixODBC-devel`

Add `/opt/mssql-tools/bin/` to your PATH environment variable in a bash shell.   
`echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile`  
`source ~/.bash_profile`  
or  
`echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc`  
`source ~/.bashrc`  

Change default SQL Server TCP Port  
`sudo /opt/mssql/bin/mssql-conf set network.tcpport 33411`  
`sudo systemctl restart mssql-server`

Change Collection   
`sudo systemctl stop mssql-server`  
`sudo /opt/mssql/bin/mssql-conf set-collation`  
**THAI_CI_AS**  
`sudo systemctl start mssql-server`  

Enable MsSQL agent  
`sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true`  
`sudo systemctl restart mssql-server`

Enable the availability groups feature  
`sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1`  
`sudo systemctl restart mssql-server`


### ***Install the high availability add-on***
all node  
`sudo yum install -y pacemaker pcs fence-agents-all resource-agents`  
`sudo setsebool -P daemons_enable_cluster_mode 1`  
`sudo passwd hacluster`  
`sudo systemctl enable pcsd`  
`sudo systemctl start pcsd`   
`sudo systemctl enable pacemaker`  
`sudo yum install -y mssql-server-ha`

echo '10.148.4.21 node1' >> /etc/hosts

primary node  
`sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>`  
`sudo pcs cluster setup --name ha_cluster <node1> <node2> <node3>`  
`sudo pcs cluster start --all`  
`sudo pcs cluster enable --all`  

in SQL server primary node
 ```
 /* Create master key */
 CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Q2hAbndhbjFjaENTUA==';

 /* Create certificate */
CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';
GO

/* Backup certificate */
BACKUP CERTIFICATE dbm_certificate
   TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
   WITH PRIVATE KEY (
	FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
	ENCRYPTION BY PASSWORD = 'U3lzQGRtMW4='
   );
 ```

`cd /var/opt/mssql/data`   
`scp dbm_certificate.* root@<NODE>:/var/opt/mssql/data/`

Other node  
`cd /var/opt/mssql/data`   
`chown mssql:mssql dbm_certificate.*` 
```
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Q2hAbndhbjFjaENTUA==';

CREATE CERTIFICATE dbm_certificate
    FROM FILE = '/var/opt/mssql/data/dbm_certificate.cer'
    WITH PRIVATE KEY (
		FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
		DECRYPTION BY PASSWORD = 'U3lzQGRtMW4='
	);
```

All node  
`echo 'pacemakerLogin' >> ~/pacemaker-passwd`  
`echo 'cGFjZW1ha2VyTG9naW4=' >> ~/pacemaker-passwd`  
`sudo mv ~/pacemaker-passwd /var/opt/mssql/secrets/passwd`  
`sudo chown root:root /var/opt/mssql/secrets/passwd`

Only readable by root  
`sudo chmod 400 /var/opt/mssql/secrets/passwd`  
  
Edit config all node  
`sudo vi /usr/lib/ocf/resource.d/mssql/ag`

```
"${PORT_DEFAULT=<CUSTOM PORT>}"
```
`pcs property set no-quorum-policy=ignore`  
`pcs property set stonith-enabled=false`  
``  

`sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true`  


`sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=10.148.4.20 cidr_netmask=24 nic=eth0 op monitor interval=30s`  

`sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=10.148.4.20 cidr_netmask=24 op monitor interval=30s`  

`sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master`  

`sudo pcs constraint order promote ag_cluster-master then start virtualip`

```
for i in pcsd pacemaker
do
sudo systemctl enable $i;
sudo systemctl start $i;
sudo systemctl status $i;
done
```

connect   
`ApplicationIntent=ReadOnly`



### ***How to enable SNMP***
install packages  
`sudo yum install net-snmp net-snmp-utils`  

Enable service  
`sudo systemctl enable snmpd`

Backup config   
`sudo cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.orig`

Edit config  
`sudo vi /etc/snmp/snmpd.conf`
```
#       sec.name  source          community
com2sec notConfigUser  default       <PASSWORD CREDENTIAL>

---

# Make at least  snmpwalk -v 1 localhost -c public system fast again.
#       name           incl/excl     subtree         mask(optional)
view    systemview    included   .1.3.6.1.2.1.1
view    systemview    included   .1.3.6.1.2.1.25.1.1
view    systemview    included   .1

```

password is `dbamon_etax`

Restart service  
`sudo systemctl restart snmpd`