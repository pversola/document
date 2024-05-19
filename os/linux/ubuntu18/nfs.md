# How To Set Up an NFS Mount on Ubuntu

### Prerequisites

- **Host**: 192.168.200.21
- **Client**: 192.168.200.22

### Step 1 — Downloading and Installing the Components

On Host

```
$ sudo apt update
$ sudo apt install nfs-kernel-server -y
```

On Client

```
$ sudo apt update
$ sudo apt install nfs-common -y
```

### Step 2 — Creating the Share Directories on the Host

#### Example 1: Exporting a General Purpose Mount

```
$ sudo mkdir -p /mnt/shared
$ ls -la /mnt/shared
drwxr-xr-x 2 root root 4096 Mar 22 04:26 .

$ sudo chown nobody:nogroup /mnt/shared
```

#### Example 2: Exporting the Home Directory

we’ll export the `/home` directory.

### Step 3 — Configuring the NFS Exports on the Host Server

```
$ sudo vi /etc/exports
/mnt/shared    <Client IP>(rw,sync,no_subtree_check)
/home       <Client IP>(rw,sync,no_root_squash,no_subtree_check)

$ sudo systemctl restart nfs-kernel-server
```

### Step 4 — Adjusting the Firewall on the Host

```
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

```
$ sudo ufw allow from 192.168.200.22 to any port nfs
```

```
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
2049                       ALLOW       192.168.200.22
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

### Step 5 — Creating the Mount Points on the Client

```
$ sudo mkdir -p /mnt/nfs/shared
$ sudo mkdir -p /mnt/nfs/home
```

### Step 6 — Mounting the Directories on the Client

```
$ sudo mount 192.168.200.21:/mnt/shared /mnt/nfs/shared
$ sudo mount 192.168.200.21:/home /mnt/nfs/home
```

```
$ df -h
...
192.168.200.21:/mnt/shared          78G  6.4G   67G   9% /mnt/nfs/shared
192.168.200.21:/home                78G  6.4G   67G   9% /mnt/nfs/home
```

```
$ du -sh /mnt/nfs/home
32K     /mnt/nfs/home
```

### Step 7 — Testing NFS Access

#### Example 1: The General Purpose Share

```
$ sudo touch /mnt/nfs/shared/general.test
$ ls -l /mnt/nfs/shared/general.test
-rw-r--r-- 1 nobody nogroup 0 Mar 22 06:11 /mnt/nfs/shared/general.test
```

#### Example 2: The Home Directory Share

```
$ sudo touch /mnt/nfs/home/file.test
$ ls -l /mnt/nfs/home/file.test
-rw-r--r-- 1 root root 0 Mar 22 06:14 /mnt/nfs/home/file.test
```

### Step 8 — Mounting the Remote NFS Directories at Boot

```
$ sudo vi /etc/fstab
...
192.168.200.21:/mnt/shared  /mnt/nfs/shared nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
192.168.200.21:/home  /mnt/nfs/home nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```

### Step 9 — Unmounting an NFS Remote Share

```
$ cd ~
$ sudo umount /mnt/nfs/shared
$ sudo umount /mnt/nfs/home
```
