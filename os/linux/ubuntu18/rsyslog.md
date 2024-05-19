# rsyslog

1. Config `sudo vi /etc/rsyslog.conf`

```
$ sudo sed -ri '/\smodule(load="imudp"\s/s/^#?/#/' /etc/rsyslog.conf
$ cat <<EOF | sudo tee /etc/rsyslog.d/50-client.conf
##Enable sending of logs over UDP add the following line:

*.* @10.148.5.20:514


##Enable sending of logs over TCP add the following line:

*.* @@10.148.5.20:514

##Set disk queue when rsyslog server will be down:

$ActionQueueFileName queue
$ActionQueueMaxDiskSpace 1g
$ActionQueueSaveOnShutdown on
$ActionQueueType LinkedList
$ActionResumeRetryCount -1
EOF
```

2. Restart service `systemctl restart rsyslog`
