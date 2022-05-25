# How to Install NTP Server and Client(s) on Ubuntu 20.04 LTS

## Install and configure NTP Server

### Step 1: Update repository index

```
$ sudo apt update

```

```
### check time local ###
$ timedatectl

### Check time Hardware Clock ###
$ sudo hwclock
```

```
$ sudo service ntp stop
$ ntpdate <SERVER>
$ sudo service ntp start
```

```
### Set the System Time from the Hardware Clock. ###
$ sudo hwclock --hctosys

### Set the Hardware Clock to the current System Time. ###
$ sudo hwclock --systohc
```

### Chrony NTP Server

```
$ sudo apt install chrony -y
$ sudo vi /etc/chrony.conf
```

### Timezone

```
timedatectl set-timezone Asia/Bangkok
```
