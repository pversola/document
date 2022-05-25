# SNMP

## How to install and config

### CentOS 7

- install snmp packages  
  `sudo yum install net-snmp net-snmp-utils -y`

- Enable service  
  `sudo systemctl enable snmpd`

- Backup config  
  `sudo cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.bak`

- Edit config (ex. password is `Dbam0n_etaxCSP`)  
  `sudo vi /etc/snmp/snmpd.conf`

```
#####
## Edit comunity use own password
####

#       sec.name  source          community
com2sec notConfigUser  default       Dbam0n_etaxCSP

---

#####
## Disable group v1
####

#       groupName      securityModel securityName
# group   notConfigGroup v1           notConfigUser
group   notConfigGroup v2c           notConfigUser

---

#####
## Disable old view
## Add new view use subtree .1
#####

# Make at least  snmpwalk -v 1 localhost -c public system fast again.
#       name           incl/excl     subtree         mask(optional)
# view    systemview    included   .1.3.6.1.2.1.1
# view    systemview    included   .1.3.6.1.2.1.25.1.1
view    systemview    included   .1

```

- Restart service  
  `sudo systemctl restart snmpd`

---

### Ubuntu

```
$ sudo apt -y install snmpd snmp libsnmp-dev
$ sudo cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.bak
$ sudo vi /etc/snmp/snmpd.conf
```

```
...
#  AGENT BEHAVIOUR
#

#  Listen for connections from the local system only
agentAddress udp:127.0.0.1:161,udp:192.168.43.154:161
#  Listen for connections on all interfaces (both IPv4 *and* IPv6)
#agentAddress udp:161,udp6:[::1]:161
...
###############################################################################
#
#  ACCESS CONTROL
#
...
rocommunity Ex@mPL3 192.168.43.101

###############################################################################
#
# SYSTEM INFORMATION
```

```
$ sudo systemctl reload snmpd
$ netstat -nlpu|grep snmp
```

---

password is `Dbam0n_etaxCSP` To base64 `RGJhbTBuX2V0YXhDU1A=`
