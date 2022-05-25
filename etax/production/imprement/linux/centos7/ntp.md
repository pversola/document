# NTP

**_Client_**

1. config ntp server `vi /etc/ntp.conf`
   ```
   # Use public servers from the pool.ntp.org project.
   # Please consider joining the pool (http://www.pool.ntp.org/join.html).
   #server 0.centos.pool.ntp.org iburst
   #server 1.centos.pool.ntp.org iburst
   #server 2.centos.pool.ntp.org iburst
   #server 3.centos.pool.ntp.org iburst
   server <IP NTP SERVER> iburst
   ```
1. restart service
   `systemctl restart ntpd`
1. Check ntp with command `ntpq -p`

## How to force a clock update using ntp?

```
$ sudo systemctl stop ntpd
$ ntpdate -s 10.255.6.62
$ sudo systemctl start ntpd
```

```
$ hwclock
$ ntpstat
$ timedatectl

$ timedatectl set-ntp false
$ timedatectl set-time "2021-06-15 10:04:00"
$ timedatectl set-ntp true
```
