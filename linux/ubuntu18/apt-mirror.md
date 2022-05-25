# How to install local repository

## Setup Server Side

```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo systemctl reboot
```

```
$ sudo apt update
$ sudo apt install apache2 apt-mirror
```

### Configuring APT-Mirror

#### Mirror List

| #   | Mirror                        | Protocal                | URL                          |
| --- | ----------------------------- | ----------------------- | ---------------------------- |
| 1   | Khon Kaen University          | http, https, ftp, rsync | mirror.kku.ac.th/ubuntu      |
| 2   | TOT Internet Data Center      | http, rsync             | mirror1.totbb.net/ubuntu     |
| 3   | Bangmod Enterprise Co., Ltd.  | http                    | mirrors.bangmod.cloud/ubuntu |
| 4   | NIPA.CLOUD 1st Cloud Thailand | http, ftp, rsync        | mirrors.nipa.cloud/ubuntu    |

#### UBUNTU Version List

| #   | Name   | Version |     |
| --- | ------ | ------- | --- |
| 1   | Xenial | 16.04   | --- |
| 2   | Bionic | 18.04   | --- |
| 3   | Focal  | 20.04   | --- |

Ex.

```
deb http://mirror.kku.ac.th/ubuntu/ <UBUNTU_VERSION> main
deb-src http://mirror.kku.ac.th/ubuntu/ <UBUNTU_VERSION> main
```

Backup original mirror.list  
`$ sudo mv /etc/apt/mirror.list /etc/apt/mirror.list.bak`

Create new mirror.list  
`$ sudo vi /etc/apt/mirror.list`

```
############# config ##################
#
set base_path    /linuxide
#
# set mirror_path  $base_path/mirror
# set skel_path    $base_path/skel
# set var_path     $base_path/var
# set cleanscript $var_path/clean.sh
# set defaultarch  <running host architecture>
# set postmirror_script $var_path/postmirror.sh
# set run_postmirror 0
set nthreads     20
set _tilde 0
#
############# end config ##############


deb http://mirror.kku.ac.th/ubuntu focal main restricted universe multiverse
deb http://mirror.kku.ac.th/ubuntu focal-security main restricted universe multiverse
deb http://mirror.kku.ac.th/ubuntu focal-updates main restricted universe multiverse
deb http://mirror.kku.ac.th/ubuntu focal-proposed main restricted universe multiverse
deb http://mirror.kku.ac.th/ubuntu focal-backports main restricted universe multiverse

#deb-src http://mirror.kku.ac.th/ubuntu focal main restricted universe multiverse
#deb-src http://mirror.kku.ac.th/ubuntu focal-security main restricted universe multiverse
#deb-src http://mirror.kku.ac.th/ubuntu focal-updates main restricted universe multiverse
#deb-src http://archive.ubuntu.com/ubuntu artful-proposed main restricted universe multiverse
#deb-src http://archive.ubuntu.com/ubuntu artful-backports main restricted universe multiverse

clean http://mirror.kku.ac.th/ubuntu/
```

#### Fix bug apt mirror (^ UBUNTU 20.4)

`$ sudo vi /bin/apt-mirror`

```
## Goto line 451 add new line
+ add_url_to_download( $url . $_ . "/cnf/Commands-" . $arch . ".xz" );

## Got line 488 edit line
- $uri =~ s/^([^@]+)?@?// if $uri =~ /@/;
+ #$uri =~ s/^([^@]+)?@?// if $uri =~ /@/;
+ $uri =~ s/^([^@]+)?@?// if (split '/',$uri)[0] =~ /@/;

## Goto line 525 edit line
- if ( $filename =~ m{^$component/i18n/Translation-[^./]*\.bz2$} )
+ if ( $filename =~ m{^$component/i18n/Translation-[^./]*\.(bz2|xz)$} )
```

run apt-mirror which will now get/mirror all the packages in the repository.  
`$ sudo apt-mirror`

### Configuring Web Server

```
$ sudo ln -s /linoxide /var/www/ubuntu
$ sudo ln -s /linuxide/mirror/mirror.kku.ac.th/ubuntu /var/www/html/ubuntu
$ sudo service apache2 start
```

### Configuring automatic sync process by using cron scheduler.

```
$ sudo crontab -e
00 	02 	*	 *	 *	/usr/bin/apt-mirror
```

## Configuring Client Side

```
$ sudo mv /etc/apt/sources.list /etc/apt/sources.list.bak
$ sudo vi /etc/apt/sources.list
```

```
deb http://192.168.200.10/ubuntu focal main restricted universe multiverse
deb http://192.168.200.10/ubuntu focal-updates main restricted universe multiverse
deb http://192.168.200.10/ubuntu focal-security main restricted universe multiverse
```

or

```
$ sudo add-apt-repository \
   "deb http://192.168.200.10/ubuntu \
   $(lsb_release -cs) \
   main \
   restricted \
   universe \
   multiverse"

$ sudo add-apt-repository \
   "deb http://192.168.200.10/ubuntu \
   $(lsb_release -cs)-security \
   main \
   restricted \
   universe \
   multiverse"

$ sudo add-apt-repository \
   "deb http://192.168.200.10/ubuntu \
   $(lsb_release -cs)-updates \
   main \
   restricted \
   universe \
   multiverse"

$ sudo add-apt-repository \
   "deb http://192.168.200.10/ubuntu \
   $(lsb_release -cs)-proposed \
   main \
   restricted \
   universe \
   multiverse"

$ sudo add-apt-repository \
   "deb http://192.168.200.10/ubuntu \
   $(lsb_release -cs)-backports \
   main \
   restricted \
   universe \
   multiverse"
```

```
$ sudo apt update
$ sudo apt upgrade -y
```
