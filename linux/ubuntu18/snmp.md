# SNMP

## How to install and config

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