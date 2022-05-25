# ETXNPAUATDB01

[Back](../README.md)
## Instancse
Detail | Description
--- | ---
OS | Centos 7
CPU | 4
MEMORY | 8 GB
DISK | 120 GB
HostName | ETXNPAUATDB01
Internal IP | 10.148.1.17
External IP | 139.5.254.43
Visual IP   | --

---

## Storage

No | Name | Types | Size | Mouth
--- | --- | --- | --- | ---
1 | ETXNPAUATDB01_MSSQL | Corporate | 150 GB | /mssql

---

## Firewall

No | Service | Protocal | Port | Remote
--- | --- | --- | --- | ---
1 | SSH | tcp | 22 | 0.0.0.0/0
2 | PING | icmp | 0 (type) - All (code) | 0.0.0.0/0
2 | PING | icmp | 8 (type) - All (code) | 0.0.0.0/0
2 | PING | icmp | 11 (type) - All (code) | 0.0.0.0/0
3 | MsSQL | tcp | 33411 | 0.0.0.0/0

---
## Imprement
### **Install MSSQL Server 2019 on CentOS 7**
## 1. Prepare envinronment
1. Prepare OS:<br>
  `sudo yum install epel-release -y`<br>
  `sudo yum update -y`
    
## 2. Install sql server and Tool
1. Download the Microsoft SQL Server 2019 CentOS 7 repository configuration file:<br>
  `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo`
    
2. Run the following commands to install SQL Server:<br>
  `sudo yum install -y mssql-server`
    
3. After the package installation finishes, run mssql-conf setup and follow the prompts to set the SA password and choose your edition.<br>
  `sudo /opt/mssql/bin/mssql-conf setup`<br>
    
4. Once the configuration is done, verify that the service is running:<br>
  `sudo systemctl status mssql-server`

5. Download the Tools Microsoft SQL Server 2019 CentOS 7 repository configuration file:<br>
  `sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo`

6. If you had a previous version of mssql-tools installed, remove any older unixODBC packages.<br>
  `sudo yum remove unixODBC-utf16 unixODBC-utf16-devel`
  
7. Run the following commands to install mssql-tools with the unixODBC developer package.<br>
  `sudo yum install -y mssql-tools unixODBC-devel`
 
8. For convenience, add `/opt/mssql-tools/bin/` to your PATH environment variable. This enables you to run the tools without specifying the full path. Run the following commands to modify the PATH for both login sessions and interactive/non-login sessions:<br>
  `echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile`<br>
  `echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc`<br>
  `source ~/.bashrc`

## 3. Config parameter MSSQL
### 3.1. Config on CentOS
1. Stop service.<br>
    `sudo systemctl stop mssql-server`

2. Change the SQL Server collation.<br>
    `sudo /opt/mssql/bin/mssql-conf set-collation`<br>
  Enter collation: `THAI_CI_AS`

3. Configure customer feedback.<br>
    `sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false`

4. Configure MSDTC(port).<br>
    `sudo /opt/mssql/bin/mssql-conf set network.tcpport 33411`

5. Enable SQL Server Agent.<br>
    `sudo yum update mssql-server-agent`<br>
    `sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true`

### 3.2. Config on Instance MSSQL Server
login to instance and run the following commands to modify parameter.<br>
```
    sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  
    sp_configure 'Agent XPs', 1;  
    GO  
    RECONFIGURE  
    GO
    EXEC sys.sp_configure N'max server memory (MB)', N'4096'
    GO
    EXEC sys.sp_configure N'backup compression default', N'1'
    GO
    EXEC sys.sp_configure N'cost threshold for parallelism', N'60'
    GO
    EXEC sys.sp_configure N'max degree of parallelism', N'4'
    GO
    RECONFIGURE WITH OVERRIDE
    GO
```
---

[Back](../README.md)